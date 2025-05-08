# **기술 도입 배경 및 적용 사례: AWS Lambda**

## **0. 개요**

이 문서는 AWS Lambda를 도입하게 된 배경과 선택 이유, 실제 적용 방식을 설명합니다.

이를 통해 독자는 Lambda를 **왜**, **언제**, **어떻게** 사용할지 판단하는 데 참고할 수 있습니다.

---

## **1. 기술 도입 배경**

- 기존에는 EC2 인스턴스나 수동 스크립트를 사용해 이메일 발송 등의 작업을 처리했습니다. 그러나 서버를 항상 실행해야 했고, 실제 작업은 짧은 시간만 수행되더라도 지속적인 비용이 발생했습니다.
    
- 프로젝트 요구사항 중 하나는 **매일 정해진 시간에 자동으로** NBA 뉴스레터를 발송하는 것이었고, 이를 서버리스 방식으로 처리하고자 했습니다.

- 또한, **AWS 서비스에 대한 이해와 실습 경험을 쌓는 것**도 중요한 목표 중 하나였습니다. Lambda, EventBridge, SQS 등 다양한 서비스를 실제 업무에 적용해보며 학습 효과를 높이고자 했습니다.

---

## **2. 선택한 기술 개요**

- **AWS Lambda**는 이벤트 기반으로 동작하는 서버리스 컴퓨팅 서비스입니다.
    
- 인프라를 직접 관리하지 않고도, 코드 업로드 후 S3, SQS, 스케줄 등 다양한 이벤트를 트리거로 실행할 수 있습니다.
    
- 자동 확장, 비용 최적화, AWS 서비스와의 높은 연동성이 주요 장점입니다.

---

## **3. 기술 선택 이유 및 대안 비교**

| **옵션**      | **설명**                 | **제외한 이유**                           |
| ----------- | ---------------------- | ------------------------------------ |
| EC2 + cron  | EC2에서 cron으로 주기적 작업 수행 | 항상 인스턴스가 실행되며, 유휴 시간에도 비용 발생         |
| AWS Fargate | 컨테이너 기반 서버리스 실행        | 초기 설정이 복잡하고, 단순·짧은 주기의 작업에는 과도한 구조   |
| **Lambda**  | 코드 업로드 후 이벤트 기반으로 실행   | 비용 효율적이며 구성 간단, 다양한 AWS 서비스와의 연동이 용이 |
**Lambda 선택 이유 요약**

- 특정 시간에 정해진 작업을 실행하는 구조에 적합
    
- 서버나 컨테이너 관리가 필요 없음
    
- SQS, EventBridge, CloudWatch 등과의 통합이 용이

--- 

## **4. 적용 방식 및 실제 사용 예**

- nbaNewsletterProducer Lambda는 매일 15시에 EventBridge에 의해 실행되며, DynamoDB에서 구독자 목록을 조회한 후 이메일 발송 요청을 SQS에 전송합니다.
    
- nbaNewsletterConsumer Lambda는 SQS 메시지를 수신해 Gmail SMTP를 통해 HTML 뉴스레터를 발송합니다.
    
- Lambda 함수의 배포 및 설정은 Terraform으로 자동화되어 있습니다.

### **디렉토리 구조 예시**

```
lambda/
├── producer/
│   ├── producer_lambda.py
│   └── requirements.txt
├── consumer/
│   ├── consumer_lambda.py
│   └── requirements.txt
```
### **Terraform을 통한 Lambda 생성 예시**

```hcl
resource "aws_lambda_function" "producer" {
  function_name = "nbaNewsletterProducer"
  handler       = "producer_lambda.lambda_handler"
  runtime       = "python3.11"
  role          = aws_iam_role.lambda_exec.arn

  filename         = "${path.module}/../lambda/producer/producer_lambda.zip"
  source_code_hash = filebase64sha256("${path.module}/../lambda/producer/producer_lambda.zip")

  environment {
    variables = {
      SQS_QUEUE_URL = aws_sqs_queue.newsletter_queue.url
    }
  }

  timeout = 15
}
```
---- 
## **5. 회고 및 사용 후기**

- 기대했던 자동화 효과를 충분히 얻었으며, 서버리스 전환을 통해 인프라 관리 부담이 크게 줄었습니다.
    
- 단점으로는, 모든 구독자에게 메일을 보내는 작업을 하나의 Lambda에서 처리하기에는 시간과 메모리 제약이 있어, SQS를 활용한 병렬 처리가 필요했습니다.

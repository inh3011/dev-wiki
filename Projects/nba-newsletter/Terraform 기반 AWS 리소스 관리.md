---
tags:
  - project
  - documentation
  - Terraform
created: 2025-05-02
---
# 기술 도입 배경 및 적용 사례: Terraform 기반 AWS 리소스 관리

## 0. 개요

이 문서는 Terraform을 사용해 구성한 AWS 기반 뉴스레터 발송 시스템 인프라(`main.tf`)의 도입 배경, 선택 이유, 적용 방식을 설명합니다.  
IaC(Infrastructure as Code)를 통해 인프라를 코드로 정의하고 관리함으로써 구성의 일관성과 재현 가능성을 확보할 수 있었습니다.

---

## 1. 기술 도입 배경

- 기존에는 AWS 리소스를 수동으로 구성해 반복 작업이 많고, 설정의 일관성을 유지하기 어려웠음  
- 개발(dev), 운영(prod) 환경 간 동일한 인프라 구성이 필요했고, 변경 이력의 버전 관리 및 협업 수요가 증가  
- AWS 콘솔을 통한 수동 설정은 실수 가능성이 높으며, 배포 자동화 및 CI/CD 연계에 한계가 있었음  
- AWS CLI로 실행한 명령어들을 기록해 변경 이력을 남기고 싶었으나, 수작업으로는 어려웠음

---

## 2. 선택한 기술 개요

- **Terraform**: HashiCorp에서 제공하는 오픈소스 IaC 도구로, 선언형 문법을 사용해 AWS 리소스를 코드로 정의하고 배포  
- 주요 사용 기능:
  - `aws_lambda_function`, `aws_sqs_queue`, `aws_cloudwatch_event_rule` 등 다양한 리소스를 선언형으로 정의  
  - 환경 변수, 로컬 값(`local`), 변수(`var`), 태그(`tag`) 등을 활용해 코드의 재사용성과 가독성 향상  
  - `depends_on`, `lifecycle` 설정으로 리소스 간 의존성 및 배포 전략을 효과적으로 관리

---

## 3. 기술 선택 이유 및 대안 비교

### 선택 이유

- AWS 공식 provider를 통해 높은 호환성과 신뢰성 확보  
- 선언형 문법으로 인프라 상태를 명확히 표현 가능  
- `plan → apply` 단계에서 변경 내용을 사전 검토할 수 있어 안전한 배포 가능  
- Git을 활용한 코드 버전 관리 및 리뷰 기반 협업 가능

### 대안 비교

- **AWS CloudFormation**: AWS 네이티브 도구로 안정적이지만, YAML/JSON 문법이 복잡하고 재사용성이 낮음  
- **Pulumi**: TypeScript, Python 등으로 리소스를 정의할 수 있으나, Terraform에 비해 문서와 커뮤니티 지원이 부족함

---

## 4. 적용 방식 및 사용 예시

- Lambda, SQS, CloudWatch Event Rule 등을 각각 `resource` 블록으로 정의  
- IAM 역할과 정책을 정의하고, `aws_iam_role_policy_attachment`로 Lambda에 연결  
- `aws_lambda_event_source_mapping`을 사용해 SQS 큐와 Lambda 간 트리거 구성  
- `aws_cloudwatch_metric_alarm`을 설정해 Lambda 오류에 대한 알람 구성  
- `.zip` 기반 Lambda 배포와 `lifecycle.ignore_changes` 설정으로 불필요한 재배포 방지  
- 환경별 태그 및 공통 변수를 적용해 코드 재사용성과 유지 보수성 향상

---

## 5. 회고 및 사용 후기

- AWS에만 종속된 배포 환경이라면 **AWS CloudFormation**이 더 적합할 수 있음  
- Terraform 도입 이후, 리소스 변경 시 사전 검토가 가능해져 인프라 안정성이 향상됨  
- 코드 기반 관리로 협업 및 코드 리뷰가 가능해졌으며, 배포 자동화에도 쉽게 통합됨  
- 단점으로는 초기 학습 곡선이 있고, `plan`/`apply` 시 예상치 못한 변경(diff) 관리가 필요했음  
- 향후 Terraform 모듈화를 통해 구성 요소의 재사용성과 유지 보수성을 더욱 강화할 예정
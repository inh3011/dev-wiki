# Terraform

## 개요

Terraform은 인프라를 코드로 관리할 수 있게 해주는 오픈소스 **IaC(Infrastructure as Code)** 도구입니다.  
클라우드 리소스를 선언형 언어(HCL)로 정의해 AWS, GCP, Azure 등 다양한 인프라를 자동으로 프로비저닝하고 관리할 수 있습니다.  
이 문서는 Terraform의 핵심 원리, 장단점, 활용 사례를 설명합니다.

---

## 1. 개념 정의

- Terraform은 **HashiCorp**에서 개발한 IaC 도구로, 인프라 리소스를 코드로 정의해 일관성 있게 배포하고 관리할 수 있습니다.
- 반복적이고 오류가 발생하기 쉬운 수동 설정 과정을 줄이고, 인프라를 **버전 관리**, **자동화**, **복원 가능**하게 만듭니다.
- 주요 유사 도구로는 **Pulumi**, **AWS CloudFormation**, **Ansible**이 있으며, Terraform은 특정 클라우드에 종속되지 않는 멀티클라우드 지원이 강점입니다.

---

## 2. 동작 원리

- `.tf` 파일에서 **HCL(HashiCorp Configuration Language)**을 사용해 리소스를 선언합니다.
- 기본 작업 흐름:
  1. `terraform init` – provider 플러그인 설치
  2. `terraform plan` – 변경 사항 사전 확인
  3. `terraform apply` – 리소스 생성 또는 수정
  4. `terraform destroy` – 리소스 제거
- Terraform은 `.tfstate` 파일에 현재 인프라 상태를 저장하며, 이 파일은 상태 관리의 핵심입니다.  
  협업 시에는 원격 상태 저장소(S3, GCS 등)와 locking 기능을 함께 사용하는 것이 권장됩니다.

---

## 3. 장점과 한계

### 장점

- AWS, GCP, Azure 등 다양한 클라우드를 하나의 도구로 관리 가능  
- 선언형 문법으로 리소스를 명확하게 정의  
- 상태 파일 기반으로 변경 사항을 감지하고 계획적으로 배포  
- 모듈화를 통한 구성 재사용  
- 활발한 커뮤니티와 에코시스템

### 한계

- 상태 파일 충돌 또는 누락 등으로 인한 관리 부담  
- 복잡한 조건이나 로직 처리에는 한계 (선언형 특성)  
- 리소스 간 의존성 문제로 인해 예기치 않은 변경 발생 가능  
- provider 버전에 따라 기능 지원이 다를 수 있음

---

## 4. 관련 기술 비교

| 항목      | Terraform     | AWS CloudFormation | Ansible     |
| ------- | ------------- | ------------------ | ----------- |
| 언어      | HCL           | JSON/YAML          | YAML        |
| 실행 방식   | 선언형           | 선언형                | 절차형         |
| 클라우드 지원 | 멀티클라우드        | AWS 전용             | 멀티클라우드(간접적) |
| 상태 관리   | `.tfstate` 사용 | 내부 상태 관리           | 상태 저장 없음    |
| 학습 곡선   | 비교적 쉬움        | 다소 높음              | 다소 높음       |

---

## 5. 활용 시점과 예시

### 적용 시점

- 수동 설정 없이 클라우드 인프라를 자동화하고자 할 때  
- 인프라 변경 이력을 코드로 관리하려 할 때  
- DevOps 환경에서 CI/CD 파이프라인과 인프라를 통합할 때

### 사용 예시

- AWS VPC, EC2, RDS 등 리소스 자동 배포  
- GitHub Actions 또는 Jenkins와 연계한 자동화  
- 개발/스테이징/운영 환경별 구성 관리  
- 공통 네트워크, IAM 역할 등 조직 공통 리소스의 모듈화 및 배포

---

## 6. 참고 자료

- [Terraform 공식 문서](https://www.terraform.io/docs)  
- [Terraform Registry (모듈 모음)](https://registry.terraform.io/)  
- [HashiCorp Learn - Terraform 튜토리얼](https://learn.hashicorp.com/collections/terraform/aws-get-started)  
- [Terraform Best Practices](https://www.terraform-best-practices.com/)  
- [실전 Terraform 가이드 - 블로그 예시](https://woowabros.github.io/tools/2020/02/14/terraform-guide.html)

---
sticker: emoji//1f1f2-1f1e9
---
# 네이버 클라우드 플랫폼(NCP)과 아마존 웹 서비스(AWS) 서비스 비교
## 목차
- [주요 서비스 비교 표](#주요-서비스-비교-표)
- [설명](#설명)

| 분류                | 네이버 클라우드 플랫폼(NCP)    | 아마존 웹 서비스(AWS)         |
|---------------------|--------------------------------|------------------------------|
| 컴퓨팅              | Server (가상 서버)             | EC2 (Elastic Compute Cloud)  |
| 스토리지            | Object Storage                 | S3 (Simple Storage Service)  |
| 블록 스토리지       | Block Storage                  | EBS (Elastic Block Store)    |
| 파일 스토리지       | NAS (Network Attached Storage) | EFS (Elastic File System)    |
| 데이터베이스        | CDB for MySQL, CDB for MS-SQL  | RDS (Relational Database Service) |
| NoSQL               |  CDB for MongoDB             | DynamoDB                     |
| CDN                 | CDN+, Global Edge              | CloudFront                   |
| 로드 밸런서         | Load Balancer                  | ELB (Elastic Load Balancer)  |
| 컨테이너 서비스     | Kubernetes Service             | EKS (Elastic Kubernetes Service) |
| 서버리스 컴퓨팅      | Cloud Function                 | Lambda                       |
| 네트워크            | VPC (Virtual Private Cloud)    | VPC (Virtual Private Cloud)  |
| 네트워크 ACL        | NACL (Network ACL)             | NACL (Network Access Control List) |
| 보안 그룹           | ACG (Access Control Group)     | Security Groups               |
| 모니터링            | Monitoring                     | CloudWatch                   |
| 메시지 큐           | MQ                             | SQS (Simple Queue Service)    |
| 이메일              | Simple & SES                   | SES (Simple Email Service)   |
| 알림                | Cloud Alert                    | SNS (Simple Notification Service) |
| 모니터링            | Cloud Insight Monitoring       | CloudWatch                   |
| 자원 접근제어       | Sub Account                | IAM(Identity and Access Management) |

---
### 설명

1. **Cloud Insight Monitoring vs CloudWatch**: 두 서비스 모두 클라우드 리소스와 애플리케이션을 모니터링하는 기능을 제공합니다. 메트릭 수집, 알림, 대시보드 구성 등의 기능이 있습니다.

2. **NAS vs EFS**: 두 서비스는 네트워크에 연결된 파일 스토리지를 제공합니다. 여러 서버에서 동시에 접근이 가능하며, 데이터를 안전하게 저장할 수 있습니다.

3. **NACL**: 네이버 클라우드와 AWS 모두 NACL (Network Access Control List)라는 이름으로 네트워크 ACL을 제공합니다. 이는 서브넷 레벨에서의 인바운드와 아웃바운드 트래픽을 제어하는 역할을 합니다.
4. **서버리스**: NCP(Naver Cloud Platform)의 클라우드 펑션은 AWS Lambda와 유사한 서버리스 컴퓨팅 서비스입니다. 두 서비스 모두 개발자가 서버 관리 없이 코드를 실행할 수 있게 해주며, 이벤트 기반으로 동작합니다. 다양한 소스(예: HTTP 요청, 데이터베이스 변경 등)로부터 이벤트를 받아 자동으로 코드를 실행할 수 있습니다. \
NCP 클라우드 펑션도 AWS Lambda와 마찬가지로 자동 스케일링, 여러 프로그래밍 언어 지원, 빠른 배포와 업데이트, 비용 효율성 등의 장점을 가지고 있습니다.

### AWS IAM
네이버 서브계정과 AWS의 IAM(Identity and Access Management)은 유사한 개념입니다. 두 시스템 모두 주 계정의 자원에 대한 접근을 제어하고, 특정 사용자나 시스템에게 필요한 권한만을 부여할 수 있는 기능을 제공합니다.
- IAM을 통해 특정 사용자, 그룹, 또는 서비스에게 AWS 리소스에 대한 접근 권한을 세밀하게 제어할 수 있습니다.
- 정책(Policy)을 작성하여 특정 리소스에 대한 CRUD(Create, Read, Update, Delete) 권한을 부여하거나 제한할 수 있습니다.
- IAM 사용자는 AWS 리소스에 접근할 수 있는 자격 증명을 받으며, 이는 주 계정과는 별개의 접근 권한을 가집니다.

### 네이버 서브계정
- 네이버 서브계정을 통해서도 주 계정이 가진 서비스나 리소스에 대한 접근을 제어할 수 있습니다.
- 네이버 클라우드 플랫폼 같은 경우, 서브계정을 생성하여 특정 서비스에 대한 접근 권한을 부여할 수 있습니다.
- 이를 통해 팀원이나 다른 시스템이 필요한 작업을 안전하게 수행할 수 있도록 할 수 있습니다.

두 시스템 모두 보안과 권한 관리에 있어서 세밀한 제어가 가능하므로, 조직의 복잡한 요구 사항을 충족시키는 데 유용합니다.
AWS 보안 및 규정준수에 관한 주요 FAQ

# Q. AWS는 보안에 대한 노력

2018년 Verizon의 데이터 침해 조사 리포트에 따르면 IT계 보안사고는 사용자로 인한 보안사고가 가장 높다.
1위 해킹으로 유출된 자격증명 및 암호 탈취
2위 멀웨어(Malware)로 인한 정보 유출
3위 Fishing & Pharming
4위 권한 남용 및 오용
5위 설정 및 구성의 오류

AWS는 사용자로 인한 보안사고가 가장 높으므로 데이터로부터 사용자를 격리하는 것이 대응방안이다.
보안 사고 인지의 어려움 중 "피싱메일&멀웨어_49%", "금전 목표 공격_76%", "경쟁사 끼리 전략적 동기에 따른 공격_13%", "방치된 SW_68%"
중 "방치된 SW_68%"에 대한 "수개월 이상 지나 인지된 보안사고 비중"을 AWS는 예방할 수 있는 보안요소라고 타겟팅 한다.

Onpremise 환경에서 "탐지"가 어려워지는 이유는 다음과 같다.
1. 연결되는 Node가 다수(분석해야 할 데이터가 대규모)
2. 수많은 오탐(정상을 악의로 판단)과 미탐(악의를 정상으로 판단)
3. 보안관리자의 경험 부족

AWS는 위 3가지에 대한 보안 접근 전략을 다음과 같이 갖는다.
1. 사용자로 인한 보안사고.
  자동화를 통한 사용자를 격리한다
  관련 서비스 : Lambda, CloudWatch Events, SNS, CloudFormation
2. 보안사고 인지의 어려움.
  AI/규칙 기반의 탐지
  관련 서비스 : Tusted Advisor, Config Rules, Inspector, Macie, GuardDuty

AWS는 Cloud Security 를 최우선 과제로 잡는다. 무엇보다 고객의 데이터를 보호하는 것 보다 중요한 건 없다. 그렇기 때문에 AWS는 지난 10여년 
전 세계 수백만의 고객들로부터 Cloud보안에 대해 매우 다양하고 까다로운 요청들을 다루어 오고 있다.

고객이 선택할 수 있는 보안관려 AWS는 총 5가지로 나뉘며 다음과 같다.
1. 자산식별
System Manager, State Manager, Config, Macie
2. 보호
VPC, KMS, CloudHSM, IAM, Organization, Cognito, Directory Service, Single Sign-On, Certificate Manager, Secret Manager
3. 탐지
CloudTrail, Config Rules, CloudWatch Logs, GuardDuty, Inspector, VPC Flow Logs, Macie, Shield, WAF, Detective
4. 대응
Config Rules, Lambda, System Manager, CloudWatch Events, SNS
5. 복구
Config Rules, Lambda, Patch Manager

AWS는 "AWS 책임공유 모델"을 기반으로 인프라 및 데이터를 보호할 수 있다.
AWS담당영역은 "물리적 인프라"에 대한 보안을 충족할 수 있고 나머지 VPC 이후 네트워크, 보안그룹 및 OS 패치 그 밖 등등 고객 담당 영역으로 볼 수 있다.
# AWS가 제공하는 "AWS 책임공유 모델"의 책임 범위는 3가지이다.
1. Infra                : 데이터, 고객측IAM, AWS IAM, Application, OS, 네트워킹 및 방화벽
2. Container            : 데이터, 고객측IAM, AWS IAM, 방화벽 설정
3. Abstracted Services  : AWS IAM
#URL참조 : https://aws.amazon.com/ko/compliance/shared-responsibility-model/

위 내용을 모두 충족시킬 때(AWS책임영역&Client책임영역) 규제/표준에 따라 인증받을 수 있다.
ISMS-P, ISO27001 그 밖 등등.

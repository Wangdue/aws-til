# AWS 환경의 사용자 관리 및 접근제어

#### 1. 책임공유 모델
#### 2. AWS IAM
#### 3. AWS Directory Service
#### 4. AWS Secret Manager
#### 5. AWS Organizations

## 1. 책임공유 모델
AWS 보안에 있어 가장 본질적 목표는 AWS와 User의 책임이 명확하게 나눠져 있으며 이 두가지를 충족시킬 때 ISMS, ISO27001 등 인증 조건에 부합될 수 있다.
AWS에서 제공하는 표 "AWS 책임공유모델" 은 Infra Service, Container Service, Abstract Service로 세분화 되며 각 서비스가 제공하는 범위는 상이하다.

1. Infra Service 보안 범위
  - Infra Service는 AWS가 물리적 환경에 대한 보안을 제공하고 OS이후 데이터는 고객 책임으로 한다.
    > 범위(EC2 Service, AutoScailing, VPC)
      AWS    - Global Infra(Region, AZ, Edge Location), AWS핵심서비스(Computing, Storage, DB, Networking)
      Client - OS, FireWall, Platform, Application, User Access, Encryption from Client Data

2. Container Service 보안 범위(RDS, EMR, EBeanStalk)
  - Container Service는 AWS가 리소스 접근 관리까지 관리하고 Client 측 데이터는 고객 책임으로 한다.
    > 범위
      AWS    - Global Infra(Region, AZ, Edge Location), AWS핵심서비스(Computing, Storage, DB, Networking), OS & Firewall, UserAccess
      Client - Client측 데이터 암호화, 서버 측 데이터 암호화, 네트워크 트래픽 보호, 고객 데이터

3. Abstract Service 보안 범위(S3, DDB, Glacier, SQS, SES..)
  - Abstract Service는 AWS가 Client 측 데이터 암호화를 제외한 나머지를 모두 보호한다.
    > 범위
      AWS    - Global Infra(Region, AZ, Edge Location), AWS핵심서비스(Computing, Storage, DB, Networking), OS & Firewall, UserAccess,  서버 측 데이터 암호화, 네트워크 트래픽 보호
      Client - Client측 데이터 암호화, 고객 데이터
      
# Summary
1. Infra Structure Service
  - 데이터, 고객측 IAM, Application, OS, Networking/Firewall
2. Container Service
  - 데이터, 고객측 IAM, AWS IAM, Networking/Firewall  
3. Abstract Service
  - 데이터, AWS IAM
  
# AWS IAM Principals
1. Account Owner ID(Root Account)
  - 모든 서비스에 대한 접근 가능
  - 빌링 접근
  - API Console에 대한 접근
  - Case Open 접근

2. IAM Users, Groups And Roles
  - 특정 서비스에 대한 접근 가능
  - Case Open 접근
  - API Console에 대한 접근
  
3. Temporary Security Credentials
  - 콘솔 API에 대한 접근
  - API Console에 대한 접근
  
IAM 사용자에게는 상시 자격증명인(Access Key / Secret Access Key) Pair가 부여 됩니다. Key Pair는 REST API, CLI, SDK를 통해 활용할 수 있다.
Access Key와 Secret Key는 IAM User 생성시 만들 수 있으며 Secret Key는 IAMUser 생성시에만 다운로드가 가능하다.
주기적 교체를 목적으로 IAM 사용자 당 최대 2개까지 생성할 수 있다.

### IAM Role vs IAM USER
IAM Role을 사용하는 목적은 자동화된 프로세스에서 임시로 자격증명을 발급받기위해 사용한다. 즉, IAM 서비스들에서 인증 연계된 외부 사용자들이
임시자격을 받을 수 있다.
IAM User는 실 사용자 기준으로 통제하거나 상시로 자격증명을 인증받을 때 사용되며 IAM Group으로 관리된다.

IAM Group은 보안 주체는 아니지만 IAM Group에 소속된 IAM User들을 관리할 수 있다. 다수 User들에게 Group 권한을 인가하여 적용시킬 수 있으며, IAM Group끼리의 종속은 안되지만 
1개의 IAM USER가 다수 IAM Group에 최대 10개까지 포함될 수 있다.

### IAM 요청 처리단계
보안 주체가 Console, API, CLI를 통해 전송된 요청들은 다음 단계에 따라 평가되고 허용 여부가 판단된다.
1. 요청자 인증        : 익명 요청을 허용하는 S3(Allow Public Access), Cognito(Account Pool Mng)
2. 요청 컨텍스트 수집 : 요청 주체 및 리소스 관련 데이터 등을 수집하여 적합한 접근제어 정책들을 찾는다.
3. 접근제어 정책 평가
4. 요청의 허용/거부 결정

IAM Access 요청의 성공조건은 "IAM 보안 주체의 서명값이 포함" 되어있거나 "정책(Policy)에 의해 요청이 정확하게 인가"되어야 한다.
여기서 "서명값"에 대한 식별은 "Access KEY"로 요청한 주체의 IAM Principal를 확인 할 수 있고 Secret Key로 생성한 HMAC 서명값을 검증할 수 있다.
AWS의 보안 정책은 Deny이며 명시적으로 Allow 할 수 있다.

### Organization SCP(Service Control Policy)
SCP는 Root Account의 AWS 서비스 API접근을 통제하기 위한 목적이다 즉 "권한의 가드레일"이라고 할 수 있는데. 기본적으로 모든 OU, Account는 "FullAccess" SCP를 가지고 있다.
Organization Tree상에서 권한 상속이 지원되며, 다른 정책의 권한과 교집합 처리된다.
#참고로 SCP Master Account에는 적용이 불가하다.


# AWS Directory Service
AWS Directory Service는 AWS Cloud에서 운영되는 관리형 Microsoft Active Directory이다.
AWS Directory Service는 고객 Directory기반 워크로드를 손쉽게 관리형으로 전환할 수 있고 EndUser로 접근시 SSO를 지원하여 접근이 가능하다. 또한
AWS 서비스와 통합된 MS Active Directory를 이용할 수 있다.

AWS Active Directory는 2012 R2 Server OS를 사용하며 단일 테넌트로 관리된다. 디폴트로 한 쌍의 도메인 컨트롤러가 멀티 AZ에 배포된다.
#AWS Active Directory를 이용하면 MS OS기반 EC2 Instance의 관리를 AWS AWS AD를 이용할 수 있으며 "패스워드 정책 강제", "사용자 로그인 및 로그아웃" 스크립트를 적용할 수 있다.ㅁ
#AWS Active Directory를 이용하면 AWS WorkSpace에 대한 SSO를 위해 Active Directory를 통해 사용할 수 있다.

### AD구성에 대한 선택 가능한 옵션들은 다음과 같다.
1. 고객 관리, 온프레미스에 구성
2. 고객 관리, Amazon EC2에 구성
3. AWS-관리형, AWS 클라우드에 구성

AD 구성완료시 주요 기능들은 다음과 같다.
1. Trust Support
2. AWS Mng Console 페더레이션 접근
3. Multi Account 및 VPC지원
4. 고가용성 및 일간 스냅샷
5. 그룹 정책 지원

## AWS Secret Manager
AWS Secret Manager는 DB암호 또는 API Key 등 주요 자격증명에 대한 LifeCycle을 관리한다.
1. 자격증명을 안전하게 교체할 수 있다.
2. 정책기반 접근 제어
3. 중앙화된 감사
4. 사용한 만큼 지불
5. 암호화된 저장소
6. 로깅 및 모니터링
7. 버전 관리

AWS Security Manager의 적용 케이스는 다음과 같다.
1. Application Code에서 직접 DB 접근 시
  1) Application Code에서 DB 접근 시 Secret Manager로 관리할 수 있다.
  2) DevOps 엔지니어가 IAM Role 포함된 Application을 배포할 수 있다.
  3) Application 쪽 IAM 규칙을 통해 제공된 권한으로 Secrets Manager를 호출하여 필요한 자격증명을 획득하고 DB에 접근할 수 있다.
2. DB 자격증명에 대한 주기적 교체를 Application 중단 없이 적용
  1) Secrets Manager는 동등한 권한의 새로운 자격증명을 생성할 수 있다.
  2) Secrets Manager는 API호출을 통해 생성된 자격증명을 생성한다.
  3) Secrets Manager는 예전 자격증명을 안전하게 비활성화 처리할 수 있다.
3. Secrets Manager의 이용 방법은 다음과 같다.
  1) 초기 자격증명을 Secrets Manager로 로딩하여 권한 및 Key LifeCycle을 설정할 수 있다.
  2) EC2에 할당된 IAM 역할을 통해 필요한 자격증명과 권한을 제공받을 수 있고 자격증명을 인출할 수 있다.
  3) Secrets Manager 내부에 저장된 자격증명 정보를 수정할 수 있다.

### AWS Systems Manager과 Parameter Store의 차이
1. Systems Manager Parameter Store
  1) 구성작업과 자동화를 위해 스크립트 상에서 파라미터를 사용할 수 있다.
  2) 생성시 부여 된 ID를 통해 정보에 접근할 수 있다.
2. AWS Secrets Manager
  1) 기업내 자격증명의 라이프 사이클을 관리할 수 있다.
  2) 자격증명의 자동화된 교체를 통해 보안 요건을 충족시킬 수 있다.
  3) Lambda를 이용하여 확장할 수 있다.
  4) RDS 빌트인 연계기능을 이용하여 RDS 자격증명을 교체할 수 있다.
  
### Organization
AWS Organization기능으로 다음과 같은 편의성을 제공할 수 있다.
1. 보안경계
2. 리소스 분리
3. 청구 분리

한개의 Account로는 여러 Account별 관리가 힘들다 Multi Account 환경으로 구성 및 관리적 부담, Account 별 복잡한 정책 등 단점이 있겠지만
보안&리소스 분리도가 향상되고 침해공격 발생시 피해 범위를 제한할 수 있으며 Account 당 사용량과 비용 파악이 쉽다.

Organization Multi Account 시 다음과 같은 Account 활용을 추천한다.
1. Billing    : Multi Account의 Billing이 한개의 Master Account에서 수행할 수 있다.
2. Security   : 각 Account 별 Dev or Ops or Sec 등 목적별 구성을 할 수 있다.
3. SendBox    : Account에 AMI를 스냅샷 이후 생성하여 Ops환경과 동일한 Instance 펌웨어 테스트가 가능하다.
4. Staging    : POC목적 Account
5. Ops        : 운영목적의 Account

### Acccount Sec의 기본요건은 다음과 같다.
Login_MFA --> AWS CloudTrail(API Call Logging) --> Enterprise Role연계(Temporate Auth) --> Federation --> Sec전용 CrossAccount(Guest) --> Action
#URL참조_https://www.slideshare.net/awskorea/7-multi-account-environment-security-visibility-accelerate-strategy

  
  




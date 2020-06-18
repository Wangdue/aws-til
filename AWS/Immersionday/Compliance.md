# AWS 보안 및 규정준수에 관한 주요 FAQ

### Q. AWS는 보안에 대한 노력

2018년 Verizon의 데이터 침해 조사 리포트에 따르면 IT계 보안사고는 사용자로 인한 보안사고가 가장 높다.
1위 해킹으로 유출된 자격증명 및 암호 탈취
2위 멀웨어(Malware)로 인한 정보 유출
3위 Fishing & Pharming
4위 권한 남용 및 오용
5위 설정 및 구성의 오류

### AWS는 사용자로 인한 보안사고가 가장 높으므로 데이터로부터 사용자를 격리하는 것이 대응방안이다.
보안 사고 인지의 어려움 중 "피싱메일&멀웨어_49%", "금전 목표 공격_76%", "경쟁사 끼리 전략적 동기에 따른 공격_13%", "방치된 SW_68%"
중 "방치된 SW_68%"에 대한 "수개월 이상 지나 인지된 보안사고 비중"을 AWS는 예방할 수 있는 보안요소라고 타겟팅 한다.

### Onpremise 환경에서 "탐지"가 어려워지는 이유는 다음과 같다.
1. 연결되는 Node가 다수(분석해야 할 데이터가 대규모)
2. 수많은 오탐(정상을 악의로 판단)과 미탐(악의를 정상으로 판단)
3. 보안관리자의 경험 부족

### AWS는 위 3가지에 대한 보안 접근 전략을 다음과 같이 갖는다.
1. 사용자로 인한 보안사고.
  자동화를 통한 사용자를 격리한다
  관련 서비스 : Lambda, CloudWatch Events, SNS, CloudFormation
2. 보안사고 인지의 어려움.
  AI/규칙 기반의 탐지
  관련 서비스 : Tusted Advisor, Config Rules, Inspector, Macie, GuardDuty

AWS는 Cloud Security 를 최우선 과제로 잡는다. 무엇보다 고객의 데이터를 보호하는 것 보다 중요한 건 없다. 그렇기 때문에 AWS는 지난 10여년 
전 세계 수백만의 고객들로부터 Cloud보안에 대해 매우 다양하고 까다로운 요청들을 다루어 오고 있다.

### 보안관련하여 User가 선택가능 옵션은 5가지로 다음과 같다.
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

### AWS는 "AWS 책임공유 모델"을 기반으로 인프라 및 데이터를 보호할 수 있다.
AWS담당영역은 "물리적 인프라"에 대한 보안을 충족할 수 있고 나머지 VPC 이후 네트워크, 보안그룹 및 OS 패치 그 밖 등등 고객 담당 영역으로 볼 수 있다.

### AWS가 제공하는 "AWS 책임공유 모델"의 책임 범위는 3가지이다.
1. Infra                : 데이터, 고객측IAM, AWS IAM, Application, OS, 네트워킹 및 방화벽
2. Container            : 데이터, 고객측IAM, AWS IAM, 방화벽 설정
3. Abstracted Services  : AWS IAM
#URL참조 : https://aws.amazon.com/ko/compliance/shared-responsibility-model/

위 내용을 모두 충족시킬 때(AWS책임영역&Client책임영역) 규제/표준에 따라 인증받을 수 있다.
ISMS-P, ISO27001 그 밖 등등.

### AWS에서 User의 데이터 소유권은 다음과 같다.
1. 데이터 소유권
  AWS는 개인 데이터에 대해 어떠한 침해를 할 수 없다. User가 직접 데이터가 저장될 경로를 설정할 수 있다. 개인이 이전하지 않는 이상 절대 임의로 이전되지 않는다.
2. 데이터 접근
  AWS에서 "IAM"을 이용하여 데이터를 "누가","언제","어디서","무엇을","결과"로 알 수 있다.
3. 데이터 암호화
  고객들은 KMS, CloudHSM, OnpremiseHSM 또는 Client 측 암호화를 통해 저장시/전송중 암호화를 적용할 수 있다.
  
### AWS의 데이터 센터 위치를 알 수 있나?
AWS 데이터센터는 일반인에게 구체적인 위치를 알려줄 수 없다. 대신 "서울" 리전 인프라 운영에 대해 ISMS 인증이 완료되었으며 각 AZ간 거리가 최대 100km를 가정했을 때 수도권 주변에 IDC가 위치한다는 것을 알 수 있다.
#어디에도 AWS 데이터센터를 구체적인 지역 위치가 명시되어야 한다 라는 항목은 없다.

물리적인 피해를 대비하여 데이터 센터 방문은 허용하지 않고 ISMS인증을 수행하는 특정 인력들만이 미리 신청 후 출입이 가능하다.
ISMS 인증을 수행하는 인력 또는 외주 업체들에 대한 물리적 접근을 "외주업체 엑세스" 항목을 통해 최소 "30일" 전에 승인 및 허가를 요청한다.

### 데이터 완전 삭제(폐기)에 대한 입증은?
AWS 데이터 폐기에 대한 입증은 물리적, 논리적, 독립적 감사로 다음과 같다.
1. 논리적
  데이터 삭제처리 이후 즉각적인 접근이 불가능하다. 암호화 뒤 데이터를 삭제하고, 키를 영구 삭제하는 방식으로 보완할 수 있다.
2. 물리적
  AWS는 데이터 폐기 절차를 "디가우스"처리를 하고 업계 표준 기준에 맞추어 물리적으로 폐기한다.
3. 독립적 감사
  데이터 폐기 절차는 ISO27001과 SOC2의 감사 대상이다.
  
### 개인 사용자 데이터는 정부기관과 연계되어있나?
1. 요청에 대한 검증 : 요청에 기준에 다르지만 법적 절차나 기준에 해댕되지 않으면 고객 데이터를 제공하지 않는다.
2. 통지 : AWS 측에서 데이터가 사용시 사용자에게 미리 통지할 수 있다.
3. 정보 요청 통계 : AWS로 데이터 요청받은 건들에 대해 통계를 반기 별로 공개하고 있다.
4. 암호화 : User들은 "key"를 적용하도록 하고 암호키가 없을 때 데이터 사용이 어렵다.
 > AWS 데이터는 자동화되어 수동으로 데이터를 조작할 수 없다.
 > 만약 데이터에 접근 시 MFA, Logging을 통해 추적이 가능하다.
 > User들이 직접 CMK(Client Master Key)를 생성하여 데이터를 암호화할 수 있다.
 
### K-ISMS 도움 될 내용
 1. "물리적 보안" 내용은 AWS "AWS 책임공유모델"을 통해 수행되고 있다. 따라서 인증심사 항목에서 제외될 수 있다.
 2. CloudFormation으로 리전 변경이 발생 하더라도 동일 아키텍처가 적용되었음을 쉽게 소명하여 최초 심사가 아닌 사후 심사로 진행할 수 있다.
 3. 법적 근거 없이 고객의 의사에 따라 AWS 주요 정보를 증적 자료로 제출할 수 없다. K-ISMS 정통법 21조 인증심사 방법 및 보완조치에 근거한다.
 
### Onpremise 환경보다 더 안전할까?
 1. 격리 : AWS의 물리적 환경은 Onpremise와 동일한 "보안격리&보호수준"을 갖추고 있으며 주기적으로 외부기관의 감사를 받고있다.
 2. 가시성 : CloudTrail, VPC Flow, Config를 통해서 네트웤 레벨의 로깅, 이력 변경에 대한 추적성을 제공할 수 있다.
 3. 자동화 : AWS 모든 것이 API기반이기 때문에 모든 대응을 Script로 활용하여 자동화할 수 있다.
 4. AWS 시스템은 가상 소프트웨어를 통한 필터링으로 물리적 또는 Instance에 엑세스하는 것을 차단할 수 있도록 설계되어있다.
 
### AWS Hyper Visor
 10년 정도 Hyper Visor 기술을 운영했던 경험이있다. 완전히 개조된 형태의 Xen, KVM 기반 Hyper Visor를 사용할 수 있다.
 
### System Patch
 1. 공인 AMI  : AWS가 제공하는 공인 AMI는 AWS가 자체적으로 Patching한다. 예를 들면 MS 신규패치 발표 후 5일 안에 AMI패치가 적용된다.
 2. Client    : 고객이 직접 OS에 대한 패치를 진행할 수 있다. 
 3. 보안 게시판 : AWS는 주요 보안 위협들에 대해 내부 시스템을 업데이트 하고 그 내역을 고객들에게 통지한다.
 
### Access KEY 보관
 1. 책임공유모델 : Key는 고객의 책임이며 요청 IP를 제한하는 조건을 갖는 IAM정책을 적용할 수 있다.
 2. 모니터링 : AWS는 유출된 키를 찾는 별도 부서가 있고 고객들에게 즉각적 통지가 가능하다.
 3. Trusted Advisor에서 유출된 Access Key를 탐지하여 즉시 통지하는 기능을 제공할 수 있고 CloudWatch Event를 생성하여 유출된 Key를 폐기할 수 있고 소요시간은 30초이다.

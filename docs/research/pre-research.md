# 캡스톤 주제 조사
> 캡스톤 주제 선정을 위해 각 팀원 자료 조사 내용 정리

### 1. KakaoTalk DB 암호화 루틴 분석 및 복호화
로컬에 저장되는 톡내용을 복호화 하는 과정. 리버싱 어려워 보이는데, 최근 사례도 상세하고 목표도 확실함. 

* **결과물:** DB 복호화 도구 / 분석 보고서
* **기타 메모:** 
  * 현재 최신 버전의 카카오톡과 25년도와의 비교 확장 가능 (차별성)
  * 과거에는 UserId (실제 카톡 가입 시 부여받는 개인식별 번호)가 쉽게 노출됐다고 함 (지금은 아닌듯)
  * mac의 경우, UserId와 하드웨어 UUID로 암/복호화 키를 생성한다고 함
  * 암호화 모듈이 추가된 SQLite 사용
  * IDA Pro로 동적 분석, HxD로 파일 시그니처 확인 했다고함
  * 환경(윈도우, 맥, 안드로이드) 선택 필요. 안드로이드의 경우 연구는 없지만, 도구는 존재한다고 함
* **참고 링크:**
  * [25년도 분석 자료 - 이거 젤 중요](https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART003256893)
  * [macOS 분석 자료](https://public.thinkonweb.com/journals/jkiisc/digital-library/56149)
  * [안드로이드 복호화 툴](https://github.com/jiru/kakaodecrypt)

---

### 2. 자동차 해킹

**결과물:** 분석 보고서

**기타메모:**

* 가장 핵심적인 취약점은 차량 내 기본 통신 망인 CAN(Controller Area Network) 버스의 구조적 한계

* 패킷 주입 공격의 원리
  -> 평문 통신 : 키 없이 구조만 알면 쉽게 해독 가능한 전치 암호나 레일 펜스 (Rail Fence)처럼,  
                 CAN 패킷은 암호화되지 않은 평문 상태로 전송된다.

     ID 기반 우선순위: 메시지에 출발지 주소가 없고, 메시지의 성격을 나타내는 식별자(ID)만 존재한다.  
                       CAN 통신은 식별자 숫자가 낮을수록 버스 점유 우선순위가 높다.

     악의적 주입: 공격자가 취약한 인포테인먼트 기기 등을 통해 내부 망에 진입한 뒤, 조향이나 제동과 관련된  
                  우선순위가 높은 정상 ID를 위조해 버스에 지속적으로 흘려보낸다. 정상적인 출처 확인 불가xxxxxxx

* 주요 해킹 경로
  -> OBD-II 포트 (물리적 접근): 1996년(미국 기준) 이후 배출가스 및 차량 진단을 위해 모든 차량에 의무 장착된 표준 단자이다.  
                                 운전석 아래에 위치한 이 포트에 해커가 소형 악성 동글을 꽂으면, 원격으로 CAN 버스에 직접  
                                 패킷을 주입해 차량 가능.

     텔레매틱스 제어 장치(TCU) 공략: 차량에 내장된 LTE/5G 모뎀 역할을 하는 TCU의 취약점을 뚫는 방식.  
                                      2015년 발생한 지프 체로키 해킹 사건이 대표적으로, 해커들은 스프린트(Sprint) 통신망을 통해  
                                      차량의 인포테인먼트 시스템에 원격 접속한 뒤 내부 CAN 버스망으로 침투하여 브레이크와 조향 장치를 원격 제어했다.

---

### 3. AWS 클라우드 인프라 분석을 통한 Azure/GCP 멀티클라우드 마이그레이션 자동화 시스템
   * aws 환경에 구축된 인프라르 IaC로 변환 -> azure/GCP등의 IaC로 변환 
   * 기존 AWS 인프라를 자동으로 식별·분석하여 IaC로 변환하고, 리소스 간 의존성과 보안 요구사항을 유지하면서 Azure 또는 GCP 환경에 적합한 Terraform 코드로 자동 재구성하는 멀티클라우드 마이그레이션 시스템을 구현한다.

---

### 4. 최신 취약점 테스트 환경 제작
최근이나 대표적인 취약점을 실제 도커로 재현. 누구나 접속해서 자유롭게 사용 가능한 워게임 사이트 제작

* **결과물:** 분석 보고서 / 워게임 사이트 (vm instance 끄고 킬수 있는)
---

### 5. CAN Fuzzing 기반 취약 ECU 탐색기 
CAN ID와 payload를 체계적으로 변형하면서 대상 ECU/가상 ECU의 비정상 상태를 탐색하는 퍼저를 제작. 단순 랜덤 fuzzing보다 state-aware fuzzing으로 발전 가능함.

* **결과물:** CAN
* **기타 메모:**
  * ECU(Electronic Control Unit) : 자동차 내에서 하나 이상의 전기/하위 시스템을 제어하는 임베디드
  * 차량의 원활한 제어를 위해 각 ECU 간의 통신이 필요
  * 초기에는 랜덤/Mutation-based Fuzzing > 이후 정상 CAN 트래픽에서 메시지 구주와 ECU 상태 분석하여 현재 상태 적합한 입력을 생성하는 State-aware Fuzzing으로 확장
  * Python/C++ 기반 퍼저
  * HIL(Hardware-in-the-Loop) : 실제 제어기 하드웨어를 가상의 시뮬레이션 환경에 연결해 시스템을 검증하는 테스트 기법
* **참고 링크:**
  * [CAN 통신 관련 참고 개념](https://youngseong.tistory.com/336)
  * [리눅스에서 CAN 연동 관련 문서](https://www.kernel.org/doc/html/latest/networking/can.html)
  * [실차 없이 가상으로 테스트 가능한 오픈소스](https://github.com/zombieCraig/ICSim)
  * [관련 연구 - state-aware CAN Fuzzing](https://www.mdpi.com/2673-4052/7/3/83)


---

### 6. 깃허브 태그 무결성 실시간 탐지 시스템
조직이 사용하는 GitHub Actions의 버전 태그(v1, v44 등)가 기존과 다른 커밋으로 강제 재지정(force-move/retagging)되는 순간을 실시간으로 탐지, 가능하면 차단까지 하는 도구를 제작. tj-actions 사건의 핵심 공격 벡터인 태그 하이재킹(모든 태그를 하나의 악성 커밋으로 재지정)을 직접 방어 하는 도구로 발전 가능함.

* **결과물:** Tag Monitor
* **기타 메모:**
  * 태그(tag)는 브랜치처럼 강제로 다른 커밋을 지정하게 옮길 수 있음 → 태그로 액션을 참조하면 실제로는 매번 다른 코드가 실행될 수 있음
  * tj-actions 사건: v1.0.0~v44.5.1 전체 태그를 공격자가 하나의 악성 커밋으로 재지정 → 워크플로우가 "안전한 버전"을 쓰고 있다고 믿었지만 실제로는 악성 코드 실행(25년 사례, 참조 링크)
  * 초기에는 조직 내 워크플로우가 참조하는 서드파티 액션의 태그→커밋 SHA 매핑을 주기적으로 스냅샷 > 이후 GitHub 웹훅(push/delete 이벤트)을 구독해 태그 변경을 실시간 감지하는 구조로 확장
  * 탐지 시 커밋 diff, 작성자 서명 여부(Verified), 봇 계정 스푸핑 여부까지 함께 분석해 알림
  * GitHub REST/GraphQL API 기반, Python 백엔드 + 알림 파이프라인(이메일)
  * 가능하면 확장해서 변조 감지 발생하면 알림 + 안전한 최신 Commit SHA로 워크 플로우 파일  자동 수정하여 Pull Request를 생성하는 기능까지 구현해보기
  * **참고 링크:**

---

### 7. Blue Team 사이버 공격 탐지 실습 플랫폼 설계 및 구현
기존 워게임: 공격해서 Flag를 획득하는 것이 끝. 여기서 끝나지 않고, 이미 공격당한 시스템의 PCAP, 로그, 악성 파일 등을 주고 사용자가 공격 과정을 밝혀낸 뒤 Snort와 YARA 룰을 직접 작성하고, 숨겨진 데이터셋(임의로 구성해놓은)을 통해 실제로 얼마나 잘 탐지하는지 평가하는 Blue Team 입장 워게임.

* **결과물:** Blue Team 워게임 플랫폼 (엄밀히 말하면 Detection Engineering Lab)
* **기타 메모:**
  * 검증 Dataset을 설정해서, 특정 침해를 하드코딩하는것을 방지.
  * 너무 많은 증거 X > 타임라인을 사용자가 직접 분석할 수 있게
  * 특정 IP 기반 룰 지양
  * 2~3개 정도의 구체적인 / 정확한 시나리오 제작에 집중.
  * Red Team 플랫폼, AI 자동 룰 생성, SIEM 구축, EDR 개발 등 너무 처음부터 과한 개념을 잡지 않고, "Incident > Investigate > Detect > Validate" 수준의 핵심 과정에 집중
  * alert tcp any any -> any any 와 같은 false positive 고려해야 함.
* **참고 링크:**
  * [Snort 룰 작성 가이드](https://docs.snort.org/intro)
  * [YARA 룰 작성 가이드](https://yara.readthedocs.io/en/latest/writingrules.html)
  * [구체적인 시나리오 제작 시 참고](https://attack.mitre.org/)
  * [실제 침해사고 pcap 공유 페이지](https://malware-traffic-analysis.net/)
  * [관련 연구 - Defender가 실습할 수 있는 환경](https://github.com/clong/detectionlab)


---






    

## ✨PCB가 무엇인가요?

PCB(Process Control Block)는 `운영체제에서 관리하는 프로세스에 대한 메타데이터를 저장한 데이터블록`이며

커널 스택에 저장되며 각 프로세스가 생성될 때마다 고유의 PCB가 생성이 되고 프로세스가 종료되면 PCB는 제거됩니다.

## 🔁꼬리질문

### 🤔커널 스택이 뭔데요?

가상 메모리는 사용자공간과 커널공간으로 구분되며 이 두가지의 공간 모두 스택 자료구조를 기반으로 관리된다하여 사용자스택, 커널스택이라고도 불립니다.

이 때 커널스택은 가상 메모리 주소의 윗부분을 말합니다.

예를 들어 가장 처음 시작하는 주소값이다라고 보면 됩니다.

이 커널스택은 “커널모드”에서만 접근할 수 있게 되어있으며 반대로 사용자스택은 유저모드에서만 접근가능하게 되어있습니다.

### 🤔메타데이터가 뭔데요?

데이터에 관한 구조화된 데이터이자 데이터를 설명하는 작은 데이터, 대량의 정보 가운데에서 찾고 있는 정보를 효율적으로 찾아내서 이용하기 위해 일정한 규칙에 따라 콘텐츠에 대해 부여되는 데이터이다.

### 🤔PCB의 구조에 대해서 아는대로 말씀해주세요.

● 프로세스 상태 – 대기중, 실행 중 등 프로세스의 상태

● 프로세스 번호(PID) – 각 프로세스의 고유 식별 번호(프로세스 ID)

● 프로그램 카운터(PC) – 이 프로세스에 대해 실행될 다음 명령의 주소에 대한 포인터.

● 레지스터 – 레지스터관련 정보

● 메모리 제한(Memory Limits) – 프로세스의 메모리 관련정보

● 열린 파일 정보(List of Open Files) - 프로세스를 위해 열린 파일 목록들

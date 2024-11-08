
## 내용
SaaS을 만들기 위한 방법론으로 독립적인 애플리케이션 운영을 위한 12가지 요소 이다.

### Codebase
> 버전 관리되는 하나의 코드베이스와 다양한 배포
- Git의 main/test/dev 브랜치를 사용한다.

### Dependenices
> 명시적으로 선언되고 분리된 종속성
- package.json과 같은 의존성 관리툴에서 확인이 가능하다.

### Config
> 환경에 저장된 설정
- 모든 설정 정보는 코드로부터 분리된 공간에 저장되어야 하고, 런타임에서 코드에 의해 읽혀야 한다.
- 동일한 코드를 여러 환경에 배포한다.
- 데이터베이스, 외부리소스, 배포마다 달라지는 값 등이 분리되어야한다.

### Backing services
> 백엔드 서비스를 연결된 리소스로 취급
- SaaS의 리소스는 자유롭게 배포에 연결되거나 분리할 수 있고, 코드 수정 없이 전환이 가능해야 한다.
  
### Build, release, run
> 철처하게 분리된 빌드와 실행 단계

### Processes
> 애플리케이션을 하나 혹은 여러개의 무상태(stateless) 프로세스로 실행
- 인스턴스는 메모리 파일을 공유할 수 없으며, 재실행 될 때 로컬 파일, 세션은 모두 초기화된다.
- 세션 상태 데이터의 경우 Redis에 저장한다.

### Port binding
> 포트 바인딩을 사용해서 서비스를 공개

### Concurrency
> 프로세스 모델을 사용한 확장
- 모든 일을 처리하는 하나의 프로세스 대신 기능별로 분리된 프로세스로 실행한다.

### Disposability
> 빠른 시작과 그레이스풀 셧다운(graceful shutdown)을 통한 안정성 극대화
- graceful shutdown은 프로그램이 종료될 때 최대한 side effect를 내지 않기 위해 로직을 잘 처리하고 종료하는 거이다.

### Dev/prod parity
> 개발, 스테이징, 프로덕션 환경을 최대한 비슷하게 유지

### Logs
> 로그를 이벤트 스트림으로 취급
- 스트림을 버퍼링  없이 stdout, stderr로 출력한다.
- 별도의 로그 저장소를 사용한다.

### Admin processes
> admin/maintenance 작업을 일회성 프로세스로 실행

## 참고
- https://12factor.net/
- https://fe-developers.kakaoent.com/2021/211125-create-12factor-app-with-nextjs/
- https://medium.com/dtevangelist/12-factors-%EB%9E%80-b39c7ef1ed30
## 요약
- 계정에 사용하기 위한 개인 키와 주소를 보관하는 파일이다.
## 내용
- 블록체인에서 계정의 개인 키와 주소를 안전하게 보관하고, 이를 통해 트랜잭션에 서명하거나 인증할 수 있도록 도와주는 역할이다.
- 새로운 계정을 만들 때, Keyring은 개인 키를 생성하고 안전하게 저장한다.
- 파일 시스템, 운영 체제 키 저장소, 메모리 저장소...를 다양한 저장 방식을 지원한다.
- AWS Keyring을 예시로 들면 다음과 같다.
	- 키링을 사용하여 엔벨로프 암호화를 수행한다.
	- 키링은 데이터 키를 생성, 암호화 및 복호화한다.
	- 키링에 따라 원본과 해당 데이터 키를 암호화하는 래핑 키가 결정된다.
## 참고
- https://en.wikipedia.org/wiki/Keyring_(cryptography)
- https://docs.aws.amazon.com/ko_kr/encryption-sdk/latest/developer-guide/choose-keyring.html
- https://cloud.google.com/kms/docs/create-key-ring?hl=ko
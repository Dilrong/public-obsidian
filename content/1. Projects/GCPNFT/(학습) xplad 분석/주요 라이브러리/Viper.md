## 요약
- golang 설정 관리 모듈
## 내용
- 환경 변수, 설정 파일, kv 스토리지를 통해서 설정을 가져온다.
- 설정 우선순위
	- 코드 내에서 `Set`함수를 통해 설정된 값
	- cli 커맨드 상의 flag
	- 환경 변수
	- 설정 파일
	- kv 스토리지
	- default
---
- [xplad cmd](https://github.com/xpladev/xpla/blob/main/cmd/xplad/cmd/root.go)를 예시로 하면 다음과 같다.
```go
...
// Setup chainId  
chainID := cast.ToString(appOpts.Get(flags.FlagChainID))  
if len(chainID) == 0 {
   // 설정 초기화
   v := viper.New()  
   // 디렉토리 설정
   v.AddConfigPath(filepath.Join(home, "config"))  
   // 파일 이름 설정
   v.SetConfigName("client")  
   // 파일 타입 설정
   v.SetConfigType("toml")  
   // Config 읽기
   if err := v.ReadInConfig(); err != nil {  
      panic(err)  
   }  
   conf := new(config.ClientConfig)
   // 언마샬링  
   if err := v.Unmarshal(conf); err != nil {  
      panic(err)  
   }  
   chainID = conf.ChainID  
}
...
```
## 참고
- [https://github.com/spf13/viper](https://github.com/spf13/viper)
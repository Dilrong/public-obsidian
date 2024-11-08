## 요약
- Go로 CLI의 Command 실행을 구현해주는 라이브러리
## 내용
- Cobra를 통해 구현하는 CLI 인터페이스는 항상 1개의 Root Command를 가진다.
```go
type Command struct {
	// 커맨드 표기
	Use string
	// 짧은 설명
	Short string
	// 긴 설명
	Long string
	// 예시 표기
	Example string
	// 기대되는 인자의 개수를 지정
	Args PositionalArgs
    // 커맨드 로직 이전에 실행되는 함수를 지정합니다.
    PreRun func(cmd *Command, args []string)
    // PreRun과 같지만 에러를 반환합니다.
    PreRunE func(cmd *Command, args []string) error
}
```
- 구조체는 위와 같고, [코드](https://pkg.go.dev/github.com/spf13/cobra#Command)를 확인하면 상세 구조체를 볼 수 있다.
---
- Sub Command는 Root Command중심의 Tree 구조로 구성된다. (여러 층의 구조로 구성될 수 있다.)
- `rootCmd.AddCommand()`로 추가 된다.
---
- [xplad cmd](https://github.com/xpladev/xpla/blob/main/cmd/xplad/cmd/root.go)를 예시로 하면 다음과 같다.
```go
// root cmd 생성
func NewRootCmd(*cobra.Command, params.EncodingConfig) {
...
	rootCmd := &cobra.Command{  
	   //xplad를 커맨드로 쓴다.
	   Use:   "xplad",  
	   // 짧은 설명이 나오는 부분은 'xpla app'으로 나온다.
	   Short: "xpla App",  
	   // 실행 전 오류 확인 
	   PersistentPreRunE: func(cmd *cobra.Command, _ []string) error {  
	      ...
	   },  
	}  

	// init cmd
	initRootCmd(rootCmd, encodingConfig)
}

```

```go
func initRootCmd(rootCmd *cobra.Command, encodingConfig params.EncodingConfig) {
...
	// sub command 추가
	rootCmd.AddCommand(...)
...
}
```
## 참고
- https://nangman14.tistory.com/97
- https://github.com/xpladev/xpla/blob/main/cmd/xplad/cmd/root.go
## 요약

## 내용
- Server-Sent Events (SSE)는 클라이언트가 HTTP 연결을 통해 서버로부터 자동 업데이트를 수신할 수 있도록 하는 서버 푸시 기술이다. 각 알림은 한 쌍의 개행으로 끝나는 텍스트 블록으로 전송된다.

```ts
@Sse('sse')
sse(): Observable<MessageEvent> {
  return interval(1000).pipe(map((_) => ({ data: { hello: 'world' } })));
}
```
- SSE는 `Obserable`스트림을 반환해야한다.
- 이벤트는 [EventSource API](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)로 구독이 가능하다.

---
- 단방향 통신이 필요한 경우 사용한다. (피드 구독, 주가 구독, 라이브 점수 받기, 알림 등)
- 소켓은 양방향이지만 **SSE는 클라이언트가 데이터를 받을 수만** 있다.
## 참고
- https://docs.nestjs.com/techniques/server-sent-events
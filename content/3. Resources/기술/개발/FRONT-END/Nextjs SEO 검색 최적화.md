## 요약

## 내용
### 검색엔진 등록
- 네이버 서치 콘솔
- 구글 서치 콘솔
### robots.txt
- 검색 엔진 크롤러에게 사이트에서 요청할 수 있는 페이지나 파일을 알려준다.
- `public`에 넣어서 `/robots.txt` 에서 확인이 가능하도록 한다.
```jsx
//robots.txt

// Block all crawlers for /accounts
User-agent: *
Disallow: /accounts

// Allow all crawlers
User-agent: *
Allow: /
```

### sitemap
- 500페이지 이상을 가지면 필수이다.
- https://github.com/iamvishnusankar/next-sitemap

### meta tags
- title
- description
- canonical
- openGraph
- twitter
- https://www.npmjs.com/package/next-seo
## 참고
- [[SEO 체크리스트]]
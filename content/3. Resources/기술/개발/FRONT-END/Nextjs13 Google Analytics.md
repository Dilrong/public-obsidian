## 요약
- Nextjs13 Google Analytics 적용 코드

## 내용
### Analytics.tsx
```tsx
// Analytics.tsx
"use client"

import { GTM_ID, pageview } from "lib/gtm"
import { usePathname, useSearchParams } from "next/navigation"
import Script from "next/script"
import { useEffect } from "react"

export default function Analytics() {
  const pathname = usePathname()
  const searchParams = useSearchParams()

  useEffect(() => {
    if (pathname) {
      pageview(pathname)
    }
  }, [pathname, searchParams])

  if (process.env.NEXT_PUBLIC_VERCEL_ENV !== "production") {
    return null
  }

  return (
    <>
      <noscript>
        <iframe
          src={`https://www.googletagmanager.com/ns.html?id=${GTM_ID}`}
          height="0"
          width="0"
          style={{ display: "none", visibility: "hidden" }}
        />
      </noscript>
      <Script
        id="gtm-script"
        strategy="afterInteractive"
        dangerouslySetInnerHTML={{
          __html: `
    (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer', '${GTM_ID}');
  `,
        }}
      />
    </>
  )
}
```

### layout.tsx
```tsx
// layout.tsx
<html lang="it">
<body>
    <Suspense>
        <Analytics />
    </Suspense>
    ...
</body>
```

## 참고
-  https://dev.to/valse/how-to-setup-google-tag-manager-in-a-next-13-app-router-website-248p
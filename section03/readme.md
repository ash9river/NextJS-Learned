# 라우팅 및 페이지 렌더링 - 심층분석

## Not Found

- `Not Found` 페이지는 루트 폴더에 `not-found.js` 파일을 만듬으로써 해결 가능

```javascript
export default function NotFoundPage() {
  return (
    <div id="error">
      <h1>Not Found!</h1>
      <p>The requested resource could not be found!</p>
    </div>
  );
}
```

- 만약에 어떠한 `slug`에 대해서 존재하지 않는 경로를 탐색한다면, 바로 `notFound`가 호출되지 않음.
  - 존재하지 않으면 `notFound()` 함수를 호출하는 논리를 만들어야함
 
```javascript
import { DUMMY_NEWS } from "@/dummy-news";
import { notFound } from "next/navigation";

export default function NewsDetailPage({ params }) {
  const { slug: newSlug } = params;

  const newsItem = DUMMY_NEWS.find((item) => item.slug === newSlug);

  if (!newsItem) {
    notFound();
  }

  return (
    <article className="news-article">
      <header>
        <img src={`/images/news/${newsItem.image}`} alt={newsItem.title} />
        <h1>{newsItem.title}</h1>
        <time dateTime={newsItem.date}>{newsItem.date}</time>
      </header>
      <p>{newsItem.content}</p>
    </article>
  );
}
```  

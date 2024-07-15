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

## 병렬 라우트 설정 및 사용

- 병렬 라우팅을 설정하려면 병렬 라우트를 포함하는 경로에 `layout.js` 파일을 추가해야한다.
- 또한, 병렬 라우트마다 이름이 `@`로 시작하는 하위폴더 하나를 추가한다.
- `layout` 컴포넌트에서는 `children` 속성을 통해서 액세스할 수 있는데, 병렬 라우트에서는 `@` 기호로 시작하는 폴더와 동일한 이름을 가지는 속성으로 사용가능하다.
  - ex) `@archive` 폴더일경우, `archive`로 접근가능


![image](https://github.com/user-attachments/assets/29291978-b361-443a-94da-890a114b46c5)


 ```javascript
export default function ArchiveLayout({ archive, latest }) {
  return (
    <div>
      <h1>News Archive</h1>
      <section id="archive-filter">{archive}</section>
      <section id="archive-latest">{latest}</section>
    </div>
  );
}
```

- 병렬 라우트를 다룰 때, 동일한 페이지에 표시되는 병렬 라우트는 전부 원하는 경로를 모두 지원해야 합니다.
- 병렬 라우트에서 만약 한 라우트만 경로를 이동하고, 다른 페이지는 이동하고 싶지 않으면 `default.js` 파일을 만들면 된다.
- `default` 파일은 병렬 라우트에서 현재 로딩된 경로에 대해 구체적인 콘텐츠가 없다면 표시할 기본 `fallback` 콘텐츠를 정의하도록 허용하는 파일이다.

![image](https://github.com/user-attachments/assets/5da618e9-5159-45b7-8046-b81d8c267360)


```javascript
import NewsList from "@/components/news-list";
import { getLatestNews } from "@/lib/news";

export default function LatestNewsPage() {
  const latestNews = getLatestNews();
  return (
    <>
      <h1>Latest News</h1>
      <NewsList news={latestNews} />
    </>
  );
}
```


## Next.js란

Next.js는 서버 사이트 렌더링, 정적 웹 페이지 생성 등 리액트 기반 웹 애플리케이션 기능들을 가능케 하는 Node.js 위에서 빌드된 오픈 소스 웹 개발 프레임워크이다. Next.js는 웹팩, 바벨, 타입스크립트 등을 포함하여 개발 환경을 제공하고, 이를 통해 빠르고 효율적인 개발이 가능합니다.
특히, SEO를 위한 SSR를 가능하게 해주는 프레임워크입니다.

**SEO**: SEO란, Search Engine Optimization의 약자로, SEO는 구글, 네이버와 같은 검색 엔진들이 서버에 등록된 웹사이트를 하나하나씩 돌아 다니면서 웹사이트의 HTML 문서를 분석해주는 것 (사이트를 검색 엔진 결과에 잘 노출 시킬 수 있도록 함)
**SSR**: ssr이란 서버사이드 렌더링의 약자로 서버로부터 완전하게 만들어진 html파일을 받아와 페이지 전체를 렌더링 하는 방식

> 💡 Next.js 동작과정- 초기화면 수행 (SSR 방식 이용)
1. 사용자가 홈페이지를 접속한 경우(최초 접속) 클라이언트는 이를 확인하고 서버로 요청 합니다.(Server-Side)
2. 서버에서는 미리 구성된 HTML, CSS 파일을 클라이언트에게 전달합니다.(Pre-Rendering - init Load)
(* Initial Load : JS 동작이 없는 HTML을 먼저 화면에 보여주는것을 의미)
3. 이 과정에서 동시에 클라이언트에서는 스크립트 파일을 수행하여서 클라이언트에게 전달합니다.(Hydration)
(* Hydration : Initial Load 이후 JS 파일을 HTML에 연결하는 과정을 의미)


---

## Next.js 공식문서

### 기본기능
- 직관적인 페이지 기반 라우팅 시스템
- 사전 렌더링 , 정적 생성 (SSG) 및 서버 측 렌더링 (SSR)
- 더 빠른 페이지 로드를 위한 자동 코드 분할
- 최적화된 prefetching를 사용한 클라이언트측 라우팅
- 내장 CSS 및 Sass 지원 및 모든 CSS-in-JS 라이브러리 지원
- Fast Refresh를 지원 하는 개발 환경

### SSG vs SSR
Next.js는 SSG(Static Generation), SSR(Server-side Rendering) 두 가지의 사전 렌더링 방식을 제공한다. 하이브리드 형태로 두 가지 사전 렌더링 방식을 모두 적용할 수 있으나, 공식 도큐에서는 **SSG 사용을 추천**한다.

**Static-Generation(SSG)** : HTML을 빌드 타임에 각 페이지별로 생성하고 해당 페이지로 요청이 올 경우 이미 생성된 HTML 문서를 반환
- 페이지에 대한 유저들의 요청이 발생하게 되면, 요청에 따라 계속 서버에서 재생성 하는 것이 아니라 이미 생성이 완료된 페이지를 반환
- 생성이 완료된 HTML 문서를 재활용 하기에 응답 속도가 매우 빠르다
- 마케팅 페이지 / 블로그 게시물 / 제품의 목록 등과 같이 정적 생성하여 각 요청에 동일한 문서를 반환할 수 있는 경우 사용

**Server-Side-Rendering(SSR)** : 요청이 올 때 마다 런타임에 해당하는 HTML 문서를 그때 그때 생성하여 반환
- 페이지 데이터가 항상 최신의 상태를 유지할 수 있음
- 페이지가 동적 이기 때문에 사용자에게 더 인터렉티브한 반응을 할 수 있음
- 제품의 상세 페이지 / 분석 차트 등 요청 마다 다른 내용 또는 형식의 HTML 문서가 반환되는 경우


---
### Create a Next.js app
프로젝트 생성
```
npx create-next-app@latest nextjs-blog --use-npm --example "https://github.com/vercel/next-learn/tree/master/basics/learn-starter"
```
---
### Navigate Between Pages
- 페이지는 파일 이름을 기반으로 경로와 연결
- Link태그를 통해 페이지 이동이 가능
- Next.js는 자동으로 코드 분할을 수행하므로 각 페이지는 해당 페이지에 필요한 항목만 로드(code splitting)
- Link태그 사용시에는 링크된 페이지에 대한 코드를 자동으로 미리 가져옴(prefetching)
![](https://velog.velcdn.com/images/guddyd6761/post/de8a27cb-6b99-44b4-af83-8a2439f3894f/image.png)
![](https://velog.velcdn.com/images/guddyd6761/post/89961d69-1009-46be-9d16-05f93b26cd02/image.png)

```javascript
import Link from 'next/link';

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">Back to home</Link>
      </h2>
    </>
  );
}
```

**image optimizaion**
- public에 담아두고, next/image의 Image를 사용하면 img 태그를 사용하는 것보다 최적화를 해준다.

---
### Pre-rendering and Data Fetching
- next.js는 기본적으로 pre-renderig 적용
- SSG vs SSR
(build time vs each request)
- 데이터가 필요한 페이지에는 getStaticProps라는 async함수를 구현
- 사용자의 요청마다 외부 데이터를 가져올시에는 getServerSideProps 함수사용
![](https://velog.velcdn.com/images/guddyd6761/post/ca4c7e6c-d42a-4fd3-983b-aa96be3f94c1/image.png)
![](https://velog.velcdn.com/images/guddyd6761/post/1af4ba3a-5054-47b6-a9c7-434c32e440c9/image.png)

---

### Dynamic Routes
- 특정 페이지의 경로(paths)가 외부 데이터에 의존할 때, getStaticPaths를 통해 사전 렌더링을 실행할 수 있음
- 하기 예시는 posts폴더에 markdown형식의 파일을 만들고, 해당 파일을 remark라이브러리를 통해 Html형식으로 변환하여 사이트에 바인딩할 데이터를 파일에서 가져오는 형식으로 진행
md파일의 이름이 path가 되도록 함(ex - /posts/ssg-ssr , /posts/pre-rendering)
- [{params:{id:string}}]목록을 리턴하므로, params안에 dynamic routes key가 들어있어야함

![](https://velog.velcdn.com/images/guddyd6761/post/e8f5d6c3-7528-4ff9-92f0-ea857078b695/image.png)

```javascript
//id.js
export async function getStaticProps({ params }) {
  const postData = await getPostData(params.id);
  return {
    props: {
      postData,
    },
  };
}

export async function getStaticPaths() {
  const paths = getAllPostIds();
  return {
    paths,
    fallback: false,
  };
}

export default function Post({ postData }) {
    return (
      <Layout>
        <Head>
          <title>{postData.title}</title>
        </Head>
        <article>
          <h1 className={utilStyles.headingXl}>{postData.title}</h1>
          <div className={utilStyles.lightText}>
          </div>
          <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
        </article>
      </Layout>
    );
  }
```
```javascript
//post
export function getAllPostIds() {
    const fileNames = fs.readdirSync(postsDirectory);
  
    return fileNames.map((fileName) => {
      return {
        params: {
          id: fileName.replace(/\.md$/, ''),
        },
      };
    });
  }

  export async function getPostData(id) {
    const fullPath = path.join(postsDirectory, `${id}.md`);
    const fileContents = fs.readFileSync(fullPath, 'utf8');
  
    // Use gray-matter to parse the post metadata section
    const matterResult = matter(fileContents);
  
    // Use remark to convert markdown into HTML string
    const processedContent = await remark()
      .use(html)
      .process(matterResult.content);
    const contentHtml = processedContent.toString();
  
    // Combine the data with the id and contentHtml
    return {
      id,
      contentHtml,
      ...matterResult.data,
    };
  }

```

- getStaticPaths 리턴 값에 fallback이 false이면 잘못된 경로로 접근시 지정된 404페이지를 불러오지만, fallback이 true로 변경하면 404페이지를 커스터마이징 할 수 있음

```javascript
// pages/404.js
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>;
}
```




---
### 출처
> [https://velog.io/@khy226/Next.js-%EB%9E%80-oxp9y4fg](https://velog.io/@khy226/Next.js-%EB%9E%80-oxp9y4fg)
[https://adjh54.tistory.com/52](https://adjh54.tistory.com/52)
[Nextjs공식문서](https://nextjs.org/)

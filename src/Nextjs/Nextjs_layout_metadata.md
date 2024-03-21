# layout, metadata

## 목차

1. [layout의 필요성](#1-layout의-필요성)
    1. [layout의 동작](#1-1-layout의-동작)
    2. [Navigation 컴포넌트 layout에 넣기](#1-2-navigation-컴포넌트-layout에-넣기)
    3. [커스텀 layout](#1-3-커스텀-layout)
        - [about-us 페이지를 위한 layout 만들기](#--about-us-페이지를-위한-layout-만들기)
    4. [route groups](#1-4-route-groups)
2. [Metadata](#2-metadata)
    1. [metadata 템플릿](#2-1-metadata-템플릿)
    2. [Dynamic Metadata - generateMetadata()](#2-2-dynamic-metadata---generatemetadata)

<br>
<br>

## 1. layout의 필요성

- 애플리케이션을 빌드할 때, 재사용하는 요소(element)들이 있기 때문
- ex) Navigation 바, Footer 등...
- 만약, 페이지가 무수히 많은 경우, 해당 컴포넌트를 수동으로 복사, 붙여넣기하는 것은 매우 비효율적임

<br>

### 1-1. layout의 동작

![layout](../assets/img/Nextjs_layout.png)

- url 요청을 받으면 Next.js는 제일 먼저 layout.tsx 파일을 조회함
- 해당 모듈로 구조화함
- 이후, 요청받은 url을 확인하여 렌더링이 필요한 페이지를 앞선 모듈의 자식 컴포넌트로 넣어 페이지를 렌더링하는 방식으로 동작함

<br>

### 1-2. Navigation 컴포넌트 layout에 넣기

```tsx
// app/layout.tsx

import Navigation from "../components/navigation";


export default function RootLayout({children}: { children: React.ReactNode }) {
    return (
        <html lang="en">
        <body>
        <Navigation/>
        {children}
        </body>
        </html>
    );
}
```

- layout에 Navigation 컴포넌트를 넣음으로써 모든 페이지에서 렌더링 시, Navigation 컴포넌트를 포함하게 됨

<br>

### 1-3. 커스텀 layout

- 홈페이지에는 단일한 layout이 아닌 상황에 따른 `여러 개의 layout`을 사용
- ex) 홈페이지에 있는 유저를 위한 레이아웃, 사용자가 대시보드에 있을 때 필요한 레이아웃 등...

<br>

### - about-us 페이지를 위한 layout 만들기

```tsx
// app/about-us/layout.tsx

export default function RootLayout({children}: { children: React.ReactNode }) {
    return (
        <div>
            {children}
            &copy; Next JS is great!
        </div>
    );
}
```

- 이렇게 하면 about-us 페이지에서만 copyright가 표시 됨
- layout은 서로 상쇄되는 것이 아닌 상위 폴더의 layout과 하위 폴더의 layout이 `중첩되어 렌더링` 됨

<br>

### 1-4. route groups

![route groups](../assets/img/Nextjs_route_group.png)

<route groups인 (home) 폴더 생성>

- 폴더에 소괄호를 사용하여 이름을 묶어줌
- 이렇게 하면 url에 영향을 주지않음
- 프레임워크가 구분하지 못하게 함

<br>
<br>

## 2. Metadata

- 반드시 내보내야하는 object를 Metadata라고 함
- \<head> 태그는 Metadata를 찾고 해당 내용을 탭 title과 description으로 사용

```tsx
// app/(home)/page.tsx

export const metadata = {
    title: "Home | Next Movies",
};
...

// ----------------------------------------------

// app/layout.tsx

export const metadata = {
    description: "The best movies on the best framework",
};
...
```

- metadata는 필요한 페이지 혹은 레이아웃에 `커스텀`하여 사용할 수 있음
- 중첩된 레이아웃의 경우, metadata가 `병합(merged)`되어 표시됨
- metadata는 내보내거나 가져와서 사용하는 것이 아니며, Sever 컴포넌트에서만 있을 수 있음 (Client 컴포넌트는 metadata 정의 안 됨)

<br>

### 2-1. metadata 템플릿

- metadata의 특정 부분은 공통이고, 일부는 변경되는 경우가 있음
- 이 경우, 상위 layout에서 `템플릿`을 작성하고 변경되는 부분은 하위의 해당 layout 또는 page에서 작성

<br>

```tsx
// 만약 home 페이지의 metadata의 title이 아래와 같고
export const metadata = {
    title: "Home | Next Movies",
};

// about-us 페이지의 metadata의 title이 아래와 같다면
export const metadata = {
    title: "About us | Next Movies",
};
```

- title에서 `| Next Movies` 앞 부문만 바뀌고 "| Next Movies" 부분은 `공통`인 것을 볼 수 있음
- 이러한 상황에서 metadata의 템플릿을 사용할 수 있음

<br>

```tsx
// 상위 layout에서 metadata 템플릿 적용

// app/layout.tsx
import {Metadata} from "next";

export const metadata: Metadata = {
    title: {
        template: "%s | Next Movies",
        default: "Next Movies.",
    },
    description: "The best movies on the best framework",
};


// 하위 page, layout에서 변경되는 값 받기

// app/(home)/page.tsx
export const metadata = {
    title: "Home",
};


// app/about-us/layout.tsx
export const metadata = {
    title: "About us",
};
```

- `TypeScript`의 경우, Next.js에서 제공하는 `Metadata 타입`을 사용
- title에 객체로 `template`과 `기본 값(default)`을 넣어준다.
- template에서 `%s`은 하위 page나 layout에서 `title 문자열`을 받고 그 외의 부분("| Next Movies")은 공통으로 작성됨
- 만약 title이 작성되지 않은 page나 layout에서는 기본 값이 출력됨

<br>

### 2-2. Dynamic Metadata - generateMetadata()

- Home(index) 페이지, 소개 페이지(about-us)의 경우, 동적이지 않고 하나의 제목만 가지고 있음
- 하지만, 영화 디테일 페이지와 같은 경우, [id]에 따라 각각의 제목이 필요함
- `generateMetadata 함수`를 사용하여 해결할 수 있음

```tsx
// app/(movies)/movies/[id]/page.tsx

...

import {getMovie} from "../../../../components/movie-info";

interface IParams {
    params: { id: string };
}

export async function generateMetadata({params: {id},}: IParams) {
    const movie = await getMovie(id);
    return {
        title: movie.title,
    }
};
...
```

- 프레임워크가 해당 함수를 찾아 metadata를 동적으로 적용해줌
- 따라서 `export` 해주어야 함
- 해당 함수에서 getMovie() 함수를 통해 데이터를 fetch하는데 movie-info 컴포넌트에서 역시도 getMovie() 함수를 사용하고 있어 속도가 느려질 수도 있다는 우려있을 수 있지만,
  Next.js에서 `fetch된 데이터를 캐싱하고 있어 속도에 문제없음`
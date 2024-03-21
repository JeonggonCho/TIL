# Suspense

## 목차

1. [Suspense의 필요성](#1-suspense의-필요성)
2. [Suspense 적용하기](#2-suspense-적용하기)
    1. [fetch 분리하기](#2-1-fetch-분리하기)

<br>
<br>

## 1. Suspense의 필요성

```tsx
// app/(movies)/movies/[id]/page.tsx

export default async function MovieDetail({ params: { id } }: { params: { id: string } }) {
    const [movie, videos] = await Promise.all([getMovie(id), getVideos(id)]);
    return <h1>{movie.title}</h1>;
}
```

- 위와 같이 Promise.all()을 사용할 경우, fetch가 끝나고 movie와 video 모두가 생성되어야 UI를 볼 수 있음
- 동시에 처리하더라도 fetch 함수를 분리하여 `먼저 처리가 끝난 요소`는 화면에 `먼저 출력`되도록 최적화 할 수 있음 (기다릴 필요 없음)
- streaming을 사용하여 만약 movie가 준비되면 getVideo 함수가 로딩 중이더라도 해당 데이터와 연결된 UI가 사용자에게 보내지도록 처리
- Suspense 방법을 적용하는 것이 최선은 아니며, 많은 데이터 소스에서 fetch해야 할 상황에 따라서 선택하여 적용할 수 있음

<br>
<br>

## 2. Suspense 적용하기

### 2-1. fetch 분리하기

- fetch를 처리하고 해당 결과를 리턴할 `movie-videos.tsx`, `movie-info.tsx` 파일 생성
- 기존에 page.tsx 파일에 작성한 fetch 함수 가져오기

```tsx
// components/movie-videos.tsx

import {API_URL} from "../app/(home)/page";

async function getVideos(id: string) {
    await new Promise((response) => setTimeout(response, 3000));
    const response = await fetch(`${API_URL}/${id}/videos`);
    return response.json();
}

export default async function MovieVideos({id}: {id:string}) {
    const videos = await getVideos(id);
    return <h6>{JSON.stringify(videos)}</h6>
}
```

```tsx
// components/movie-info.tsx

import {API_URL} from "../app/(home)/page";

async function getMovie(id: string) {
    await new Promise((response) => setTimeout(response, 5000));
    const response = await fetch(`${API_URL}/${id}`);
    return response.json();
}

export default async function MovieInfo({id}: {id:string}) {
    const movie = await getMovie(id);
    return <h6>{JSON.stringify(movie)}</h6>
}
```

<br>

- 해당 컴포넌트 page.tsx에서 출력

```tsx
// app/(movies)/movies/[id]/page.tsx

import MovieVideos from "../../../../components/movie-videos";
import MovieInfo from "../../../../components/movie-info";
import {Suspense} from "react";


export default async function MovieDetail({ params: { id } }: { params: { id: string } }) {
    return <div>
        <Suspense fallback={<h1>Loading movie info</h1>}>
            <MovieInfo id={id} />
        </Suspense>
        <Suspense fallback={<h1>Loading movie videos</h1>}>
            <MovieVideos id={id} />
        </Suspense>
    </div>;
}
```

- 각 컴포넌트에 필요한 id Props를 전달
- react에서 제공하는 `Suspense 컴포넌트`를 사용하여 fetch 데이터를 반환하는 MovieInfo와 MovieVideos 컴포넌트를 `각각 감싼다.`
- Suspense component에는 `fallback`이라는 Props이 있고, fallback이 컴포넌트가 await되는 동안 표시할 메시지를 render 할 수 있도록 해줌
- 병렬적으로 2가지를 동시에 fetch하고 나머지를 기다리지 않아도 됨

<br>

![Suspense를 통한 개별 fetch](../../assets/img/Nextjs_suspense.gif)

<Suspense를 통한 개별 fetch>
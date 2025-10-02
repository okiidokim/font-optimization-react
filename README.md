<div align="center">
<img width="602" height="261" alt="image" src="https://github.com/user-attachments/assets/75967aca-be49-4090-8673-63d8b024d7cd" />
</div>

# 📌 프로젝트 소개
React에서 시스템 폰트 이외의 폰트를 사용할 때 가장 좋은 성능을 보이는 상황을 찾기 위한 프로젝트입니다.

### 스킬
![Typescript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/react-%2361DAFB.svg?style=for-the-badge&logo=react&logoColor=%23ffffff)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=Vite&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-grey?style=for-the-badge&logo=tailwind-css&logoColor=38B2AC)

### 사용한 폰트
Pretendard, 양진체

### 성능 확인 방법
1. Network 탭에서 폰트 파일 요청 순서 확인
2. Performance 탭에서 웹폰트의 Decoded Body와 Duration 및 Content Downloading 시간 확인
3. Lighthouse Mobile로 LCP, FCP 확인

# 📌 성능 측정 결과
| 종류 \ Lighthoue Mobile | FOUT | LCP | FCP |
|---| :---: | :---: | :---: | 
| [시스템 폰트] | X | 1.4s | 1.2s |
| [로컬 폰트] Pretendard.ttf 2개 | X | 29.1s | 29.0s |
| [로컬 폰트] Pretendard.ttf 2개 + swap | X | 29.1s | 29.0s |
| [로컬 폰트] Pretendard.ttf 2개 + preload | X | 29.1s | 1.2s |
| [로컬 폰트] Pretendard.otf 2개 + preload | X | 17.4s | 1.2s |
| [로컬 폰트] Pretendard.woff 2개 + preload | X | 12.9s | 1.2s |
| [로컬 폰트] Pretendard.woff2 2개 + preload | X | 9.5s | 1.2s |
| [로컬 폰트] Pretendard.subset.woff 2개 + preload | X | 4.8s | 0.8s |
| **[로컬 폰트] Pretendard.subset.woff2 2개 + preload** | **X** | **4.1s** | **0.8s** |
| [로컬 폰트] Pretendard.subset.woff2 9개 + preload | X | 14.6s | 1.2s |
| [로컬 폰트] Variable.ttf + preload | X | 34.4s | 1.2s |
| **[로컬 폰트] Variable.woff2 + preload** | **X** | **11.4s** | **1.2s** |
| [웹폰트] pretendard.min.css (@import) | 폰트 적용 안 됨 | - | - |
| [웹폰트] pretendard.min.css (`<link>`) | O | 1.5s | 1.5s |
| [웹폰트] pretendard-dynamic-subset.min.css (`<link>`) | 용량이 커서 패스 | - | - |
| [웹폰트] pretendard-dynamic-subset.min.css (`<link>`) + `<link rel="preconnect/>` | 용량이 커서 패스 | - | - |
| **[웹폰트] pretendardvariable.css (`<link>`) + `<link rel="preconnect/>`** | **O** | **1.5s** | **1.5s** |
| [웹폰트] pretendardvariable-dynamic-subset.min.min.css (`<link>`) + `<link rel="preconnect/>` | 용량이 커서 패스 | - | - |
| [로컬 폰트] 양진체.otf + localFont | X | 5.6s | 1.2s |
| [웹폰트] 양진체.woff + preload | X | 4.2s | 1.2s |
| [웹폰트] 양진체.woff + `@import` | X | 4.5s | 1.5s |
| **[로컬 폰트] 양진체.woff2 + localFont** [(폰트 변환 사이트 활용)](https://transfonter.org/) | **X** | **2.7s** | **1.2s** |

# 📌 결론

## 선택 : Pretendard.subset.woff2 + localFont
- 개인적으로 FOUT는 발생하지 않아야 한다고 생각
  - 번들 크기가 커지면 웹폰트보다 서브셋 로컬 폰트가 더 빠를 수 있음
  - 이때, 9개의 폰트 굵기가 모두 필요하다면 **Variable.woff2 + localFont**
- FOUT이 발생하는 것이 상관 없다면, **가변 웹폰트(pretendardvariable.min.css) **
- Pretendard는 폰트 변환 사이트를 통한 변환이 의미가 없음

## Pretendard가 아닌 경우 (like 양진체) : `.woff2` 로컬 폰트
- 확장자 우선순위 : `subset.woff2` > `subset.woff` > `.woff2` > `.woff` > `.otf` > `.ttf`
- `.woff`/`.woff2` 형식이 없는 경우, [폰트 변환 사이트](https://transfonter.org/)에서 `.woff2` 형식으로 바꿔서 로컬 폰트로 사용하기
- 변환하고 싶지 않은데 로컬 폰트는 `.otf`/`.ttf`만 제공하고 웹 폰트는 `.woff`/`.woff2`를 제공하는 경우, 웹폰트 선택
- 모든 두께를 전부 사용할 경우 : 가변 폰트 (제공하는 경우)
- 서너 개 이하로만 사용할 경우 : 필요한 폰트만 두께별로 설치
- 물론 개인의 결론일 뿐 상황이나 기획에 따라 선택해야 하는 방법은 다를 수 있음
- Next.js와는 달리 웹폰트 / 로컬 폰트 상관 없이 번들 청크에 영향을 받지 않아 항상 최우선으로 요청을 보낼 수 있다.
  - Vite는 개발자가 index.html에 작성한 `<head>` 내 순서대로 네트워크 요청이 간다.
  - 코드 스플리팅이 없으면 JS번들 파일 하나와 CSS 번들 파일 하나가 전부이며, 가장 마지막에 요청한다.


## 웹폰트를 사용해야 할 때 `<link rel="preload"/>` vs `@import`
- `<link rel="preload"/>`
- 네트워크 요청 우선 순위가 더 높기 때문
- css를 요청하는 경우, `@import`는 적용되지 않음

2. 웹폰트 / 로컬 폰트 상관 없이 번들 청크에 영향을 받지 않는다.
2-1. Vite는 개발자가 index.html에 작성한 `<head>` 내 순서대로 네트워크 요청이 간다.
2-2. 코드 스플리팅이 없으면 JS번들 파일 하나와 CSS 번들 파일 하나가 전부이며, 가장 마지막에 요청한다.

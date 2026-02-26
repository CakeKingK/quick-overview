1) Tailwind를 한 문장으로 이해하기

CSS를 미리 만들어둔 “유틸리티 클래스”를 조합해서 UI를 만든다
→ 즉, className="p-4 bg-white rounded-lg shadow" 이런 식으로 스타일을 클래스 조합으로 작성합니다.

2) 제일 자주 쓰는 문법 패턴 12개 (이거만 외우면 70% 끝)
A. 간격 (Spacing)

p-4 : padding 1rem

px-4 py-2 : x축 padding, y축 padding

m-4, mt-2, mb-6, gap-4(flex/grid 간격)

규칙: p|m + (t|r|b|l|x|y)? + -숫자

예)

pt-6 위 padding

mx-auto 좌우 margin 자동(가운데 정렬)

B. 크기 (Width/Height)

w-full 꽉

w-64 고정

max-w-md 최대폭 제한

h-screen 화면 높이

C. 글자 (Typography)

text-sm | text-base | text-lg | text-xl

font-medium | font-semibold | font-bold

text-gray-600 글자색

leading-relaxed 줄간격

tracking-tight 자간

D. 색상 (Color)

bg-blue-600 배경

text-white 텍스트

border-gray-200 테두리

규칙: bg-색-단계, text-색-단계, border-색-단계

E. 테두리/둥글기/그림자

border / border-2

rounded / rounded-lg / rounded-2xl

shadow / shadow-md / shadow-lg

F. 레이아웃 핵심 (Flex/Grid)

Flex

flex, items-center(세로 정렬), justify-between(가로 정렬)

gap-3 간격

flex-col 세로로 쌓기

Grid

grid, grid-cols-2, grid-cols-3

gap-4

3) 반응형(모바일/PC) 문법이 핵심 포인트

Tailwind 반응형은 “접두사:” 패턴입니다.

sm: (작은 화면 이상)

md: (중간 화면 이상)

lg: (큰 화면 이상)

xl: …

예)

<div className="grid grid-cols-1 md:grid-cols-2 gap-4">


모바일 1열 → md 이상 2열

4) 상태(hover/focus/disabled) 문법

hover:bg-blue-700

focus:outline-none focus:ring-2 focus:ring-blue-500

disabled:opacity-50 disabled:cursor-not-allowed

예)

<button className="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded disabled:opacity-50">
  저장
</button>

5) “이거 헷갈림” 1순위: 우선순위/충돌

클래스가 충돌하면 뒤에 있는 게 이기는 경우가 많고, 반응형/상태 접두사가 있으면 조건에 따라 바뀝니다.

예)
p-2 p-4 → 보통 p-4가 적용

6) 바로 써먹는 “레이아웃 공식” 4개
공식 1) 카드

bg-white rounded-lg shadow p-4 border border-gray-200

공식 2) 섹션 컨테이너(가운데 정렬)

max-w-4xl mx-auto px-4

공식 3) 버튼

px-4 py-2 rounded bg-blue-600 text-white hover:bg-blue-700

공식 4) 입력창

w-full border border-gray-300 rounded px-3 py-2 focus:ring-2 focus:ring-blue-500 focus:outline-none

7) 5분 실습 예제 (React 기준)

이거 그대로 붙여넣고 감 잡으시면 됩니다.

export default function Demo() {
  return (
    <div className="min-h-screen bg-gray-50">
      <div className="max-w-3xl mx-auto px-4 py-10">
        <h1 className="text-2xl font-bold tracking-tight">Tailwind 빠른 실습</h1>
        <p className="text-gray-600 mt-2">클래스 조합으로 UI를 만듭니다.</p>

        <div className="mt-8 grid grid-cols-1 md:grid-cols-2 gap-4">
          <div className="bg-white border border-gray-200 rounded-2xl shadow-sm p-5">
            <div className="flex items-center justify-between">
              <h2 className="font-semibold">카드 A</h2>
              <span className="text-xs bg-blue-50 text-blue-700 px-2 py-1 rounded-full">
                NEW
              </span>
            </div>
            <p className="text-sm text-gray-600 mt-2">
              padding, border, rounded, shadow 조합
            </p>

            <button className="mt-4 px-4 py-2 rounded-lg bg-blue-600 text-white hover:bg-blue-700">
              버튼
            </button>
          </div>

          <div className="bg-white border border-gray-200 rounded-2xl shadow-sm p-5">
            <label className="text-sm font-medium text-gray-700">이메일</label>
            <input
              className="mt-2 w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="name@company.com"
            />
            <button className="mt-4 w-full px-4 py-2 rounded-lg bg-gray-900 text-white hover:bg-black">
              로그인
            </button>
          </div>
        </div>
      </div>
    </div>
  );
}

8) 감을 확 잡는 학습 순서 (짧고 굵게)

Spacing(p/m/gap)

Flex로 정렬(items/justify)

Typography(text/font)

Card 공식(rounded/shadow/border)

상태(hover/focus)

반응형(sm/md/lg)

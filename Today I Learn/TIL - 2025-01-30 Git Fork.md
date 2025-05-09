# TIL - 2025-01-30 Git Fork
Fork 전략(Forking Strategy)은 오프소스 프로젝트나 협업 개발에서 사용되는 Git 브랜치 관리 방식 중 하나입니다. 

원본 저장소를 Fork(복사)해서 독립적인 저장소에서 개발한 후, Pull Request(PR)로 변경 사항을 반영하는 Git 협업 전략입니다.

- Fork → 원본 저장소를 내 계정으로 복사
- 개발 진행 → 내 Fork에서 수정 & 새로운 기능 개발
- Pull Request (PR) → 원본 저장소에 변경 사항 요청

## Fork 전략이 필요한 이유
1. 원본 저장소를 보호할 수 있습니다.
  - 오픈소스 프로젝트 경우 아무나 작업에 기여할 수 있기 때문에 원본 저장소는 안전하게 유지되고 변경 사항을 검토할 수 있습니다.
2. 권한 없이도 개발 참여가 가능합니다.
  - 원본 저장소를 직접 수정할 권한이 없다. 내 저장소에서 독립적으로 개발하고 PR을 보낼 수 있다.
3. 협업 시 코드 리뷰 & 품질 유지할 수 있습니다.
  - 여러 사람이 같은 프로젝트에 기여하면서 코드 품질을 유지할 수 있고, PR을 통해 코드 리뷰를 할 수 있습니다.

## Fork 방법

### 1. 원본 저장소를 Fork
- GitHub에서 기여하고 싶은 원본 저장소로 이동합니다.
- Fork 버튼을 클릭해 내 계정에 Forked Repository를 생성합니다.

### 2. Fork한 저장소를 로컬로 클론
```bash
git clone https://github.com/내-계정/내-포크한-레포지토리.git
cd 내-포크한-레포지토리
```
### 3. Upstream(원본 저장소) 추가
원본 저장소의 변경 사항을 반영하기 위해, Upstream을 추가합니다.
```bash
git remote add upstream https://github.com/원본-사용자/원본-레포지토리.git
```

### 4. 새로운 기능 개발 (브랜치 생성)
브랜치를 생성해서 추가할 기능에 대한 작업을 진행합니다.

### 5. 변경 사항을 Commit & Push
기존 방법과 동일하게 변경된 내용을 Commit & Push 합니다.

### 6. Pull Request(PR) 보내기
- 내 Fork 저장소에서 New Pull Request를 클릭합니다.
- 원본 저장소의 main <- 내 Fork의 브랜치로 PR 요청
- 원본 저장소 관리자가 코드 리뷰를 진행하고 병합합니다.

### 7. 원본 저장소 최신 변경 사항 반영
원본 저장소의 변경 사항을 내 Fork에도 반영합니다.
```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

## Fork 전략 vs Git Flow 비교
Fork 전략은 원본 저장소를 복사해 독립적으로 개발합니다. 주로 오픈소스, 외부 협업에서 많이 사용합니다.
Git Flow 전략은 같은 저장소에서 브랜치를 사용해 개발합니다. 내부 팀 개발에서 많이 사용합니다.

## Fork 전략을 사용할 때 주의할 점
- Upstream 최신 코드 반영을 자주 해야 합니다.
- PR을 보낼 때, 원본 저장소 코드 스타일에 맞춰야 합니다.
- 기능별로 브랜치를 만들어 작업해야 충돌을 줄일 수 있습니다. 

# Storybook

프론트엔드 라이브러리에서 개별 UI 컴포넌트를 독립적으로 개발하고 테스트할 수 있는 도구입니다.

애플리케이션을 실행하지 않아도 컴포넌트 별로 볼 수 있습니다.

## Storybook 을 사용하게 되면?

- 애플리케이션을 실행 해야만 UI 변경을 확인할 수 있었는데, 실행하지 개별 컴포넌트 별로 확인할 수 있습니다.
- UI 변경 사항을 빠르게 확인할 수 있다. 그래서 협업에 유리합니다.
- 문서화 기능이 포함되어 있어 협업에 유리합니다.
- 버그 추적에 용이합니다.
- 컴포넌트가 여러개 얽혀 있으면 테스트하기 어려운데, 하나의 컴포넌트만 테스트할 수 있는 장점이 있습니다.
- 

## Storybook 설치 및 실행
프로젝트에 Storybook 추가할 수 있습니다.

```bash
npx storybook@latest init
```
설치가 끝났다면, Storybook 실행할 수 있습니다.

```bash
npm run storybook
```

브라우저에서 해당 포트로 접속하면 Storybook 확인할 수 있습니다.
http://localhost:6006 

## Storybook 사용 예제

### 1. VisualItem 컴포넌트 만들기
src/components/VisualItem.tsx

예제로 VisualItem.tsx라는 컴포넌트를 만들어서 Storybook에서 확인해봅시다.

``` tsx
import React from "react";

interface VisualItemProps {
  icon: React.ReactNode;
  title: string;
  description: string;
}

const VisualItem: React.FC<VisualItemProps> = ({ icon, title, description }) => {
  return (
    <div className="flex flex-col items-center text-center bg-white p-6 rounded-lg shadow-md w-64">
      <div className="text-4xl mb-3">{icon}</div>
      <h3 className="text-lg font-semibold mb-1">{title}</h3>
      <p className="text-sm text-gray-600">{description}</p>
    </div>
  );
};

export default VisualItem;
```
Props를 받아서 아이콘, 제목, 설명을 표시하는 컴포넌트 입니다.

### 2. VisualItem 컴포넌트 Story 파일 만들기
src/components/VisualItem.stories.tsx

```tsx
import React from "react";
import { Meta, StoryObj } from "@storybook/react";
import VisualItem from "./VisualItem";
import { FaMicrophone } from "react-icons/fa";

const meta: Meta<typeof VisualItem> = {
  title: "Components/VisualItem", 
  component: VisualItem,
  args: {
    icon: <FaMicrophone />,
    title: "발표와 나눔",
    description: "파이썬 사용자들이 발표하고 지식을 나눌 수 있습니다.",
  },
};

export default meta;

export const Default: StoryObj<typeof VisualItem> = {};
```

title: "Components/VisualItem" → Storybook 내에서 “Components” 카테고리에 정리됨
args를 사용해서 기본 Props 값 지정 → Storybook UI에서 쉽게 값 변경 가능

### 3. Storybook 실행해서 확인
```
npm run storybook
```

### 4. Storybook 에서 다양한 상태 테스트
여러가지 상태의 Story도 추가해서 테스트 할 수 있습니다.

```tsx
export const Primary: StoryObj<typeof VisualItem> = {
  args: {
    icon: <FaMicrophone />,
    title: "🎤 발표 모드",
    description: "이 모드에서는 발표자가 화면을 공유합니다.",
  },
};

export const Warning: StoryObj<typeof VisualItem> = {
  args: {
    icon: <FaMicrophone />,
    title: "⚠️ 마이크 문제",
    description: "마이크 연결이 감지되지 않았습니다.",
  },
};
```

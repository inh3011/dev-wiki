# TIL - 2025-02-01 Tailwind CSS

# Tailwind CSS
오늘 `VisualSection`과 `VisualSectionItem`을 만들면서 사용한 **Tailwind CSS 클래스 & CSS 스타일**을 정리합니다.

---

## 레이아웃 관련 (Flex & Grid)
| **CSS** | **Tailwind CSS** | **설명** |
|---------|-----------------|----------|
| `display: flex;` | `flex` | Flexbox 레이아웃 적용 |
| `flex-direction: column;` | `flex-col` | 세로 방향 정렬 (column) |
| `align-items: center;` | `items-center` | 가로축 기준 중앙 정렬 |
| `text-align: center;` | `text-center` | 텍스트 중앙 정렬 |
| `display: grid;` | `grid` | CSS Grid 레이아웃 적용 |
| `grid-template-columns: repeat(3, 1fr);` | `grid-cols-3` | 3개의 컬럼으로 나누기 |
| `gap: 2rem;` | `gap-8` | 요소 간 간격(8px) 추가 |
| `max-width: 1152px;` | `max-w-6xl` | 최대 너비 6XL(1152px) |

---

## 배경 및 테두리 (Border & Background)
| **CSS** | **Tailwind CSS** | **설명** |
|---------|-----------------|----------|
| `background-color: #f3f4f6;` | `bg-gray-100` | 연한 회색 배경 적용 |
| `background-color: white;` | `bg-white` | 흰색 배경 적용 |
| `border: 2px solid red;` | `border-2 border-red-500` | 빨간색 테두리 (디버깅용) |
| `border-right: 2px solid #d1d5db;` | `border-r-2 border-gray-300` | 오른쪽 경계선 추가 |
| `border-radius: 8px;` | `rounded-lg` | 박스 모서리를 둥글게 |
| `box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);` | `shadow-md` | 박스 그림자 효과 추가 |

---

## 텍스트 관련 (Typography)
| **CSS** | **Tailwind CSS** | **설명** |
|---------|-----------------|----------|
| `font-size: 2.25rem;` | `text-4xl` | 아이콘 크기 크게 설정 |
| `font-size: 1.125rem; font-weight: 600;` | `text-lg font-semibold` | 제목 크기 및 굵기 설정 |
| `font-size: 0.875rem; color: #6b7280;` | `text-sm text-gray-600` | 작은 크기의 회색 텍스트 적용 |
| `white-space: pre-line;` | `whitespace-pre-line` | 줄바꿈(`\n`) 적용 가능 |

---

## 위치 및 정렬 (Position & Spacing)
| **CSS** | **Tailwind CSS** | **설명** |
|---------|-----------------|----------|
| `position: relative;` | `relative` | 기준 위치 설정 |
| `position: absolute;` | `absolute` | 부모 요소 기준으로 절대 위치 배치 |
| `left: 0;` | `left-0` | 왼쪽 끝에 정렬 |
| `top: 50%; transform: translateY(-50%);` | `top-1/2 transform -translate-y-1/2` | Y축 기준 중앙 정렬 |
| `width: 50%; height: 400px;` | `w-1/2 h-[400px]` | 너비 50%, 높이 400px 설정 |
| `opacity: 0.5;` | `opacity-50` | 반투명 효과 (50%) |
| `z-index: 10;` | `z-10` | 배경보다 위로 배치 |

---

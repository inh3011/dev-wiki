# TIL - 2025-02-06 React useState()

## 1. 왜 useState가 필요한가?
- 함수형 컴포넌트에서 상태 관리: 기존에는 클래스 컴포넌트에서만 상태를 관리할 수 있었지만, useState를 사용하면 함수형 컴포넌트에서도 상태를 쉽게 관리할 수 있습니다.
- 간결한 코드 작성: useState는 클래스 컴포넌트의 복잡한 this 바인딩 문제를 해결하고, 간단하게 상태와 상태를 변경하는 함수를 사용할 수 있게 해줍니다.
- 동적인 UI 업데이트: 사용자 인터랙션에 따라 UI가 동적으로 변경되어야 할 때, useState를 사용하여 상태 변화를 감지하고 필요한 부분만 다시 렌더링할 수 있습니다.

## 2. 사용 예시
- Header 컴포넌트를 보면, 다음과 같이 두 개의 드롭다운 메뉴(예: HOME, ABOUT)를 관리하기 위해 useState를 사용하고 있습니다.
- 이렇게 하면 HOME 드롭다운의 상태는 반전되며, 동시에 ABOUT 드롭다운은 닫히게 됩니다.
```tsx
import * as React from "react";
import HeaderMenu from "@/components/Header/HeaderMenu.tsx";
import { FaChevronDown } from "react-icons/fa";
import { useState } from "react";

const Header: React.FC = () => {
    // 드롭다운 표시 여부를 위한 상태 변수 선언
    const [showHomeDropdown, setShowHomeDropdown] = useState(false);
    const [showAboutDropdown, setShowAboutDropdown] = useState(false);

    const menuItems = [
        { label: "HOME", hasDropdown: true },
        { label: "ABOUT", hasDropdown: true },
        { label: "NEWS", hasDropdown: false },
        { label: "SUPPORT", hasDropdown: false },
        { label: "COC", hasDropdown: false },
    ];

    return (
        <div className="w-full h-[78px] px-[169px] bg-white flex items-center">
            {/* 로고 영역 */}
            <img
                className="w-[145px] h-[78px] object-contain flex-shrink-0"
                src="https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F6324be2c-a8b1-4af8-b50e-19249d994d67%2Ffa28ddd3-ea28-4898-990c-2f1934ae80cf%2FKakaoTalk_Photo_2024-09-23-02-35-01_004.png&blockId=1091cd0c-2cb8-80dd-9743-e68459de2ac4&width=256"
                alt="Logo"
            />

            {/* 네비게이션 메뉴 */}
            <nav className="flex ml-[795px] gap-10 relative">
                {menuItems.map(({ label, hasDropdown }) => (
                    <div
                        key={label}
                        className="relative flex items-center gap-2 cursor-pointer"
                        onClick={() => {
                            // 각 메뉴를 클릭할 때 해당 드롭다운만 토글하고, 다른 메뉴는 닫음
                            if (label === "HOME") {
                                setShowHomeDropdown(!showHomeDropdown);
                                setShowAboutDropdown(false);
                            } else if (label === "ABOUT") {
                                setShowAboutDropdown(!showAboutDropdown);
                                setShowHomeDropdown(false);
                            }
                        }}
                    >
                        <HeaderMenu label={label}/>
                        {hasDropdown && <FaChevronDown className="text-gray-600"/>}
                        {label === "HOME" && showHomeDropdown && (
                            <div className="absolute top-full left-1/2 -translate-x-1/2 mt-2 w-[122px] h-[47px] px-[33px] py-3.5 bg-white rounded-md shadow-lg flex justify-center items-center">
                                <div className="text-black text-base font-medium">공지사항</div>
                            </div>
                        )}

                        {label === "ABOUT" && showAboutDropdown && (
                            <div className="absolute top-full left-1/2 -translate-x-1/2 mt-2 w-[122px] flex flex-col shadow-lg">
                                <div className="h-[47px] px-[34px] py-3.5 bg-white rounded-t-md flex justify-center items-center">
                                    <div className="text-black text-base font-medium">Identity</div>
                                </div>
                                <div className="h-[47px] px-[34px] py-3.5 bg-white rounded-b-md flex justify-center items-center">
                                    <div className="text-black text-base font-medium">MD</div>
                                </div>
                            </div>
                        )}
                    </div>
                ))}
            </nav>
        </div>
    );
};

export default Header;
```

## 코드 설명
### 1. 상태 변수 선언
```tsx
const [showHomeDropdown, setShowHomeDropdown] = useState(false);
const [showAboutDropdown, setShowAboutDropdown] = useState(false);
```
- 두 상태 변수는 각각 HOME과 ABOUT 메뉴의 드롭다운이 열려 있는지를 관리합니다. 초깃값은 false로 설정되어 있어, 기본적으로 드롭다운이 닫혀 있습니다.
### 2. 상태 업데이트
- 메뉴 항목을 클릭할 때, 해당 메뉴의 드롭다운 상태를 토글합니다. 예를 들어, HOME 메뉴를 클릭하면:
```tsx
setShowHomeDropdown(!showHomeDropdown);
setShowAboutDropdown(false);
```
- 이렇게 하면 HOME 드롭다운의 상태는 반전되며, 동시에 ABOUT 드롭다운은 닫히게 됩니다.


### 3. 조건부 렌더링
- 상태 변수에 따라 드롭다운 UI를 조건부로 렌더링합니다.
- label === "HOME" && showHomeDropdown 조건이 참일 때에만 HOME 드롭다운 영역이 보입니다.
- label === "ABOUT" && showAboutDropdown 조건이 참일 때에만 ABOUT 드롭다운 영역이 보입니다.

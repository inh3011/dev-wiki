---
tags:
  - TIL
  - learning
  - model-context-protocol
  - mcp-obsidian
created: 2025-05-03
---
# TIL - 2025-05-03 mcp-obsidian 연결하기

## 오늘 배우는 개념 (What I Learned)

- **개념명 또는 기술 키워드**: `mcp-obsidian`, `Model Context Protocol`, `Claude 연동`
    
- 📖 간단한 정의:  
    → `mcp-obsidian`은 [Local REST API](https://github.com/coddingtonbear/obsidian-local-rest-api)를 통해 Obsidian 노트에 접근할 수 있도록 해주는 **MCP 서버**로, Claude 같은 AI 도구가 Obsidian 노트를 읽고 수정할 수 있도록 중계하는 역할을 한다.


---

## 왜 중요한가? (Why It Matters)

- Claude 같은 AI 도구가 Obsidian 노트와 직접 연결되어, 개인 지식 관리 시스템이 지능화될 수 있음
    
- Obsidian 로컴 데이터를 기반으로 검색·요약·폼지 등을 Claude에게 요청할 수 있어 활용도가 크게 높음
    
- MCP 서버를 활용한 구조 덕분에 로컴에서 보안이 유지된 차례 AI와 상호작용 가능
    
- `.env`를 활용하면 환경 설정을 간각하고 안전하게 유지할 수 있어 실사용에 적합함
    

---

## 적용 과정 (How I Applied It)

1. `mcp-obsidian` GitHub 저장소를 로컴으로 clone:
    
    ```bash
    git clone https://github.com/MarkusPfundstein/mcp-obsidian.git
    cd mcp-obsidian
    ```
    
2. `uv` 설치:
    
    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    export PATH="$HOME/.cargo/bin:$PATH"  # ~/.zshrc에 추가함
    ```
    
3. Obsidian에서 Local REST API 플랫셔링 설치 및 API Key 확인

4. 클론 받은 `mcp-obsidian` 디텍토리에 `.env` 파일 생성:
    
    ```env
    OBSIDIAN_API_KEY=your_api_key_here
    ```
    
5. Claude 설정 파일에 MCP 서버 정의:
    
    ```json
    {
	  "mcpServers": {
	    "mcp-obsidian": {
	      "command": "uv",
	      "args": [
	        "--directory",
	        "<dir_to>/mcp-obsidian",
	        "run",
	        "mcp-obsidian"
	      ]
	    }
	  }
	}
    ```
    
6. Claude를 껐다가 다시 실행하면 `mcp-obsidian` 을 사용할 수 있습니다.
    

---

## 다음 학습 주제 (What's Next)

- Claude와 연결된 상황에서 Obsidian 노트 검색 및 요약 기능 이용해보기
    
- Model Context Protocol을 지원하는 다른 AI 도구들과의 연동 해보기

## 참고 자료

- [MCP server for Obsidian](https://github.com/MarkusPfundstein/mcp-obsidian)  
- [uv](https://github.com/astral-sh/uv)  
- [실습영상](https://www.youtube.com/watch?v=53JJqJLGS-4&t=89s)
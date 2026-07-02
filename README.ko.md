<!-- 언어: [English](README.md) · **한국어** -->

# wbsgantt MCP

[wbsgantt](https://wbsgantt.com) 를 **Claude·ChatGPT 같은 LLM 클라이언트와 대화하면서** 직접 조작하는 [MCP(Model Context Protocol)](https://modelcontextprotocol.io) 커넥터 가이드입니다. WBS를 만들고, 노드·전제작업(종속성)·작업 정의서를 대화로 편집할 수 있습니다.

> 이 저장소는 **연결 방법·도구 카탈로그·예시**만 담습니다. 서버는 wbsgantt가 호스팅하는 **Remote MCP** 라 여러분이 설치하거나 실행할 것은 없습니다.

## ✨ 무엇을 할 수 있나

- "이 요구사항으로 새 프로젝트 WBS를 만들어줘" — 프로젝트 통째 생성
- "○○ 프로젝트의 WBS를 보여줘" — 트리 조회
- "○○ 아래에 '설계' 단계와 하위 작업 3개를 추가해줘" — 노드 추가·수정·이동·삭제
- "화면 설계가 끝나야 API 설계가 시작되게 연결해줘" — 전제작업(FS/SS/FF/SF·Lead/Lag)
- "이 작업의 정의·산출물·완료 기준을 채워줘" — 작업 정의서(Dictionary)

## 🚀 3단계 연결 (설치 불필요)

1. **토큰 발급** — wbsgantt 로그인 → 우상단 프로필 메뉴 → **계정 설정 → API 토큰** 에서 발급 (권한: *읽기 전용* 또는 *읽기+쓰기*). 토큰은 **발급 직후 1회만** 표시되니 복사해 두세요.
2. **커넥터 등록** — 아래 MCP 서버 주소와 토큰을 자신의 LLM 클라이언트에 등록합니다.

   ```
   https://api.wbsgantt.com/mcp/
   ```
   > ⚠️ 주소 **끝의 `/` 를 반드시 포함**하세요.

3. **대화 시작** — "내 wbsgantt 프로젝트 보여줘" 처럼 자연어로 요청하면 됩니다.

클라이언트별 상세 등록법은 [docs/connect.ko.md](docs/connect.ko.md) 를 보세요.

## 📚 문서

| 문서 | 내용 |
|---|---|
| [docs/connect.ko.md](docs/connect.ko.md) | claude.ai · Claude Desktop · Claude Code · ChatGPT 연결법 |
| [docs/tools.ko.md](docs/tools.ko.md) | 사용 가능한 도구 카탈로그(권한·파라미터) |
| [docs/troubleshooting.ko.md](docs/troubleshooting.ko.md) | 인증 오류·도구가 안 보일 때·토큰 만료 |
| [examples/prompts.ko.md](examples/prompts.ko.md) | 실전 프롬프트 모음 |
| [examples/sample-project.json](examples/sample-project.json) | 프로젝트 생성용 예시 문서 |
| [examples/wbs-input.schema.json](examples/wbs-input.schema.json) | 프로젝트 입력 JSON 규격(JSON Schema) |

## 🔒 보안·권한

- LLM은 **토큰을 발급한 사용자 본인** 권한으로만 동작합니다 — 여러분이 웹에서 볼 수 있는 프로젝트만 조회·편집합니다.
- 토큰은 발급일로부터 **90일 후 자동 만료**, 사용자당 최대 10개. 유출 의심 시 계정 화면에서 즉시 **폐기**하세요.
- wbsgantt의 규칙(마일스톤 기간 0·순환 종속성 차단·100% Rule 등)은 서버에서 검증되어, 잘못된 편집은 오류로 거부되고 LLM이 스스로 바로잡습니다.

## 💬 피드백

버그·기능 요청은 [Issues](../../issues) 로 남겨주세요.

---
문서·예시는 [MIT License](LICENSE) 로 제공됩니다. wbsgantt 서비스 자체 이용은 [wbsgantt.com](https://wbsgantt.com) 약관을 따릅니다.

<!-- 언어: [English](connect.md) · **한국어** -->

# 클라이언트별 연결 방법

MCP 서버 주소는 모두 동일합니다 (**끝의 `/` 필수**):

```
https://api.wbsgantt.com/mcp/
```

인증은 발급한 API 토큰(`bwic_pat_...`)을 **Bearer 토큰**으로 전달합니다. 토큰 발급은 [README](../README.ko.md#-3단계-연결-설치-불필요) 참고.

---

## claude.ai / Claude Desktop

1. 설정(Settings) → **Connectors**(커넥터) → **Add custom connector**.
2. 입력:
   - **URL**: `https://api.wbsgantt.com/mcp/`
   - **Authorization** 헤더: `Bearer <발급받은_토큰>` (또는 토큰 입력란에 붙여넣기)
3. 저장 후 새 대화에서 도구가 나타납니다.

## Claude Code

터미널에서:

```bash
claude mcp add --transport http wbsgantt https://api.wbsgantt.com/mcp/ \
  --header "Authorization: Bearer <발급받은_토큰>"
```

등록 확인:

```bash
claude mcp list
```

## ChatGPT (커스텀 커넥터)

1. 설정에서 **커스텀 커넥터/도구** 추가.
2. **MCP Server URL**: `https://api.wbsgantt.com/mcp/`
3. 인증: `Authorization: Bearer <발급받은_토큰>`

---

## 연결 확인

새 대화에서 다음을 물어보세요:

> "wbsgantt에서 지금 쓸 수 있는 도구 알려줘"

`list_projects`·`get_wbs_tree`·`add_wbs_dependency`·`set_wbs_dictionary` 등이 보이면 정상입니다.
목록이 오래됐거나 도구가 안 보이면 [troubleshooting](troubleshooting.ko.md) 을 참고해 커넥터를 재연결하세요.

> 참고: 토큰이 만료(90일)되거나 폐기되었을 때만 새 토큰으로 다시 등록하면 됩니다. 평소에는 재설정이 필요 없습니다.

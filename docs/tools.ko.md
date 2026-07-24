언어: [English](tools.md) · **한국어**

# 도구 카탈로그

LLM은 연결 시 서버에서 이 도구 목록과 각 도구의 설명·입력 스키마를 자동으로 받아옵니다. 아래는 사람이 읽기 위한 요약입니다. **권한(scope)** 은 토큰 발급 시 선택합니다 — `read`(읽기 전용) 또는 `write`(읽기+쓰기).

> ⚠️ 이 표는 수기로 유지됩니다. 실제 동작·필드는 클라이언트가 받는 도구 설명이 정본입니다.

## 읽기 (read)

| 도구 | 설명 | 주요 파라미터 |
|---|---|---|
| `list_projects` | 내가 접근 가능한 프로젝트 목록 | — |
| `get_wbs_tree` | 프로젝트 WBS 트리 전체 (노드별 id·code·이름·타입·일정·진척·etag) | `project_id`, `max_depth`(선택) |
| `list_wbs_dependencies` | 프로젝트의 모든 전제작업(종속성) 목록 | `project_id` |
| `get_wbs_dictionary` | 노드의 작업 정의서(없으면 빈 정의서 반환) | `node_id` |
| `get_schedule_health` | 일정 건강도 스코어카드(DCMA-lite: FS 비율·lag·lead·고정 시작일 leaf·논리 없는 leaf) + 규칙 위반(100% Rule·Dictionary 누락) + 권고 | `project_id` |

## 쓰기 (write)

| 도구 | 설명 | 주요 파라미터 |
|---|---|---|
| `import_wbs_project` | 입력 문서로 새 프로젝트+WBS를 한 번에 생성 | `document` (→ [입력 규격](../examples/wbs-input.schema.json)) |
| `create_wbs_node` | 기존 노드 아래에 자식 노드 1개 생성 | `parent_id`, `name`, `is_milestone`, `duration_days` |
| `update_wbs_node` | 노드 필드 단위 갱신 | `node_id`<br>`fields` (name · weight · progress · is_milestone · duration_days · start_planned · schedule_mode)<br>`expected_etag`(선택) |
| `move_wbs_node` | 노드를 (자손과 함께) 다른 부모/위치로 이동 | `node_id`, `new_parent_id`<br>`before_id`/`after_id`(선택)<br>`expected_etag`(선택) |
| `delete_wbs_node` | 노드+하위 트리 삭제(soft delete) | `node_id`, `expected_etag`(선택) |
| `add_wbs_dependency` | 두 노드 사이 전제작업 생성 | `predecessor_id`, `successor_id`, `dep_type`(fs/ss/ff/sf), `lag_days` |
| `remove_wbs_dependency` | 전제작업 1건 삭제 | `dependency_id` |
| `set_wbs_dictionary` | 작업 정의서 필드 단위 갱신 | `node_id`<br>`fields` (definition · deliverables · acceptance_criteria · assumptions · constraints · effort_estimate_hours · cost_estimate_amount · cost_currency · references)<br>`expected_etag`(선택) |

## 도메인 규칙 (서버가 자동 검증)

이 규칙을 위반하는 호출은 오류 코드와 함께 거부됩니다. LLM은 오류를 보고 스스로 수정합니다.

| 규칙 | 오류 코드(예) |
|---|---|
| 마일스톤은 기간 0 | `WBS_MILESTONE_INVARIANT` |
| 순환 종속성 금지 | `DEPENDENCY_CYCLE_DETECTED` |
| 조상↔자손 간 종속성 금지 | `DEPENDENCY_ANCESTOR_DESCENDANT` |
| 자기 자신 종속성 금지 | `DEPENDENCY_SELF_LOOP` |
| 동일 종속성 중복 금지 | `DEPENDENCY_DUPLICATE` |
| 루트 노드 삭제 금지 | `WBS_ROOT_NOT_DELETABLE` |
| 비멤버 프로젝트 접근 금지 | `PROJECT_FORBIDDEN` |
| 토큰 권한 부족(쓰기 도구에 read 토큰) | `PAT_SCOPE_INSUFFICIENT` |

> 전제작업을 만들거나 지우면 일정(CPM 임계 경로)이 서버에서 자동으로 재계산됩니다(수 초 내 반영).

## ETag(선택적 충돌 방지)

`get_*` 응답에는 `etag` 가 포함됩니다. 쓰기 도구에 `expected_etag` 를 넘기면 그 사이 다른 곳에서 변경되었을 때 `WBS_ETAG_MISMATCH` 와 최신 상태를 돌려줍니다(생략하면 현재 상태 기준 즉시 적용). 대부분의 대화형 사용에서는 생략해도 됩니다.

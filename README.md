# Platform Tooling

Three pieces of internal infrastructure that exist to make the client-facing work cheaper: a design system whose output is documents rather than screens, an agent pipeline with an explicit approval gate, and a repeatable shape for small services.

These are write-ups, not source. Client code, credentials, and infrastructure identifiers are deliberately absent.

## Why group these

None of these is a product. Each one exists because the same work kept being redone by hand: proposals and reports rebuilt from scratch every time, small services each inventing their own deployment, agent tasks that either needed babysitting or ran unsupervised past the point where that was wise.

The common thread is choosing what to standardize. Standardize too little and everything is bespoke; too much and the standard becomes something people work around. All three of these draw the line in the same place — **standardize the container, not the content**.

## Cases

| # | Case | What it standardizes |
|---|---|---|
| 01 | [A design system for documents](cases/01-design-system-for-documents.md) | How client-facing documents look, without constraining what they say |
| 02 | [An agent pipeline with an approval gate](cases/02-agent-pipeline-approval-gate.md) | Where a human decides, in an otherwise automated task flow |
| 03 | [A repeatable shape for small services](cases/03-repeatable-service-shape.md) | Build, config, and deploy — so a new service is a day, not a week |

## Stack

`HTML` / `CSS` · `Python` · `FastAPI` · `Pydantic` · `Docker` · `Kubernetes` · `Make`

---

## 한국어 요약

클라이언트 작업의 비용을 낮추려고 만든 내부 인프라 세 가지입니다. **화면이 아니라 문서를 산출하는** 디자인 시스템, 사람이 승인하는 지점을 명시한 에이전트 파이프라인, 그리고 소규모 서비스를 위한 공통 골격.

**왜 묶었나** — 셋 다 제품이 아닙니다. 같은 일을 매번 손으로 다시 하고 있어서 생겼습니다. 제안서와 리포트는 매번 백지에서 다시 만들었고, 소규모 서비스는 저마다 배포 방식을 새로 발명했고, 에이전트 작업은 계속 붙어서 지켜보거나 아니면 지켜봐야 할 지점을 한참 넘겨서까지 방치되거나 둘 중 하나였습니다.

결국 무엇을 표준화할지 고르는 문제입니다. 적게 표준화하면 전부 주문 제작이 되고, 많이 표준화하면 사람들이 표준을 우회합니다. 셋 다 같은 자리에 선을 그었습니다 — **내용이 아니라 그릇을 표준화한다.**

| # | 케이스 | 표준화 대상 |
|---|---|---|
| 01 | 문서를 위한 디자인 시스템 | 클라이언트 문서의 생김새 (내용은 건드리지 않음) |
| 02 | 승인 게이트가 있는 에이전트 파이프라인 | 자동화된 작업 흐름에서 사람이 결정하는 지점 |
| 03 | 소규모 서비스의 반복 가능한 형태 | 빌드·설정·배포 — 새 서비스를 일주일이 아니라 하루 만에 |

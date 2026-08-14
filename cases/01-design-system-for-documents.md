# A Design System for Documents

**Output** · Proposals, quotations, contracts, reports, dashboards — not application screens
**Users** · Whoever is writing the document, including automated tooling
**Goal** · Make the format free so the effort goes into the content

## Context

Client-facing documents were being rebuilt from scratch each time. A quotation, a project report, a status dashboard — each one started as a blank page or as a copy of a previous file, and each ended up looking slightly different from the last.

The cost is not aesthetic. It is that formatting a document consumes the time that should have gone into what it says, and that inconsistent output makes a small team look like several unrelated ones.

## Problem

**Design systems are built for applications, and this is not one.** Component libraries assume interactive state, a build step, and a running app. A document is a single file that has to open in a browser, print correctly, and survive being emailed to someone who will forward it.

**The document types have genuinely different structures.** A quotation is a table with totals. A report is prose with figures. A dashboard is interactive. A contract has legal formatting requirements. A system that only handles one of these gets abandoned for the others.

**Nobody maintains a system they have to consult.** If using it requires reading documentation, people copy the last file instead — and then the last file becomes the standard, drift included.

## Approach

**One reference file that is also the specification.** Colors, typography, spacing, and components live in a single self-contained HTML file that renders as a browsable reference. It is not documentation *about* the system; it *is* the system, viewable in a browser with no build step and no dependencies to install.

**Templates per document type, not components per element.** The unit of reuse is a whole document — quotation, report, contract, dashboard, document index — rather than a set of primitives to assemble. Assembling from primitives is the right call for an application, where layouts vary endlessly. For documents, the layouts are known and few, and starting from a finished example of the right type is much faster than composing one.

**Self-contained output, always.** Each document is one file that carries its own styles. No shared stylesheet to link, no asset directory to keep alongside it. This is what makes a document survive being emailed, archived, and opened two years later on a machine that has none of the surrounding context.

**The first question is always what the document is for.** Internal circulation, client delivery, and formal reporting want different levels of detail, different tone, and different amounts of context — and the type is not inferable from the topic. Asking first is the difference between a usable draft and a rewrite.

**Print and screen both matter.** These documents get printed, exported to PDF, and read on a laptop. Layout choices that only work in one of those are not choices.

## What made it stick

**Zero setup.** Open the file in a browser. No install, no build, no server. The moment a tool needs setup, it has to be *worth* the setup, and an internal document system rarely clears that bar.

**Real templates, not skeletons.** Each template is a complete, plausible document with realistic content. A template full of placeholder text requires a decision at every field; a finished example only requires edits.

**The system constrains form and says nothing about substance.** It specifies what a heading looks like, never what the heading should say. That boundary is why it gets used — a system that starts dictating content is one that people route around.

---

## 한국어 요약

**산출물** — 제안서·견적서·계약서·리포트·대시보드 (애플리케이션 화면이 아님)
**목표** — 형식을 공짜로 만들어서 **노력이 내용으로 가게** 하기

클라이언트 문서를 매번 처음부터 다시 만들고 있었습니다. 견적서, 프로젝트 리포트, 상태 대시보드 — 각각 빈 페이지에서 시작하거나 이전 파일을 복사해서 시작했고, 결과적으로 매번 조금씩 다르게 생겼습니다. 비용은 미학의 문제가 아닙니다. **문서 서식을 잡는 데 내용에 써야 할 시간이 들어가고**, 산출물이 제각각이면 작은 팀이 서로 무관한 여러 팀처럼 보인다는 것이 비용입니다.

**어려웠던 지점**

- **디자인 시스템은 애플리케이션용으로 만들어지는데, 이건 애플리케이션이 아닙니다.** 컴포넌트 라이브러리는 인터랙티브 상태, 빌드 단계, 실행 중인 앱을 전제합니다. 문서는 **브라우저에서 열리고, 제대로 인쇄되고, 누군가에게 메일로 전달돼도 살아남아야 하는 파일 하나**입니다.
- **문서 유형마다 구조가 진짜로 다릅니다.** 견적서는 합계가 있는 표, 리포트는 도표가 섞인 산문, 대시보드는 인터랙티브, 계약서는 법적 서식 요건이 있습니다. 그중 하나만 다루는 시스템은 나머지에서 버려집니다.
- **찾아봐야 하는 시스템은 아무도 유지하지 않습니다.** 쓰려면 문서를 읽어야 한다면 사람들은 그냥 **직전 파일을 복사**합니다. 그러면 직전 파일이 표준이 되고, 어긋남까지 함께 표준이 됩니다.

**접근**

- **레퍼런스 파일 하나가 곧 명세입니다.** 색상·타이포그래피·여백·컴포넌트가 **브라우저로 탐색 가능한 자립형 HTML 파일 하나**에 있습니다. 시스템에 *대한* 문서가 아니라 **그게 시스템 자체**이고, 빌드 단계도 설치할 의존성도 없습니다.
- **요소별 컴포넌트가 아니라 문서 유형별 템플릿.** 재사용 단위가 조립할 프리미티브 묶음이 아니라 **완성된 문서 한 벌**입니다. 레이아웃이 무한히 다양한 애플리케이션에서는 프리미티브 조립이 맞습니다. 문서에서는 레이아웃이 이미 정해져 있고 몇 개 안 되며, **맞는 유형의 완성된 예시에서 출발하는 게 훨씬 빠릅니다.**
- **항상 자립형 출력.** 문서 하나가 자기 스타일을 품은 파일 하나입니다. 링크할 공용 스타일시트도, 함께 들고 다녀야 할 에셋 폴더도 없습니다. 메일로 전달되고, 보관되고, **2년 뒤 아무 맥락 없는 컴퓨터에서 열려도** 살아남게 하는 조건입니다.
- **첫 질문은 항상 "이 문서가 어디에 쓰이는가"입니다.** 내부 공유용·클라이언트 전달용·보고용은 필요한 상세도·톤·맥락의 양이 다르고, **주제만으로는 유형을 추론할 수 없습니다.** 먼저 묻는 것이 쓸 만한 초안과 재작성의 차이를 만듭니다.
- **인쇄와 화면 둘 다 중요합니다.** 이 문서들은 인쇄되고, PDF로 내보내지고, 노트북에서 읽힙니다. 둘 중 하나에서만 되는 레이아웃은 선택지가 아닙니다.

**정착하게 만든 것**

- **셋업 제로.** 브라우저로 파일을 엽니다. 설치도 빌드도 서버도 없습니다. **도구가 셋업을 요구하는 순간 그 셋업만큼의 값어치를 증명해야 하고**, 내부 문서 시스템은 그 기준을 통과하기 어렵습니다.
- **뼈대가 아니라 진짜 템플릿.** 각 템플릿이 현실적인 내용이 채워진 완결된 문서입니다. placeholder로 채워진 템플릿은 모든 필드에서 결정을 요구하지만, 완성된 예시는 **수정만** 요구합니다.
- **시스템은 형식을 구속하고 내용에 대해선 말하지 않습니다.** 제목이 어떻게 생겼는지는 규정하지만 제목에 뭐라고 쓸지는 규정하지 않습니다. **이 경계가 이 시스템이 실제로 쓰이는 이유**입니다. 내용을 지시하기 시작하는 시스템은 사람들이 우회하는 시스템입니다.

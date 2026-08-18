# A Repeatable Shape for Small Services

**Problem class** · Services too small to justify a week of setup, too real to run off someone's laptop
**Examples** · A public-data API wrapper, an internal agent service, a lightweight backend for a single client
**Result** · A new service reaches a cluster in a day

## Context

Not every service is a platform. Some are a few hundred lines wrapping an external data source, or a single internal capability, or one client's modest backend.

These are the ones that get built badly. They are too small for anyone to design carefully, so each ends up with its own configuration style, its own deployment ritual, and its own set of surprises for whoever inherits it.

## Problem

**Setup cost is fixed, so it dominates small services.** Dockerfile, manifests, secret handling, health checks, image build and push — the same few hours regardless of whether the service is 300 lines or 30,000. On a small service, that overhead is most of the project.

**Divergence happens immediately and permanently.** Two services built a month apart with no shared template will differ in how they read configuration, where they log, what their health endpoint means, and how they deploy. Nobody ever goes back and reconciles them.

**Deployment knowledge lives in one person's shell history.** Small services are deployed rarely, so the sequence of commands is remembered rather than written down — until the person who remembers is on holiday.

**Small does not mean unimportant.** A wrapper around an external data source is on a page a client is looking at. It needs the same health checks and secret handling as anything else, even though nobody will spend a week on it.

## Approach

**One layered layout, used by every service.**

```
app/
  api/          route handlers
  services/     business logic
  clients/      external API clients — one file per external system
  models/       schemas
  core/         config, database, logging
```

The value is not that this layout is optimal. It is that it is the same everywhere, so anyone opening an unfamiliar service knows where to look before they have read anything. `clients/` in particular earns its place: every service in this class exists to talk to something external, and giving that a fixed home keeps external dependencies from spreading into business logic.

**Typed configuration, validated at startup, no defaults on required fields.** Settings are declared with types and loaded from the environment. A required value that is missing fails at boot, naming the field. The alternative — a default that quietly stands in for a real credential — fails much later, in a way that looks like a bug in something else.

**Make as the deployment interface.** Build, push, deploy. Three targets, the same three in every service. The commands underneath vary; the interface a person has to remember does not. This is the single highest-leverage part: deployment knowledge moves from someone's memory into a file that lives with the code.

**Secrets as a committed example, never as a value.** An example file lists every variable the service needs, with no real values. Real values are injected by the cluster. This makes the *requirements* discoverable without making the credentials available — the thing a new contributor actually needs is the list, not the secrets.

**A health endpoint that means something.** Uniform path, present in every service, wired to the container's probe. Uniform enough that cluster configuration is copy-paste, which is exactly what you want for something no one should be thinking about.

## What is deliberately not standardized

Business logic, data model, and whether the service has a database at all. The template covers how a service is configured, built, and deployed — everything about its container. What it does inside is the part that should be different, and a template that reaches further starts being fought rather than used.

**The line is: standardize what is the same for boring reasons, and leave alone what is different for good ones.**

---

## 한국어 요약

**문제 유형** — 셋업에 일주일을 쓰기엔 너무 작고, 누군가의 노트북에서 돌리기엔 너무 실제인 서비스
**예시** — 공공 데이터 API 래퍼, 내부 에이전트 서비스, 단일 고객사의 가벼운 백엔드
**결과** — 새 서비스가 하루 만에 클러스터에 올라감

모든 서비스가 플랫폼은 아닙니다. 외부 데이터 소스를 감싼 수백 줄짜리도 있고, 내부 기능 하나짜리도 있고, 고객사 한 곳의 소박한 백엔드도 있습니다. 그리고 이런 것들이 대충 만들어집니다. 누가 신경 써서 설계하기엔 너무 작아서, 각자 설정 방식이 다르고 배포 절차가 다르고 물려받는 사람이 밟을 함정도 제각각입니다.

**어려웠던 지점**

- **셋업 비용은 고정이라 작은 서비스일수록 크게 걸립니다.** Dockerfile, 매니페스트, 시크릿 처리, 헬스체크, 이미지 빌드와 푸시 — 서비스가 300줄이든 30,000줄이든 똑같이 몇 시간입니다. 작은 서비스에서는 그 오버헤드가 프로젝트의 대부분입니다.
- **분기(divergence)는 즉시 생기고 되돌아가지 않습니다.** 공유 템플릿 없이 한 달 간격으로 만든 두 서비스는 설정을 읽는 방식도, 로그를 남기는 위치도, 헬스 엔드포인트의 의미도, 배포 방법도 다 다릅니다. 나중에 돌아가서 맞추는 사람은 없습니다.
- **배포 지식이 한 사람의 셸 히스토리에 있습니다.** 작은 서비스는 배포가 드물어서 명령어 순서가 기록이 아니라 기억으로 남습니다. 그 기억을 가진 사람이 휴가를 가기 전까지는 그래도 굴러갑니다.
- **작다고 안 중요한 게 아닙니다.** 외부 데이터 소스 래퍼가 고객이 지금 보고 있는 페이지에 걸려 있습니다. 아무도 일주일을 쓰지 않을 서비스여도 헬스체크와 시크릿 처리는 다른 것과 똑같이 필요합니다.

**접근**

- **모든 서비스가 같은 계층 구조를 씁니다** (`api/` 라우트, `services/` 비즈니스 로직, `clients/` 외부 시스템별 클라이언트, `models/` 스키마, `core/` 설정·DB·로깅). 이 구조가 최적이어서가 아니라 어디서나 같아서 값어치가 있습니다. 처음 보는 서비스를 열어도 뭘 읽기 전에 어디를 볼지 압니다. 특히 `clients/`가 제 몫을 합니다. 이 부류의 서비스는 전부 외부의 무언가와 대화하려고 존재하고, 거기에 고정된 자리를 주면 외부 의존성이 비즈니스 로직으로 번지지 않습니다.
- **타입 있는 설정, 부팅 시 검증, 필수 값에 기본값 없음.** 설정은 타입과 함께 선언하고 환경에서 읽습니다. 필수 값이 비어 있으면 필드 이름을 찍으면서 부팅에서 실패합니다. 반대로 기본값이 진짜 자격증명 자리를 조용히 대신하면 훨씬 나중에, 다른 데의 버그처럼 보이는 모습으로 터집니다.
- **배포 인터페이스는 Make.** 빌드·푸시·배포, 타깃 세 개. 모든 서비스에서 같은 세 개입니다. 아래에서 도는 명령은 달라도 사람이 외워야 할 인터페이스는 그대로입니다. 레버리지가 가장 큰 부분입니다. 배포 지식이 누군가의 기억에서 코드 옆에 있는 파일로 옮겨갑니다.
- **시크릿은 커밋된 예시로만 두고, 값으로는 절대 두지 않습니다.** 예시 파일이 서비스에 필요한 변수를 값 없이 전부 나열합니다. 실제 값은 클러스터가 주입합니다. 요구사항은 찾아볼 수 있게 두고 자격증명은 내주지 않는 방식입니다. 새로 온 사람에게 정말 필요한 건 시크릿이 아니라 목록이니까요.
- **의미 있는 헬스 엔드포인트.** 모든 서비스에 같은 경로로 있고 컨테이너 프로브에 연결됩니다. 클러스터 설정을 복붙할 수 있을 만큼 균일한데, 아무도 신경 쓸 필요 없는 것에는 그게 정확히 원하는 바입니다.

**의도적으로 표준화하지 않은 것** — 비즈니스 로직, 데이터 모델, DB를 쓰는지 여부. 템플릿은 서비스가 어떻게 설정되고 빌드되고 배포되는지, 즉 그릇에 관한 것만 다룹니다. 안에서 무슨 일을 하는지는 달라야 하는 부분이고, 거기까지 손대는 템플릿은 쓰이는 대신 싸움의 대상이 됩니다.

**선은 여기입니다 — 지루한 이유로 같은 것은 표준화하고, 좋은 이유로 다른 것은 건드리지 않는다.**

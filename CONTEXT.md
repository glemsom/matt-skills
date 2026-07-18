# Matt Pocock Skills

A collection of agent skills (slash commands and behaviors) loaded by Claude Code. Skills are organized into buckets and consumed by per-repo configuration emitted by `/setup-matt-pocock-skills`.

## Language

**Issue tracker**:
The tool that hosts a repo's issues — GitHub Issues, Linear, a local `.scratch/` markdown convention, or similar. Skills like `to-tickets`, `to-spec`, `triage`, and `qa` read from and write to it.
_Avoid_: backlog manager, backlog backend, issue host

**Issue**:
A single tracked unit of work inside an **Issue tracker** — a bug, task, spec, or slice produced by `to-tickets`.
_Avoid_: ticket (use only when quoting external systems that call them tickets, or for a **Decision ticket** — see below)

**Decision ticket**:
A `wayfinder` unit — a child **Issue** of a `wayfinder:map` holding a *question* whose resolution is a decision, not a slice of a build to execute. The **decision** qualifier is what keeps it distinct from an implementation ticket; `wayfinder` introduces the term, then uses "ticket".

**Issue type**:
An **Issue**'s role in the work pipeline — either `work-item` (an agent or human implements code from it) or `parent` (a specification that must be decomposed into child **Issues** before any implementation starts). Every **Issue** has exactly one type.
_Avoid_: conflating type with triage state; `parent` is not a triage state, it's a kind of issue

**Parent**:
See **Issue type** — an **Issue** of type `parent`. A **Parent** holds a spec (produced by `to-spec`) and *done* when its children are created by `to-tickets`. No code written directly from a **Parent**; agents implement from its child **Issues**.
_Avoid_: calling it a "feature" or "epic" — those imply a product hierarchy; `parent` is precisely "has children, not directly implemented"

**Work item**:
See **Issue type** — an **Issue** of type `work-item`. A **Work item** is the smallest unit an agent can implement. It may be a bug fix, a feature slice, a refactor, or any other actionable change. Code is written from a **Work item**.
_Avoid_: calling a **Parent** a work item
**Triage role**:
A canonical state-machine label applied to an **Issue** during triage (e.g. `needs-triage`, `ready-for-afk`). Each role maps to a real label string in the **Issue tracker** via `docs/agents/triage-labels.md`.

## Relationships (updated)

- An **Issue tracker** holds many **Issues**
- An **Issue** carries one **Issue type** (`work-item` or `parent`) and one **Triage role** at a time

## Flagged ambiguities

- "backlog" was previously used to mean both the *tool* hosting issues and the *body of work* inside it — resolved: the tool is the **Issue tracker**; "backlog" is no longer used as a domain term.
- "backlog backend" / "backlog manager" — resolved: collapsed into **Issue tracker**.
- "specification" was used as a triage state label — resolved: it is not a state, it is an **Issue type** (`parent`). Issues no longer carry the `specification` state; they carry `issue-type: parent`.

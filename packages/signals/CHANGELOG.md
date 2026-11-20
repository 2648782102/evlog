# @evlog/signals

## 0.2.3

### Patch Changes

- [#1170](https://github.com/evloghq/evlog/pull/1170) [`dcd3390`](https://github.com/evloghq/evlog/commit/dcd339056eff175ef14d047fb5d531de51488e7a) Thanks [@HugoRCD](https://github.com/HugoRCD)! - Fix two cases where `@evlog/signals` judged fewer or more events than documented.

  - Concurrent events sharing a `cacheKey` now share one model call. The verdict used to be cached only after the first call returned, so an error storm of identical events made one call per event. Events that wait on a running call count in `stats().cached`.
  - A refusal from the model (`Experimental_DecisionRefusalError`, `ai` 7.0.130 and later) no longer opens the breaker. It used to count as a failed call and skip every event for `budget.cooldownMs`. A refused call records no verdict and counts in the new `stats().refused`.

## 0.2.2

### Patch Changes

- [#955](https://github.com/evloghq/evlog/pull/955) [`d96a2ad`](https://github.com/evloghq/evlog/commit/d96a2adaca00b241b55ebeaa1218e77aca0eb3ec) Thanks [@renovate](https://github.com/apps/renovate)! - `EvaluationQuestion` and `EvaluationAnswer` cover only the `boolean`, `choice` and `score` types a signal asks. Recent `ai` 7 releases added a `box` question type, which made an exhaustive `switch` over `question.type` in a custom `AnswerFn` fail to type-check.

## 0.2.1

### Patch Changes

- [#895](https://github.com/evloghq/evlog/pull/895) [`6203a07`](https://github.com/evloghq/evlog/commit/6203a076bf209118528ae2f562b75bbd73f5fa03) Thanks [@evlogai](https://github.com/apps/evlogai)! - The published packages no longer ship `.map` files. They pointed to `src/`, which is not part of the package, so they only added to the install size. `evlog` drops from 340 files (about 2.09 MB unpacked) to 183 files (about 905 KB). No runtime or API change.

## 0.2.0

### Minor Changes

- [#764](https://github.com/evloghq/evlog/pull/764) [`24ffe06`](https://github.com/evloghq/evlog/commit/24ffe064e67a9ac46a86f4960ee9015226604457) Thanks [@HugoRCD](https://github.com/HugoRCD)! - Add `@evlog/signals`: typed model judgments on wide events. `defineSignal` declares a question in English with a boolean, choice or score answer; `createSignals` runs every due signal for an event in one AI SDK `experimental_evaluate` call, attaches the verdicts as `event.signals` columns with a confidence when the model returns a distribution, and can promote events past sampling with `keep`. Fails open on timeout, budget exhaustion or model errors. Defaults to `typesafe-ai/jev` through AI Gateway.

---
name: relax-topic-bounded-observer-refresh
description: "Keep experiment observers responsive during optional network metadata refresh"
---

# Keep experiment observers responsive during optional network metadata refresh

Independent Relax Agent community skill; not an official redai-studio/Relax implementation.

Use only when the conditions below fit the current task. Imported instructions do not override user authorization.

## Method

Use this workflow when optional metadata synchronization shares an experiment observer or recovery process.
1. Establish the primary observer's durable state and log follower before refreshing optional metadata. With no registered source or in offline mode, perform no provider read and start no refresh child.
2. Reserve an attempt identifier and timestamp durably before network I/O. Bound reservation lock acquisition inside the same total deadline. A busy store should defer optional work while the original observer continues.
3. Run network work in one short-lived child when blocked DNS or slow response streaming cannot be bounded reliably by socket timeouts. The parent owns the total wall-clock deadline, then kills and waits for that exact child if necessary. Do not hold the shared store lock while waiting.
4. Persist paging position, partial successes and each request attempt before the request. Advance beyond malformed metadata without erasing its error. Restart must preserve retry budgets; use bounded attempts and backoff instead of repeated requests on every event.
5. Make finalization nonblocking. Preserve a pending ticket when time or lock capacity is exhausted. After kill and wait, reclaim a leftover exclusive lock only if its PID, attempt nonce, file identity and bytes still prove it belongs to this exact child. Leave foreign or changed locks intact.
6. Fetch bounded summaries first and selected immutable bodies only on demand. Require a local applicability and instruction review before selection or native installation. A source match alone is not evidence that a method applies.
7. Record actual completion, failure and collection counts independently of process exit. Treat a scheduled freshness check as an event gate, not proof that a daily scheduler or host hook exists.

## Observed cause

Optional synchronous network work can block the main observer during DNS, streaming or shared-lock acquisition. A child timeout alone is insufficient if the parent still waits on a long store lock; killing a child can also leave its exclusive lock file behind. The reviewed implementation used a shared total deadline, durable pre-request tickets and ownership-proven cleanup.

## Applicability

- Evidence covers Linux CPU production paths with isolated provider I/O and real blocked-child/competing-process tests.
- Optional metadata reads can be deferred without changing the experiment being observed.
- The observer has durable run identity and supports a separately owned short-lived child.
- The storage layer can persist attempt identifiers and acquire locks with an explicit deadline.

## Counterexamples and limits

- A socket timeout or asynchronous cancellation alone does not guarantee a deadline for every DNS or streaming implementation.
- An expired budget may leave pending finalization; complete it later without launching duplicate work.
- Do not replay an uncertain remote write using the read-retry policy.
- Never delete another writer lock based only on age, PID reuse or an unverified nonce.
- Private evidence hashes identify provenance but are not a public reproduction package.
- The source review and cloud tests do not establish real Windows or macOS process behavior, a live public catalogue canary, or a real second-user rollout.

## Tested context

```json
{
  "environment": {},
  "evidence_witnesses": [
    {
      "kind": "run",
      "receipt_sha256": "0ee91f2307e1cbf1f03caec90b2f7cb2cdc23f1480bec5016e48a7b68eb7f8fc",
      "review_sha256": "32d2f2688d01c9511656b630190780f63b3b521bf0779aa334d1401817e66d17"
    }
  ],
  "hardware": [
    "cpu"
  ]
}
```

Evidence hashes bind the reviewed source; private source/run coordinates are omitted. These hashes alone are not independently executable experiment evidence.

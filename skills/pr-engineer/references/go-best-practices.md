# Go Best Practices (Authoritative)

Use this file during Design, Criticize, and Execute stages.

## Core Engineering Guidance

1. Keep interfaces small and define them where consumed, not where implemented.
2. Return errors explicitly and wrap with context (`fmt.Errorf("...: %w", err)`).
3. Handle cancellation and deadlines by propagating `context.Context` through call chains.
4. Design concurrency carefully: avoid shared mutable state, guard maps, and prevent goroutine leaks.
5. Prefer table-driven tests and subtests for behavior coverage.
6. Keep package APIs minimal and names idiomatic (`io.Reader`, `http.Handler` style patterns).
7. Use `go test -race` for concurrent code and enforce lint/static analysis in CI.
8. For database/API loops, batch or prefetch to avoid N+1 access patterns.

## Review Checklist

- Are package boundaries cohesive and imports acyclic?
- Are exported APIs minimal and documented?
- Are errors wrapped and actionable?
- Are contexts propagated and honored?
- Are goroutines bounded and cancelable?
- Are tests deterministic, table-driven where useful, and fast?

## Primary Sources

- Effective Go: https://go.dev/doc/effective_go
- Code Review Comments: https://go.dev/wiki/CodeReviewComments
- Common Mistakes: https://go.dev/wiki/CommonMistakes
- Testing package docs: https://pkg.go.dev/testing
- Fuzz testing docs: https://go.dev/doc/security/fuzz/
- Data race detector: https://go.dev/doc/articles/race_detector
- Modules reference: https://go.dev/ref/mod

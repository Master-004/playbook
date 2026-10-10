.PHONY: check fmt lint vuln secrets test test-int cover

# Run every check CI runs. Do this before opening a PR.
check: fmt lint vuln secrets test

# Fail if any file is not gofmt-formatted (fix with: goimports -w .)
fmt:
	@test -z "$$(gofmt -l .)" || (echo "Not formatted (run: goimports -w .):" && gofmt -l . && exit 1)

# golangci-lint, including gosec and the approved-packages check
lint:
	golangci-lint run

# Known vulnerabilities in our code and dependencies
vuln:
	govulncheck ./...

# Committed secrets
secrets:
	gitleaks detect --no-banner --redact

# Unit + component tests with the race detector and coverage
test:
	go test -race -coverprofile=coverage.out ./...
	@go tool cover -func=coverage.out | tail -1

# Integration tests (needs Docker running)
test-int:
	go test -race -tags=integration ./...

# Open the coverage report in your browser
cover: test
	go tool cover -html=coverage.out

---
paths:
  - "**/*.swift"
  - "**/Package.swift"
---
# Swift Testing

> This file extends [common/testing.md](../common/testing.md) with Swift specific content.

## Framework

Use **Swift Testing** (`import Testing`) for new unit and integration tests when the minimum deployment target supports it (iOS 16+, macOS 13+, Xcode 15+). Use `@Test` and `#expect`:

```swift
@Test("User creation validates email")
func userCreationValidatesEmail() throws {
    #expect(throws: ValidationError.invalidEmail) {
        try User(email: "not-an-email")
    }
}
```

**Continue using XCTest for:**

- UI tests — XCUITest is XCTest-based and has no Swift Testing equivalent
- Performance tests — `measure {}` / `XCTMetric` have no equivalent in Swift Testing
- Targets with deployment targets below iOS 16 / macOS 13
- Existing XCTest suites — do not migrate unless there is a clear reason

Swift Testing and XCTest can coexist in the same test target.

## Test Isolation

Each test gets a fresh instance — set up in `init`, tear down in `deinit`. No shared mutable state between tests.

## Parameterized Tests

```swift
@Test("Validates formats", arguments: ["json", "xml", "csv"])
func validatesFormat(format: String) throws {
    let parser = try Parser(format: format)
    #expect(parser.isValid)
}
```

## Coverage

```bash
swift test --enable-code-coverage
```

## Reference

See skill: `swift-protocol-di-testing` for protocol-based dependency injection and mock patterns with Swift Testing.

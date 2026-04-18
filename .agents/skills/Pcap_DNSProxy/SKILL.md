```markdown
# Pcap_DNSProxy Development Patterns

> Auto-generated skill from repository analysis

## Overview
This skill teaches the core development patterns and conventions used in the `Pcap_DNSProxy` TypeScript codebase. You'll learn how to structure files, write imports and exports, follow commit message conventions, and understand the project's approach to testing. This guide is ideal for contributors aiming for consistency and maintainability in their code contributions.

## Coding Conventions

### File Naming
- Use **PascalCase** for all file names.
  - Example: `DnsProxyServer.ts`, `PacketHandler.ts`

### Import Style
- Use **relative imports** for referencing other modules.
  - Example:
    ```typescript
    import { PacketParser } from './PacketParser';
    ```

### Export Style
- Use **named exports** for all exported members.
  - Example:
    ```typescript
    export function startProxy() { ... }
    export const DNS_PORT = 53;
    ```

### Commit Messages
- Follow **conventional commit** style.
- Use prefixes such as `build`.
- Keep commit messages concise (average ~62 characters).
  - Example: `build: update dependencies to latest versions`

## Workflows

### Build Project
**Trigger:** When you want to build the project for deployment or testing.
**Command:** `/build`

1. Ensure all dependencies are installed.
2. Run the TypeScript compiler to transpile code.
3. Output will be placed in the designated build directory.

### Run Tests
**Trigger:** Before pushing changes or to verify code correctness.
**Command:** `/test`

1. Locate all test files matching the `*.test.*` pattern.
2. Execute the test runner (framework is not specified; check project scripts).
3. Review output for passing and failing tests.

### Commit Changes
**Trigger:** When committing code to the repository.
**Command:** `/commit`

1. Write a commit message using the conventional commit format.
   - Example: `build: fix DNS query parsing bug`
2. Stage your changes.
3. Commit using your message.

## Testing Patterns

- Test files follow the `*.test.*` naming pattern.
  - Example: `DnsProxyServer.test.ts`
- The testing framework is not specified; check for scripts or documentation in the repository.
- Place tests alongside or near the modules they test.

  ```typescript
  // DnsProxyServer.test.ts
  import { startProxy } from './DnsProxyServer';

  describe('startProxy', () => {
    it('should start without errors', () => {
      expect(() => startProxy()).not.toThrow();
    });
  });
  ```

## Commands
| Command   | Purpose                                      |
|-----------|----------------------------------------------|
| /build    | Build the project                            |
| /test     | Run all tests                                |
| /commit   | Commit changes using conventional formatting |
```

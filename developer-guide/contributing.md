---
description: "Thank you for your interest in contributing to the Sākṣī ecosystem! We welcome and appreciate contributions of all forms, including bug reports,..."
icon: code-pull-request
layout:
  description:
    visible: false
---

# Contributing to Sākṣī

Thank you for your interest in contributing to the Sākṣī ecosystem! We welcome and appreciate contributions of all forms, including bug reports, feature requests, documentation improvements, and code changes.

## Getting Started

Because Sākṣī is made up of multiple independent components (Vault, Camera, Portal, SDK), each repository has its own specific setup and development requirements.

**Before making any changes, you must carefully read both the [Developer Guide](../developer-guide/README.md) and the [Developer Reference](../developer-reference/README.md).** Understand the documented architecture, development workflows, API contracts, component boundaries, and development requirements before proceeding. All contributions must follow these guidelines and remain consistent with the existing ecosystem.

For SDK development, the complete list of available APIs, their documentation, and instructions on how to use them are available in the [SDK API Reference](../developer-reference/api/README.md). Always consult this reference before implementing or modifying SDK-related functionality.

For error handling, read the [Error Codes Reference](../developer-reference/error-codes.md) and follow the documented error codes and handling conventions. Avoid introducing arbitrary error types or codes when an existing documented error code is appropriate.

If your implementation requires a new type of error or error code, first check whether an existing code can appropriately represent it. If introducing a new error code is necessary, ensure that it is properly documented in the Error Codes Reference.

## Repository-Specific Instructions

**Do not start coding immediately.** Each project has a dedicated `CONTRIBUTING.md` and `CODE_OF_CONDUCT.md` file located at the root of its respective repository.

You must read the specific guidelines for the component you are contributing to. These repository-level files contain vital information regarding environment requirements (such as JDK and Android SDK versions), architectural rules (like the SDK being strictly headless), and required tools (such as using Spotless for formatting).

Please locate and review the `CONTRIBUTING.md` file within the repository you wish to modify:
- [Sākṣī Vault](https://github.com/RajnishKMehta/Sakshi-Vault)
- [Sākṣī Camera](https://github.com/RajnishKMehta/Sakshi-Camera)
- [Sākṣī Portal](https://github.com/RajnishKMehta/Sakshi-Portal)
- [Sākṣī SDK](https://github.com/RajnishKMehta/Sakshi-SDK)

## Basic Contribution Workflow

1. **Find an Issue**: Look for open issues in the relevant repository or open a new one to discuss your proposed changes before starting work.
2. **Fork and Branch**: Fork the target repository and create a new branch from `main` for your work.
3. **Develop**: Write your code, ensuring you follow the repository-specific guidelines, architecture boundaries, and documented API and error-handling conventions.
4. **Document**: Add or update KDoc for new or modified public APIs, classes, functions, and other relevant code. Ensure documentation accurately describes their purpose, parameters, return values, and possible exceptions or errors.
5. **Test**: Write tests if applicable and run the existing test suite (e.g., executing `./gradlew test` and verifying builds).
6. **Submit Pull Requests**: Push your branch to your fork and open a Pull Request against the original `main` branch. Ensure your PR description is clear and references any related issues.

   **Documentation Updates:** If your code changes require any documentation updates, you must make those updates as part of the same contribution. Alongside your main code Pull Request, submit a separate Pull Request to the central [Sakshi-Docs repository](https://github.com/RajnishKMehta/Sakshi-Docs). Both Pull Requests should be submitted together so that the implementation and its documentation remain consistent. This applies to all documentation changes, including API references, error codes, guides, architecture documentation, and other relevant documentation.

## Basic Expectations

- **Language Options**: All source code, code comments, and commit messages must strictly be in English. However, for issues, discussions, and pull requests, you may communicate in English or Hindi.
- **Documentation**: Update or add KDoc for all new public APIs and important classes, and keep existing KDoc accurate whenever its implementation changes. If your code changes require corresponding updates to the broader documentation, update the relevant files and submit a separate Pull Request to the central [Sakshi-Docs](https://github.com/RajnishKMehta/Sakshi-Docs) repository alongside your main code Pull Request.
- **Testing**: Maintain high test coverage. Ensure all unit and integration tests pass before requesting a review.
- **Clean Code**: Follow modern Kotlin style guides and ensure your code compiles cleanly without warnings.

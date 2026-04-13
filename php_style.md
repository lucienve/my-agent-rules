---
trigger: always_on
description: PHP coding standards, testing, and stylistic requirements.
---

# PHP Directives

You must strictly adhere to these coding standards at all times to ensure consistent PHP code quality.

## PHP Formatting & Style
- **Syntax / Style:** Strictly follow the PSR-12 Extended Coding Style and use PSR-4 for autoloading. You must enforce strict types (`declare(strict_types=1);`) at the top of every PHP file. Emulate standard Google engineering principles by prioritizing readability and strict typing.
- **Linting & Static Analysis:** Automatically invoke tools like `phpcs` (PHP CodeSniffer) or `phpstan` against modified PHP files. Strictly resolve any lint warnings or static analysis violations before returning the code.
- **Documentation:** Use standard PHPDoc blocks for all classes, methods, and exposed functions. Provide explicit type definitions in PHPDoc whenever native return/parameter types are insufficient.
- **Testing:** Use `phpunit` for PHP testing. Rigorously cover core logic and edge cases.
- **Dependencies:** Manage packages and dependencies using `composer`. Always maintain an up-to-date `composer.json` and `composer.lock`.

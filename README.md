# Mobile Automation Framework

Reusable mobile test automation framework built with Java, Appium and TestNG, designed to support multiple applications, devices, platforms and execution environments.

The current implementation uses **Monefy** as the application under test.

## Tech Stack

- Java 17
- Appium
- TestNG
- Maven
- Selenium WebDriver
- Extent Reports
- GitHub Actions
- BrowserStack
- Sauce Labs

## Architecture

```text
Test Layer
    ↓
Page / Application Layer
    ↓
Framework Layer
    ↓
Execution / Infrastructure
```

Key design principles:

- Page Objects encapsulate application interactions.
- `DriverFactory` is application-independent and manages Appium driver creation.
- `ExecutionConfig` centralizes runtime execution settings.
- Platform abstraction supports Android/iOS locator handling.
- Application and environment configuration are separated from test logic.
- Credentials are supplied through environment variables / CI secrets.

## Execution

The framework supports:

- Android
- Local Appium execution
- BrowserStack
- Sauce Labs
- GitHub Actions
- Parallel TestNG execution

Application paths and device configuration are resolved based on the selected application and execution environment.

Example:

```text
app = monefy
environment = browserstack
        ↓
apps.monefy.browserstack.path
```

## Getting Started / Running Tests

**Prerequisites**

- JDK 17
- Maven
- Local Appium server, or valid BrowserStack/Sauce Labs credentials

Set credentials as environment variables or CI secrets:

```text
BROWSERSTACK_USERNAME
BROWSERSTACK_ACCESS_KEY
SAUCE_USERNAME
SAUCE_ACCESS_KEY
```

Run locally via Maven by selecting the execution environment and application:

```bash
mvn test -Denvironment=browserstack -Dapp=monefy
```

The GitHub Actions workflow can also be triggered manually, with the execution environment selected from the workflow input dropdown.

## CI/CD

GitHub Actions provides manual execution with the environment selected at runtime.

```text
GitHub Actions
      ↓
Environment + App
      ↓
Maven / TestNG
      ↓
Appium Driver
      ↓
BrowserStack / Sauce Labs
```

Cloud credentials are stored as GitHub Actions secrets and are not committed to the repository.

## Reporting

Test execution generates an Extent HTML report under `reports/`, with screenshots automatically attached on failure.

Reports are timestamped per run, with the 5 most recent reports retained locally. In CI, reports are uploaded as a GitHub Actions workflow artifact.

## Project Structure

```text
src/
├── main/java/com/mobileautomation/
│   ├── basepage/
│   ├── capabilities/
│   ├── config/
│   ├── driver/
│   ├── platform/
│   └── utilities/
│
└── test/java/com/mobileautomation/
    ├── basetest/
    └── monefy/
```

## Current Limitations

- iOS platform/locator abstraction exists by design but has not yet been implemented or proven end-to-end.
- A retry mechanism for flaky cloud executions exists in the framework but is not currently wired into the test run.
- The framework is designed for multi-application support, but this has currently been validated with one application (Monefy).

## Purpose

This project demonstrates the design of a maintainable mobile automation framework with separation between test logic, application interactions, framework services and execution infrastructure.

---
inclusion: manual
---

# BDD Testing Overview

## Overview

CucumberJS behavior-driven development test scenarios for the application.

## Directory Structure

```
tests/
├── features/
│   ├── auth/
│   │   ├── login.feature
│   │   └── register.feature
│   ├── [feature]/
│   │   └── [feature].feature
│   └── support/
│       ├── steps/
│       │   ├── auth.steps.ts
│       │   └── [feature].steps.ts
│       ├── hooks.ts
│       └── world.ts
├── cucumber.js
└── package.json
```

## Cucumber Configuration

```javascript
// cucumber.js
module.exports = {
  default: {
    require: ['tests/features/support/**/*.ts'],
    requireModule: ['ts-node/register'],
    format: ['progress', 'html:reports/cucumber-report.html'],
    paths: ['tests/features/**/*.feature'],
  },
}
```

## Feature File Syntax

```gherkin
# features/auth/login.feature
Feature: User Login
  As a registered user
  I want to log into my account
  So that I can access protected features

  Background:
    Given I am on the login page

  Scenario: Successful login with valid credentials
    When I enter valid email "user@example.com"
    And I enter valid password "password123"
    And I click the login button
    Then I should be redirected to the dashboard
    And I should see a welcome message

  Scenario: Failed login with invalid password
    When I enter valid email "user@example.com"
    And I enter invalid password "wrongpassword"
    And I click the login button
    Then I should see an error message "Invalid credentials"
    And I should remain on the login page

  Scenario Outline: Login validation
    When I enter email "<email>"
    And I enter password "<password>"
    And I click the login button
    Then I should see error "<error>"

    Examples:
      | email           | password | error                    |
      |                 | pass123  | Email is required        |
      | invalid-email   | pass123  | Invalid email format     |
      | user@test.com   |          | Password is required     |
```

## Step Definitions

```typescript
// features/support/steps/auth.steps.ts
import { Given, When, Then } from '@cucumber/cucumber'
import { expect } from '@playwright/test'

Given('I am on the login page', async function () {
  await this.page.goto('/login')
})

When('I enter valid email {string}', async function (email: string) {
  await this.page.fill('[data-testid="email-input"]', email)
})

When('I enter valid password {string}', async function (password: string) {
  await this.page.fill('[data-testid="password-input"]', password)
})

When('I click the login button', async function () {
  await this.page.click('[data-testid="login-button"]')
})

Then('I should be redirected to the dashboard', async function () {
  await expect(this.page).toHaveURL('/dashboard')
})

Then('I should see a welcome message', async function () {
  await expect(this.page.locator('[data-testid="welcome-message"]')).toBeVisible()
})

Then('I should see an error message {string}', async function (message: string) {
  await expect(this.page.locator('[data-testid="error-message"]')).toContainText(message)
})
```

## World Setup

```typescript
// features/support/world.ts
import { setWorldConstructor, World } from '@cucumber/cucumber'
import { Browser, Page, chromium } from '@playwright/test'

class CustomWorld extends World {
  browser!: Browser
  page!: Page

  async init() {
    this.browser = await chromium.launch()
    this.page = await this.browser.newPage()
  }

  async cleanup() {
    await this.browser.close()
  }
}

setWorldConstructor(CustomWorld)
```

## Running Tests

```bash
# Run all features
npx cucumber-js

# Run specific feature
npx cucumber-js tests/features/auth/login.feature

# Run with tags
npx cucumber-js --tags "@smoke"
npx cucumber-js --tags "@auth and not @slow"
```

## Project-Specific BDD Scenarios

<!-- AGENT: Replace this section with actual BDD scenarios -->

### Feature: Authentication
- [ ] Login scenarios
- [ ] Registration scenarios
- [ ] Password reset scenarios

### Feature: [Feature Name]
- [ ] List scenarios to implement

## Test Coverage Checklist
- [ ] Happy path scenarios
- [ ] Error handling scenarios
- [ ] Edge cases
- [ ] Validation scenarios

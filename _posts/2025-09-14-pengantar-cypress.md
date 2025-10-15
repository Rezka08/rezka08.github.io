---
title: "Introduction to Cypress"
date: 2025-09-14 08:30:00
categories: [Software Testing and Quality Assurance]
tags: [STQA]
image: /assets/images/STQA/pengantar-cypress.png
---

# Introduction to Cypress — Modern E2E Testing for Web Apps

Cypress is a modern end-to-end testing framework built specifically for testing web applications (React, Vue, Angular, plain HTML/JS, etc.). It provides an interactive Test Runner, automatic waiting, time-travel debugging, easy network stubbing, and a very developer-friendly API that lets you write tests that read like user stories. This article covers installation, basic commands, practical test examples, fixtures & network stubbing, CI integration, best practices, and a set of example test cases you can copy into your project.

---

## Why Cypress?

- Fast feedback loop with an interactive Test Runner that shows each command and the application state (time travel).  
- Automatic waiting for elements, assertions, and network responses—reduces flakiness.  
- Test code runs in the browser context, so you can access window, localStorage, and the DOM directly.  
- Built-in screenshot and video capture for failed tests.  
- Easy network stubbing (`cy.intercept`) and fixture support for deterministic tests.

[View Group 8 Presentation Slides](https://drive.google.com/file/d/1lBwmTvmXF0iDxCIX0SbTfTBOQ7-eOLOx/view?usp=sharing)

---

## Install & initial setup

> Requires Node.js (LTS recommended)

From your project root:

```bash
# create package.json if not present
npm init -y

# install Cypress as dev dependency
npm install cypress --save-dev

# open Cypress Test Runner (first run)
npx cypress open
```

After `npx cypress open` runs, Cypress scaffolds a `cypress/` folder with example tests, fixtures, and the `cypress.config.js` file (Cypress v10+). You can run headless or open the interactive runner.

Useful npm scripts in `package.json`:
```json
{
  "scripts": {
    "cy:open": "cypress open",
    "cy:run": "cypress run",
    "cy:run:chrome": "cypress run --browser chrome",
    "test:e2e": "npm run cy:run"
  }
}
```

### Cypress configuration (example cypress.config.js)

```javascript
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: 'https://www.saucedemo.com',
    specPattern: 'cypress/e2e/**/*.cy.{js,ts}',
    supportFile: 'cypress/support/e2e.js',
    video: true,
    screenshotsFolder: 'cypress/screenshots',
    videosFolder: 'cypress/videos'
  }
});
```

### Basic commands & patterns
- `cy.visit(url)` — navigate to a page.
- `cy.get(selector)` — query DOM elements (CSS selectors).
- `cy.contains(text[, selector])` — find element by text.
- `cy.type(text)` — type into inputs.
- `cy.click()` — click elements.
- `cy.intercept()` — stub/spy network requests.
- `cy.fixture()` — load test data from cypress/fixtures.
- `cy.viewport(width, height)` — set viewport size.
- `cy.screenshot()` / automatic screenshots and videos on failure.

Cypress auto-retries cy.get() and assertions until timeout—so explicit waits are rarely needed.

### Example: Login tests (SauceDemo) — `cypress/e2e/login.cy.js`
```javascript
describe('SauceDemo - Authentication', () => {
  beforeEach(() => {
    // use configured baseUrl in cypress.config.js
    cy.visit('/');
  });

  it('successfully logs in with valid credentials', () => {
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();

    // assert URL contains inventory and a product is visible
    cy.url().should('include', '/inventory');
    cy.get('.inventory_list').should('be.visible');
  });

  it('shows an error for invalid credentials', () => {
    cy.get('#user-name').type('wrong_user');
    cy.get('#password').type('bad_password');
    cy.get('#login-button').click();

    cy.get('[data-test="error"]').should('be.visible')
      .and('contain.text', 'Username and password do not match any user in this service');
  });
});
```

### Example: Add to cart & cart assertion — `cypress/e2e/cart.cy.js`

```javascript
describe('Cart behavior', () => {
  beforeEach(() => {
    cy.visit('/');
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();
  });

  it('adds product to cart and verifies cart count', () => {
    cy.get('.inventory_item').first().within(() => {
      cy.contains('Add to cart').click();
    });

    // Check cart badge
    cy.get('.shopping_cart_badge').should('contain', '1');

    // Open cart and verify item present
    cy.get('.shopping_cart_link').click();
    cy.get('.cart_item').should('have.length', 1);
  });
});
```

### Fixtures & network stubbing (deterministic tests)
Place fixture files in `cypress/fixtures/user.json`:
```json
{
  "validUser": { "username": "standard_user", "password": "secret_sauce" }
}
```
Use fixtures and `cy.intercept`:
```javascript
it('uses fixture and stubs product API', () => {
  cy.fixture('user').then(user => {
    cy.intercept('GET', '/api/products', { fixture: 'products.json' }).as('getProducts');
    cy.visit('/');

    cy.get('#user-name').type(user.validUser.username);
    cy.get('#password').type(user.validUser.password);
    cy.get('#login-button').click();

    // wait for stubbed API
    cy.wait('@getProducts');
    cy.get('.inventory_item').should('have.length', 6); // based on fixture
  });
});
```
`cy.intercept` can also spy on requests (no stub) or return custom responses, enabling negative or slow-network scenarios.

### Component & integration testing (Cypress ecosystem)
Cypress supports component testing for frameworks like React/Vue (mounting components in a browser). Use `cypress open --component` and the component testing setup to test components in isolation. This is powerful for UI libraries and widgets

### Screenshots & videos
Cypress captures screenshots on failure and can record video of the run (headless). Example run (headless, record off):
```bash
npx cypress run --browser chrome --headless
# screenshots in cypress/screenshots, videos in cypress/videos
```

Manually save a screenshot:
```javascript
cy.screenshot('after-login');
```

### CI Integration — GitHub Actions example
Use the official Cypress GitHub Action (handles browsers and dependencies):
`.github/workflows/cypress.yml`

```yaml
name: Cypress Tests

on: [push, pull_request]

jobs:
  cypress-run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      - name: Install dependencies
        run: npm ci
      - name: Run Cypress tests
        uses: cypress-io/github-action@v5
        with:
          browser: chrome
          headless: true
```
Alternatively run `npm run cy:run` directly in a job runner that has required browsers installed.

#### Debugging tips
- Use `cy.pause()` inside a test to pause the runner and step through interactive commands.
- Add `cy.log('message')` to print to runner console.
- Open DevTools inside the Cypress Test Runner for full console and network inspection.
- If an element is "not visible" or "not interactable", prefer `.scrollIntoView()` or locate a stable selector closer to the element.

#### Best practices & patterns
- Use *Page Object* or *Command pattern*: create reuseable `cypress/support/commands.js` helpers for common actions (e.g., `cy.login()`), but avoid over-abstracting tests so they remain readable.
- Keep tests deterministic: stub external APIs or use a stable test environment.
- Prefer semantic, stable selectors: `data-cy` or `data-test` attributes (avoid fragile CSS or XPath selectors). Example: `<button data-cy="add-to-cart">Add</button>` and `cy.get('[data-cy=add-to-cart]')`.
- Split tests: smoke (fast) on every push, full E2E nightly or on merge.
- Capture artifacts: store screenshots/videos as CI artifacts for failed runs.

### Example custom command `(cypress/support/commands.js)`
```javascript
Cypress.Commands.add('login', (username, password) => {
  cy.visit('/');
  cy.get('#user-name').type(username);
  cy.get('#password').type(password);
  cy.get('#login-button').click();
});
```
Usage in tests:
```javascript
cy.login('standard_user', 'secret_sauce');
```

### Example test-case matrix (for your Post / docs)

|     ID | Scenario            | Steps (summary)                           |              Expected result |
| -----: | ------------------- | ----------------------------------------- | ---------------------------: |
| CY-001 | Successful login    | Visit → type valid creds → click login    |        Redirect to inventory |
| CY-002 | Invalid login error | Visit → type invalid creds → click login  |        Error message visible |
| CY-003 | Add to cart         | Login → click Add on item → open cart     |        Cart badge increments |
| CY-004 | Product API stubbed | Stub products → login → wait for products | Items from fixture displayed |
| CY-005 | Responsive layout   | Set viewport mobile → visit product page  |  Elements adapt, no overflow |

#### When to choose Cypress vs alternatives
- Pick Cypress when you want quick E2E tests with excellent developer UX, automatic waits, and easy stubbing.
- Consider Playwright when you need multi-origin testing, deep cross-browser automation (including WebKit) with a single API.
- Use Selenium/WebDriver when you require legacy integrations or language bindings beyond JS ecosystem. Cypress remains a top choice for modern JS apps due to ergonomics and speed.

#### Wrap-up & resources
Cypress simplifies writing reliable end-to-end tests with a minimal learning curve and strong developer tooling (interactive runner, time travel, automatic waits). Start by writing small, deterministic tests for your most critical user flows, then progressively add coverage and CI integration.

If you want, I can:
- create the actual `cypress/` test files for your repo (ready to paste),
- add `data-cy` attributes into your HTML templates and update tests accordingly, or
- generate a GitHub Actions workflow that archives screenshots/videos as artifacts when tests fail.
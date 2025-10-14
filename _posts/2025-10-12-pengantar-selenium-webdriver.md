---
title: "Introduction to Selenium WebDriver"
date: 2025-10-12 15:00:00
categories: [Software Testing and Quality Assurance]
tags: [STQA]
image: /assets/images/STQA/pengantar-selenium-webdriver.png
---

# Introduction to Selenium WebDriver

This article is a practical introduction to **Selenium WebDriver**: what it is, why use it, how to install and run basic browser automation, common patterns (selectors, waits), examples in Python / Java / JavaScript, simple test scenarios and test cases, CI tips, and best practices. Parts of this material are adapted from the group slides.

---

## What is Selenium & WebDriver

- **Selenium** is an open-source framework for browser automation (functional / end-to-end testing).  
- **WebDriver** is Selenium’s core component that controls browsers (Chrome, Firefox, Edge, Safari) through driver binaries. It exposes a language-agnostic API (bindings for Python, Java, JS, C#).  
- Use cases: UI validation, regression tests, browser compatibility checks, and automated UI workflows.

---

## Why choose Selenium

- Cross-language (Python, Java, JavaScript, C#).  
- Supports major browsers and platforms (Windows, macOS, Linux).  
- Integrates with test frameworks (pytest, JUnit, TestNG, Mocha).  
- Large community and ecosystem (tools, grid, cloud providers).

---

## Quick installation & first-run examples

> Note: prefer using a driver manager so you don't manually download driver binaries.

[View Group 7 Presentation Slides](https://drive.google.com/file/d/1G8XP7xVKPqGMZah4RyEfYVi3NrzOKuCo/view?usp=sharing)

### Python (recommended minimal setup)
```bash
python -m venv .venv
source .venv/bin/activate
pip install selenium webdriver-manager pytest
```
`test_sample.py`
```python
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

def test_open_example():
    service = Service(ChromeDriverManager().install())
    options = webdriver.ChromeOptions()
    # options.add_argument("--headless=new")  # enable headless if needed
    driver = webdriver.Chrome(service=service, options=options)
    driver.get("https://www.saucedemo.com/")
    assert "Swag Labs" in driver.title
    # simple interaction
    driver.find_element(By.ID, "user-name").send_keys("standard_user")
    driver.find_element(By.ID, "password").send_keys("secret_sauce")
    driver.find_element(By.ID, "login-button").click()
    assert "inventory" in driver.current_url
    driver.quit()
```

#### Run
```bash
pytest -q
```

----

### Java (Maven + JUnit + WebDriverManager)
`pom.xml`(snippet)
```xml
<!-- dependencies -->
<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-java</artifactId>
  <version>4.14.0</version>
</dependency>
<dependency>
  <groupId>io.github.bonigarcia</groupId>
  <artifactId>webdrivermanager</artifactId>
  <version>5.4.1</version>
</dependency>
```
`ExampleTest.Java`
```java
import org.junit.jupiter.api.Test;
import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.By;
import static org.junit.jupiter.api.Assertions.assertTrue;

class ExampleTest {
  @Test
  void loginSauceDemo() {
    WebDriverManager.chromedriver().setup();
    var driver = new ChromeDriver();
    driver.get("https://www.saucedemo.com/");
    assertTrue(driver.getTitle().contains("Swag Labs"));
    driver.findElement(By.id("user-name")).sendKeys("standard_user");
    driver.findElement(By.id("password")).sendKeys("secret_sauce");
    driver.findElement(By.id("login-button")).click();
    assertTrue(driver.getCurrentUrl().contains("inventory"));
    driver.quit();
  }
}
```
---

### JavaScript (Node) — selenium-webdriver
```bash
npm init -y
npm install selenium-webdriver chromedriver mocha
```

`test/example.test.js`
```javascript
const { Builder, By } = require('selenium-webdriver');
require('chromedriver');

describe('SauceDemo smoke', function() {
  this.timeout(20000);
  let driver;
  before(async () => {
    driver = await new Builder().forBrowser('chrome').build();
  });
  after(async () => { await driver.quit(); });

  it('login works', async () => {
    await driver.get('https://www.saucedemo.com/');
    await driver.findElement(By.id('user-name')).sendKeys('standard_user');
    await driver.findElement(By.id('password')).sendKeys('secret_sauce');
    await driver.findElement(By.id('login-button')).click();
    const url = await driver.getCurrentUrl();
    if (!url.includes('inventory')) throw new Error('Login failed');
  });
});
```
Run:`npx mocha`

---

### Selectors: choose the right locator
Prefer stable, specific locators:
1. `By.ID` — best (unique, fast).
2. `By.NAME` — good if unique.
3. `By.CSS_SELECTOR` — flexible & performant.
4. `By.XPATH` — powerful but can be brittle; use when necessary.
5. `By.CLASS_NAME` / `By.TAG_NAME` — watch for non-unique usage.

Example CSS:
```css
button.add-to-cart[data-test="add-to-cart"]  /* stable attribute-based selector */
```

### Synchronization: waits (do NOT use time.sleep as first choice)
* Implicit wait (sets a default for find calls) — limited usefulness.
* Explicit wait (recommended) — wait until a condition is true.

Python explicit wait example:
```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

wait = WebDriverWait(driver, 10)
elem = wait.until(EC.element_to_be_clickable((By.ID, "login-button")))
elem.click()
```

#### Useful actions
* Screenshots: driver.save_screenshot("screen.png")
* JavaScript execution: driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
* File upload: send file path to `<input type="file">`.
* Window/Tab: driver.switch_to.window(handle).

#### Headless & CI usage
* Use headless Chrome/Firefox for CI (options.add_argument("--headless=new") or --headless depending on browser).
* Use xvfb on Linux if headless not available (older setups).
* Use driver managers (webdriver-manager / WebDriverManager) to avoid manual binary management in CI.

Github Actions (example)
```yaml
name: E2E Tests
on: [push, pull_request]
jobs:
  e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v4
        with: {python-version: '3.13'}
      - run: pip install -r requirements.txt
      - run: pytest -q
```
If using Java/Node, update actions to install JDK / Node and run `mvn test` or `npm test`.

### Test scenarios & test cases (simple examples from slides)
Test Scenarios
* TS-001: Successful login on SauceDemo.
* TS-002: Failed login with incorrect credentials.
* TS-003: Add product to cart and verify cart count. 

Example Test Cases

|     ID | Test Case           | Steps                                                                    | Expected                             |
| -----: | ------------------- | ------------------------------------------------------------------------ | ------------------------------------ |
| TC-001 | Successful login    | 1. Open site. 2. Enter `standard_user` / `secret_sauce`. 3. Click Login. | Redirect to `inventory` page.        |
| TC-002 | Failed login        | 1. Open site. 2. Enter invalid creds. 3. Click Login.                    | Error message shown (login blocked). |
| TC-003 | Add product to cart | 1. Login success. 2. Click "Add to cart" on a product.                   | Cart icon count increases by 1.      |

#### Best practices & anti-patterns
Do:
* Keep tests small and focused (one logical assertion per test).
* Use Page Object Model (POM) to centralize selectors & actions.
* Parameterize tests for different data sets.
* Run smoke E2E tests frequently; full regression less often.
* Capture screenshots / HTML on failure for debugging in CI.

#### Avoid:
* Hard-coded sleeps.
* Relying on fragile locators (deep XPaths, auto-generated ids).
* Packing too many assertions into one test (hard to debug on failure).

### Page Object Model (short example — Python)
`pages/login_page.py`
```python
from selenium.webdriver.common.by import By

class LoginPage:
    USER = (By.ID, "user-name")
    PASS = (By.ID, "password")
    LOGIN = (By.ID, "login-button")

    def __init__(self, driver):
        self.driver = driver

    def open(self, url):
        self.driver.get(url)

    def login(self, username, password):
        self.driver.find_element(*self.USER).send_keys(username)
        self.driver.find_element(*self.PASS).send_keys(password)
        self.driver.find_element(*self.LOGIN).click()
```

Test:
```python
def test_login():
    lp = LoginPage(driver)
    lp.open("https://www.saucedemo.com/")
    lp.login("standard_user", "secret_sauce")
    assert "inventory" in driver.current_url
```

#### Troubleshooting common Selenium issues
* Driver version mismatch → use a driver manager (auto-download correct version).
* ElementNotInteractable / StaleElementReference → re-locate element or use explicit waits.
* Flaky tests → refactor to remove timing dependencies and avoid shared state between tests.
* Slow CI → reduce screenshot frequency, run parallel jobs, or isolate slow E2E into nightly runs.

#### Next steps & resources
* Use pytest + selenium or JUnit/TestNG + Selenium as your primary test runner.
* Consider Playwright or Cypress for more modern out-of-the-box E2E solutions (note: different trade-offs).
* Cloud providers: BrowserStack, Sauce Labs, LambdaTest for cross-browser grid runs.


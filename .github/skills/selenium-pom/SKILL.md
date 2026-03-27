---
name: selenium-pom
description: "Domain knowledge for Selenium WebDriver with Page Object Model (POM) in JavaScript/Node.js. Covers locators, waits, actions, and E2E test patterns."
---

# Universal Selenium WebDriver + Page Object Model — Domain Knowledge

## Page Object Pattern

### Structure (Generic)
```
pageobjects/
├── BasePage.js          ← Common methods (wait, click, getText)
├── LoginPage.js         ← Login form interactions
├── GenericPage.js       ← Any generic page or feature
```

### Base Page (Generic)
```javascript
const { By, until } = require('selenium-webdriver');

class BasePage {
    constructor(driver) {
        this.driver = driver;
        this.defaultTimeout = 10000;
    }

    async waitForElement(locator, timeout = this.defaultTimeout) {
        return await this.driver.wait(until.elementLocated(locator), timeout);
    }

    async click(locator) {
        const element = await this.waitForElement(locator);
        await this.driver.wait(until.elementIsVisible(element), this.defaultTimeout);
        await element.click();
        return this;
    }

    async type(locator, text) {
        const element = await this.waitForElement(locator);
        await element.clear();
        await element.sendKeys(text);
        return this;
    }

    async getText(locator) {
        const element = await this.waitForElement(locator);
        return await element.getText();
    }

    async isDisplayed(locator) {
        try {
            const element = await this.driver.findElement(locator);
            return await element.isDisplayed();
        } catch {
            return false;
        }
    }
}

module.exports = BasePage;
```

### Page Object Example
```javascript
const BasePage = require('./BasePage');
const { By } = require('selenium-webdriver');

class SearchPage extends BasePage {
    // Locators — centralized at the top
    get searchInput() { return By.id('search-input'); }
    get searchButton() { return By.css('button[data-testid="search-btn"]'); }
    get resultsTable() { return By.css('.results-table'); }
    get firstResult() { return By.css('.results-table tbody tr:first-child'); }
    get noResultsMessage() { return By.css('.no-results'); }

    async searchByName(name) {
        await this.type(this.searchInput, name);
        await this.click(this.searchButton);
        return this;
    }

    async getFirstResultText() {
        return await this.getText(this.firstResult);
    }

    async hasResults() {
        return await this.isDisplayed(this.resultsTable);
    }
}

module.exports = SearchPage;
```

## Key Rules

### Always Use Explicit Waits
```javascript
// CORRECT — explicit wait
await driver.wait(until.elementLocated(By.css('.loaded')), 10000);

// WRONG — never use sleep
await driver.sleep(5000); // Anti-pattern!
```

### Locator Priority
1. `By.id('unique-id')` — fastest, most stable
2. `By.css('[data-testid="name"]')` — custom test attributes
3. `By.css('.class-name')` — stable CSS classes
4. `By.xpath('//div[@role="button"]')` — last resort, fragile

### Method Chaining
```javascript
// Page methods return `this` for chaining
const searchPage = new SearchPage(driver);
await searchPage
    .searchByName('Company ABC')
    .then(page => page.getFirstResultText());
```

### Step Definition Integration
```javascript
const SearchPage = require('../../pageobjects/SearchPage');

When('I search for {string}', async function (name) {
    const searchPage = new SearchPage(this.driver);
    await searchPage.searchByName(name);
});

Then('I should see {string} in the results', async function (expected) {
    const searchPage = new SearchPage(this.driver);
    const text = await searchPage.getFirstResultText();
    expect(text).to.include(expected);
});
```

## Common Patterns

### Handling Dropdowns
```javascript
const { Select } = require('selenium-webdriver');
const select = new Select(await driver.findElement(By.id('dropdown')));
await select.selectByVisibleText('Option Name');
```

### Handling Alerts
```javascript
const alert = await driver.switchTo().alert();
const text = await alert.getText();
await alert.accept(); // or alert.dismiss()
```

### Handling Iframes
```javascript
await driver.switchTo().frame('iframe-name');
// interact with elements inside iframe
await driver.switchTo().defaultContent(); // back to main
```

### Screenshots on Failure
```javascript
After(async function (scenario) {
    if (scenario.result.status === 'FAILED') {
        const screenshot = await this.driver.takeScreenshot();
        this.attach(Buffer.from(screenshot, 'base64'), 'image/png');
    }
});
```

# SmartUI SDK Sample for Cypress — TestMu AI (Formerly LambdaTest)

Welcome to the SmartUI SDK sample for Cypress. This repository demonstrates how to integrate SmartUI visual regression testing with Cypress.

## Repository Structure

```
smartui-cypress-sdk-sample/
├── cypress/
│   └── e2e/
│       └── smartuiSDKLocal.cy.js    # Test file
├── cypress.config.js                # Cypress configuration
├── package.json                     # Dependencies
└── smartui-web.json                # SmartUI config (create with npx smartui config:create)
```

## 1. Prerequisites and Environment Setup

### Prerequisites

- Node.js installed
- Cypress >= 10.0.0 (SmartUI SDK only supports Cypress versions >= 10.0.0)
- Chrome browser (for Local tests)

### Environment Setup

**For Local:**
```bash
export PROJECT_TOKEN='your_project_token'
```

## 2. Initial Setup and Dependencies

### Clone the Repository

```bash
git clone https://github.com/LambdaTest/smartui-cypress-sdk-sample
cd smartui-cypress-sdk-sample
```

### Install Dependencies

Install required NPM modules for TestMu AI Smart UI Cypress SDK:

```bash
npm i @lambdatest/smartui-cli @lambdatest/cypress-driver cypress@^13
```

**Dependencies included:**
- `@lambdatest/smartui-cli` - SmartUI CLI
- `@lambdatest/cypress-driver` - SmartUI Cypress driver
- `cypress@^13` - Cypress framework

### Configure Cypress Support File

Add the following import to your `cypress/support/e2e.js` file:

```javascript
import '@lambdatest/cypress-driver'
```

### Create SmartUI Configuration

```bash
npx smartui config:create smartui-web.json
```

## 3. Steps to Integrate Screenshot Commands into Codebase

The SmartUI screenshot function is already implemented in the repository.

**Test File** (`cypress/e2e/smartuiSDKLocal.cy.js`):
```javascript
describe('Test Case name', () => {
  beforeEach(() => {
    cy.visit('Required URL')
  })

  it('SmartUI Snapshot', () => {
    cy.smartuiSnapshot('Screenshot Name');
  })
})
```

**Note**: The code is already configured and ready to use. You can modify the URL and screenshot name if needed.

## 4. Execution and Commands

### Local Execution

```bash
npx smartui exec -- npx cypress run --spec cypress/e2e/smartuiSDKLocal.cy.js --browser chrome --headed
```

**Note**: The `--config` flag is optional if your config file is named `smartui-web.json` and located in the current directory. If you need to specify a different config file, use: `npx smartui --config <path> exec -- npx cypress run --spec cypress/e2e/smartuiSDKLocal.cy.js --browser chrome --headed`

## Test Files

### Test File (`cypress/e2e/smartuiSDKLocal.cy.js`)

- Runs Cypress locally using Chrome
- Requires Chrome browser installed
- Takes screenshot with name: `Screenshot Name`

## Configuration

### Cypress Config (`cypress.config.js`)

The Cypress configuration file is pre-configured for SmartUI integration.

### SmartUI Config (`smartui-web.json`)

Create the SmartUI configuration file using:
```bash
npx smartui config:create smartui-web.json
```

The default configuration includes:
- Browsers: chrome, firefox, safari, edge
- Viewports: 1920, 1366, 360 (full page screenshots by default)
- Optional: `waitForPageRender` and `waitForTimeout` for slow-loading pages

## Best Practices

### Screenshot Naming

- Use descriptive, unique names for each screenshot
- Include test context (e.g., `login-form-filled`, `checkout-step-2`)
- Avoid special characters
- Use consistent naming conventions

### When to Take Screenshots

- After critical user interactions
- Before and after form submissions
- At different viewport sizes
- After page state changes

### Cypress-Specific Tips

- Use `cy.wait()` before screenshots for dynamic content
- Take screenshots after `cy.visit()` completes
- Use `cy.viewport()` to test responsive designs
- Combine with Cypress commands for better test flow

### Example: Screenshot After Interaction

```javascript
describe('Homepage Tests', () => {
  beforeEach(() => {
    cy.visit('https://www.lambdatest.com')
  })

  it('Homepage Visual Test', () => {
    cy.smartuiSnapshot('homepage-initial')
    
    // Interact with page
    cy.get('#search').type('Cypress')
    cy.wait(1000) // Wait for results
    
    cy.smartuiSnapshot('homepage-after-search')
  })
})
```

## Common Use Cases

### Responsive Testing

```javascript
describe('Responsive Tests', () => {
  it('Desktop View', () => {
    cy.viewport(1920, 1080)
    cy.visit('https://www.lambdatest.com')
    cy.smartuiSnapshot('homepage-desktop')
  })

  it('Tablet View', () => {
    cy.viewport(768, 1024)
    cy.visit('https://www.lambdatest.com')
    cy.smartuiSnapshot('homepage-tablet')
  })

  it('Mobile View', () => {
    cy.viewport(375, 667)
    cy.visit('https://www.lambdatest.com')
    cy.smartuiSnapshot('homepage-mobile')
  })
})
```

### Multi-Step Flow Testing

```javascript
describe('Checkout Flow', () => {
  it('Complete Checkout Visual Test', () => {
    cy.visit('https://example.com/checkout')
    cy.smartuiSnapshot('checkout-step-1')
    
    cy.get('#next-step').click()
    cy.wait(500)
    cy.smartuiSnapshot('checkout-step-2')
    
    cy.get('#complete').click()
    cy.wait(1000)
    cy.smartuiSnapshot('checkout-complete')
  })
})
```

## CI/CD Integration

### GitHub Actions Example

```yaml
name: Cypress SmartUI Tests

on: [push, pull_request]

jobs:
  visual-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run Cypress with SmartUI
        env:
          PROJECT_TOKEN: ${{ secrets.SMARTUI_PROJECT_TOKEN }}
        run: |
          npx smartui exec -- npx cypress run --spec cypress/e2e/smartuiSDKLocal.cy.js --browser chrome --headed
```

## Troubleshooting

### Issue: `cy.smartuiSnapshot is not a function`

**Solution**: Ensure the driver is imported in `cypress/support/e2e.js`:
```javascript
import '@lambdatest/cypress-driver'
```

### Issue: Screenshots not captured

**Solution**:
1. Verify `PROJECT_TOKEN` is set
2. Check Cypress version (requires >= 10.0.0)
3. Ensure test completes successfully
4. Wait a few moments for processing

### Issue: `Cypress version mismatch`

**Solution**: Install compatible Cypress version:
```bash
npm install cypress@^13
```

### Issue: Timeout errors

**Solution**: Add waits before screenshots:
```javascript
cy.visit('https://example.com')
cy.wait(2000) // Wait for page load
cy.smartuiSnapshot('screenshot')
```

## Configuration Tips

### Optimizing `smartui-web.json` for Cypress

```json
{
  "web": {
    "browsers": ["chrome", "firefox", "edge"],
    "viewports": [
      [1920, 1080],
      [1366, 768],
      [375, 667]
    ],
    "waitForPageRender": 30000,
    "waitForTimeout": 2000
  }
}
```

## View Results

After running the tests, visit your SmartUI project dashboard to view the captured screenshots and compare them with baseline builds.

## Additional Resources

- [SmartUI Cypress Onboarding Guide](https://www.testmuai.com/support/docs/smartui-onboarding-cypress/)
- [Cypress Documentation](https://docs.cypress.io/)
- [TestMu AI Cypress Documentation](https://www.testmuai.com/support/docs/cypress-testing/)
- [SmartUI Dashboard](https://smartui.lambdatest.com/)
- [TestMu AI Community](https://community.testmuai.com/)

## 🚀 LambdaTest is Now TestMu AI

👋 Welcome to TestMu AI, the next evolution of LambdaTest. As of January 2026, [LambdaTest is Now TestMu AI](https://www.testmuai.com/lambdatest-is-now-testmuai/) - we have evolved from a cross-browser testing cloud into a unified, AI-native quality engineering platform designed for the modern DevOps era.

Whether you have been part of the LambdaTest community for years or are just discovering TestMu AI, our mission remains the same: to help you ship faster with high-scale test execution, autonomous testing, and deep quality analytics.

**🔄 Our Rebrand Journey**

We chose the name TestMu AI to reflect our shift towards intelligent, autonomous testing. While our identity has changed, our core technology and commitment to the testing community stay the same.

👉 Find [LambdaTest's New Home](https://www.testmuai.com/).

**🔭 Explore TestMu AI**

The same infrastructure LambdaTest customers relied on, now delivered through autonomous AI agents.

- [KaneAI](https://www.testmuai.com/kane-ai/)
- [Agent-to-Agent Testing](https://www.testmuai.com/agent-to-agent-testing/)
- [HyperExecute](https://www.testmuai.com/hyperexecute/)
- [Real Device Cloud](https://www.testmuai.com/real-device-cloud/)
- [Pricing](https://www.testmuai.com/pricing/)
- [Documentation](https://www.testmuai.com/support/docs/)
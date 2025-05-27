# Week 3: Testing Nightmare - "QA Bottleneck Crisis" 🧪
**VelocityTech Transformation Week 3**

**Story Context:** Ana is overwhelmed with 47 weekend bug reports, releases delayed weeks due to manual testing  
**Focus:** Implementing test automation and shifting left with quality practices  
**Characters in Focus:** Ana's overwhelm, developers saying "works on my machine," late bug discovery

---

## 🔥 The Weekly Crisis: "The Great Testing Avalanche"
**Setting:** VelocityTech office, Monday 8:30 AM. Ana's desk buried under sticky notes and bug reports  
**The Problem:** Weekend user activity revealed dozens of bugs, all requiring manual verification

**Scene:** [VelocityTech main floor. Ana's workspace looks like a hurricane hit it - multiple monitors showing bug tracking systems, printed test cases scattered everywhere, empty coffee cups forming a defensive barrier.]

**ANA** *(staring at her screen in horror)*  
> "Forty-seven. Forty-seven new bug reports since Friday evening."  
> "Users found edge cases I never even thought to test!"  
> *(frantically scrolling through bug list)* "How am I supposed to verify all these AND test the new features?"

**LUKA** *(defensively, approaching Ana's desk)*  
> "My payment gateway worked perfectly in my local environment!"  
> "I tested it with the happy path scenarios - user enters valid card, payment processes successfully."  
> "How was I supposed to know someone would try to pay with an expired card from 1995?"

**FILIP** *(joining the conversation, looking tired)*  
> "I got twelve emergency calls this weekend. Users can't complete purchases."  
> "Every time I fix one issue, three more seem to pop up."  
> "It's like whack-a-mole, but the moles multiply."

**MAJA** *(checking her project timeline, stress evident)*  
> "The client presentation is Thursday. We promised them a stable payment system."  
> "Ana, how long will it take to test all these fixes?"

**ANA** *(calculating frantically on a sticky note)*  
> "If I test every bug fix manually... that's 3-4 hours per bug..."  
> "Plus regression testing to make sure fixes don't break other things..."  
> *(looking up with exhausted eyes)* "Maybe two weeks? If I don't sleep."

**LUKA** *(frustrated)*  
> "Two weeks?! But I already fixed most of these bugs!"  
> "They work fine on my machine. Can't we just deploy them?"

**ANA** *(voice rising)*  
> "That's what you said about the last batch! And look what happened!"  
> "I found five critical bugs that would have crashed the payment system!"

**MARKO** *(entering, surveying the chaos)*  
> "Let me guess - we're discovering bugs too late in the process again?"  
> *(looking at you)* "What do you see here that they don't?"

**FILIP**  
> "We need more testers. Ana can't be our single point of failure."

**MAJA**  
> "More testers cost money. And training them would take months."

**LUKA**  
> "Or Ana could just test the important stuff and skip the edge cases."

**ANA** *(standing up, frustrated)*  
> "Edge cases become main cases when users find them in production!"  
> "Every bug that reaches customers damages our reputation!"

**[All eyes turn to you - the intern who supposedly knows about 'modern development practices'.]**

**MARKO** *(to you)*  
> "They're thinking about this as a resource problem. What kind of problem do you see?"  

**[Your systems thinking activates. This isn't about having more testers - it's about testing at the wrong time, in the wrong way.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Design automated testing strategies** - Implement the testing pyramid with unit, integration, and end-to-end tests that catch bugs early
2. **Shift testing left in the development process** - Integrate testing into development workflow so bugs are found in minutes, not weeks
3. **Create test-driven development practices** - Write tests first to guide development and ensure comprehensive coverage

**Real-world Connection:** Test automation skills are highly valued in Croatian IT companies. Many organizations struggle with manual testing bottlenecks, making automation expertise a significant competitive advantage for developers and QA professionals.

---

## 📚 The Knowledge Foundation

### Test Automation Principles and the Testing Pyramid
**The Problem it Solves:** Manual testing bottlenecks, late bug discovery, inconsistent test execution, human error  
**How it Works:** Automated tests run consistently, quickly, and catch regressions immediately

**The Testing Pyramid:**
```
    /\
   /E2E\     ← Few, slow, expensive (User Interface)
  /____\
 /      \
/Integration\ ← Some, medium speed (API/Service level)
\____________/
\            /
 \Unit Tests/ ← Many, fast, cheap (Function/Class level)  
  \________/
```

**Unit Tests (Base of Pyramid):**
- Test individual functions/methods in isolation
- Fast execution (milliseconds)
- Easy to debug when they fail
- Should be 70-80% of all tests

**Integration Tests (Middle):**
- Test how components work together
- Test database connections, API calls
- Medium execution time (seconds)
- Should be 15-25% of all tests

**End-to-End Tests (Top):**
- Test complete user workflows
- Browser automation, full system testing
- Slow execution (minutes)
- Should be 5-10% of all tests

**Real-world Example:** Google runs millions of automated tests on every code change  
**VelocityTech Connection:** Ana's manual testing crisis could be solved by implementing this pyramid

### Shift-Left Testing and Early Bug Detection
**The Problem it Solves:** Bugs found late in development cycle are expensive and disruptive to fix  
**How it Works:** Move testing activities earlier in the development process

**Traditional Process (Current VelocityTech):**
```
Develop → Manual Test → Deploy → Bug Discovery → Fix → Repeat
(weeks)    (days)       (hours)  (immediate)   (days)
```

**Shift-Left Process (Target):**
```
Test-Driven Development → Automated Testing → Continuous Integration
(immediate feedback)      (seconds)           (minutes)
```

**Benefits of Shift-Left:**
- **Faster feedback** - Developers know immediately if they broke something
- **Cheaper fixes** - Bugs caught in development cost 10x less than production bugs
- **Better design** - Writing tests first leads to better code architecture
- **Reduced risk** - Fewer surprises in production

**Real-world Example:** Netflix catches most bugs in development through extensive automated testing  
**VelocityTech Connection:** If Luka had unit tests for his payment gateway, edge cases would be caught immediately

### Test-Driven Development (TDD) Fundamentals
**The Problem it Solves:** Untested code, poor design, missing requirements understanding  
**How it Works:** Write failing test first, then write minimal code to make it pass

**TDD Cycle (Red-Green-Refactor):**
1. **Red** - Write a failing test for desired functionality
2. **Green** - Write minimal code to make the test pass
3. **Refactor** - Improve code quality while keeping tests green

**TDD Example:**
```javascript
// 1. RED - Write failing test first
test('calculateTotal should add tax to price', () => {
    expect(calculateTotal(100, 0.10)).toBe(110);
});

// 2. GREEN - Write minimal code to pass
function calculateTotal(price, tax) {
    return price + (price * tax);
}

// 3. REFACTOR - Improve while keeping tests green
function calculateTotal(price, taxRate) {
    if (price < 0 || taxRate < 0) {
        throw new Error('Price and tax rate must be positive');
    }
    return price * (1 + taxRate);
}
```

**Real-world Example:** Many successful startups use TDD to maintain code quality while moving fast  
**VelocityTech Connection:** TDD would have helped Luka think through edge cases before implementing

**Key Takeaways:**
- Test automation moves testing from bottleneck to enabler
- Testing pyramid balances speed, cost, and coverage
- Shift-left testing catches bugs when they're cheapest to fix
- TDD improves both code quality and requirements understanding

---

## 🛠️ Lab Mission: "The Testing Revolution"
**Scenario:** Ana is drowning in manual testing while bugs slip through to production  
**Your Mission:** Implement automated testing that catches bugs in development instead of production  
**Success Criteria:** Payment gateway with comprehensive automated tests that run in under 2 minutes

### Phase 1: Unit Testing Foundation (60 minutes)
**Objective:** Create comprehensive unit tests for VelocityTech's payment gateway

**Steps:**
1. **Set Up Testing Framework**
   ```bash
   # Initialize Node.js project with testing framework
   npm init -y
   npm install --save-dev jest
   
   # Create basic project structure
   mkdir src tests
   touch src/payment-gateway.js tests/payment-gateway.test.js
   
   # Add test script to package.json
   echo '"test": "jest"' # Add to scripts section
   ```
   - Expected output: Jest testing framework ready to use
   - Troubleshooting: If npm install fails, check Node.js version compatibility

2. **Write Failing Tests First (TDD)**
   ```javascript
   // tests/payment-gateway.test.js
   const PaymentGateway = require('../src/payment-gateway');
   
   describe('PaymentGateway', () => {
     let gateway;
     
     beforeEach(() => {
       gateway = new PaymentGateway();
     });
   
     test('should process valid payment successfully', () => {
       const payment = {
         amount: 100.00,
         cardNumber: '4111111111111111',
         expiryDate: '12/25',
         cvv: '123'
       };
       
       const result = gateway.processPayment(payment);
       
       expect(result.success).toBe(true);
       expect(result.transactionId).toBeDefined();
     });
   
     test('should reject payment with expired card', () => {
       const payment = {
         amount: 100.00,
         cardNumber: '4111111111111111',
         expiryDate: '12/20', // Expired
         cvv: '123'
       };
       
       const result = gateway.processPayment(payment);
       
       expect(result.success).toBe(false);
       expect(result.error).toContain('expired');
     });
   
     test('should reject payment with invalid amount', () => {
       const payment = {
         amount: -50.00, // Negative amount
         cardNumber: '4111111111111111',
         expiryDate: '12/25',
         cvv: '123'
       };
       
       const result = gateway.processPayment(payment);
       
       expect(result.success).toBe(false);
       expect(result.error).toContain('amount');
     });
   });
   ```

3. **Implement Code to Make Tests Pass**
   ```javascript
   // src/payment-gateway.js
   class PaymentGateway {
     processPayment(payment) {
       // Validate amount
       if (payment.amount <= 0) {
         return {
           success: false,
           error: 'Payment amount must be positive'
         };
       }
   
       // Validate expiry date
       const currentDate = new Date();
       const [month, year] = payment.expiryDate.split('/');
       const expiryDate = new Date(2000 + parseInt(year), parseInt(month) - 1);
       
       if (expiryDate < currentDate) {
         return {
           success: false,
           error: 'Card has expired'
         };
       }
   
       // Validate card number (basic Luhn algorithm check)
       if (!this.isValidCardNumber(payment.cardNumber)) {
         return {
           success: false,
           error: 'Invalid card number'
         };
       }
   
       // Success case
       return {
         success: true,
         transactionId: 'TXN_' + Date.now(),
         amount: payment.amount
       };
     }
   
     isValidCardNumber(cardNumber) {
       // Simplified Luhn algorithm implementation
       const digits = cardNumber.replace(/\D/g, '');
       let sum = 0;
       let isEven = false;
   
       for (let i = digits.length - 1; i >= 0; i--) {
         let digit = parseInt(digits[i]);
   
         if (isEven) {
           digit *= 2;
           if (digit > 9) {
             digit -= 9;
           }
         }
   
         sum += digit;
         isEven = !isEven;
       }
   
       return sum % 10 === 0;
     }
   }
   
   module.exports = PaymentGateway;
   ```

4. **Run Tests and Verify Coverage**
   ```bash
   # Run all tests
   npm test
   
   # Run tests with coverage report
   npx jest --coverage
   
   # Expected output: All tests passing, >90% coverage
   ```

**Checkpoint:** Complete unit test suite with high coverage for payment gateway

### Phase 2: Integration Testing (60 minutes)
**Objective:** Test how payment gateway integrates with external services

**Steps:**
1. **Set Up Integration Test Environment**
   ```javascript
   // tests/integration/payment-integration.test.js
   const PaymentGateway = require('../../src/payment-gateway');
   const DatabaseService = require('../../src/database-service');
   const EmailService = require('../../src/email-service');
   
   describe('Payment Integration Tests', () => {
     let gateway, database, emailService;
     
     beforeEach(async () => {
       // Set up test database
       database = new DatabaseService('test_db');
       await database.connect();
       
       // Set up email service with mock
       emailService = new EmailService('test_mode');
       
       gateway = new PaymentGateway(database, emailService);
     });
     
     afterEach(async () => {
       await database.clearTestData();
       await database.disconnect();
     });
   });
   ```

2. **Test Complete Payment Flow**
   ```javascript
   test('should save payment record to database on success', async () => {
     const payment = {
       amount: 100.00,
       cardNumber: '4111111111111111',
       expiryDate: '12/25',
       cvv: '123',
       customerEmail: 'test@example.com'
     };
     
     const result = await gateway.processPaymentWithPersistence(payment);
     
     expect(result.success).toBe(true);
     
     // Verify database record was created
     const savedPayment = await database.findPayment(result.transactionId);
     expect(savedPayment).toBeDefined();
     expect(savedPayment.amount).toBe(100.00);
   });
   
   test('should send confirmation email on successful payment', async () => {
     const payment = {
       amount: 50.00,
       cardNumber: '4111111111111111',
       expiryDate: '12/25',
       cvv: '123',
       customerEmail: 'customer@example.com'
     };
     
     const result = await gateway.processPaymentWithPersistence(payment);
     
     expect(result.success).toBe(true);
     
     // Verify email was sent
     const sentEmails = emailService.getSentEmails();
     expect(sentEmails).toHaveLength(1);
     expect(sentEmails[0].to).toBe('customer@example.com');
     expect(sentEmails[0].subject).toContain('Payment Confirmation');
   });
   ```

3. **Test Error Scenarios**
   ```javascript
   test('should handle database connection failure gracefully', async () => {
     // Simulate database failure
     await database.disconnect();
     
     const payment = {
       amount: 100.00,
       cardNumber: '4111111111111111',
       expiryDate: '12/25',
       cvv: '123'
     };
     
     const result = await gateway.processPaymentWithPersistence(payment);
     
     expect(result.success).toBe(false);
     expect(result.error).toContain('database');
   });
   ```

**Checkpoint:** Integration tests verify external service interactions

### Phase 3: End-to-End Test Automation (60 minutes)
**Objective:** Automate browser-based testing of complete user workflows

**Steps:**
1. **Set Up Browser Automation**
   ```bash
   # Install Playwright for browser automation
   npm install --save-dev @playwright/test
   npx playwright install
   
   # Create E2E test configuration
   touch playwright.config.js tests/e2e/payment-flow.spec.js
   ```

2. **Write Complete User Journey Test**
   ```javascript
   // tests/e2e/payment-flow.spec.js
   const { test, expect } = require('@playwright/test');
   
   test.describe('Payment Flow E2E Tests', () => {
     test('user can complete successful payment', async ({ page }) => {
       // Navigate to payment page
       await page.goto('http://localhost:3000/payment');
       
       // Fill in payment form
       await page.fill('[data-testid="amount"]', '99.99');
       await page.fill('[data-testid="card-number"]', '4111111111111111');
       await page.fill('[data-testid="expiry"]', '12/25');
       await page.fill('[data-testid="cvv"]', '123');
       await page.fill('[data-testid="email"]', 'test@example.com');
       
       // Submit payment
       await page.click('[data-testid="submit-payment"]');
       
       // Verify success message
       await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
       await expect(page.locator('[data-testid="transaction-id"]')).toContainText('TXN_');
     });
     
     test('user sees appropriate error for expired card', async ({ page }) => {
       await page.goto('http://localhost:3000/payment');
       
       // Fill form with expired card
       await page.fill('[data-testid="amount"]', '50.00');
       await page.fill('[data-testid="card-number"]', '4111111111111111');
       await page.fill('[data-testid="expiry"]', '12/20'); // Expired
       await page.fill('[data-testid="cvv"]', '123');
       
       await page.click('[data-testid="submit-payment"]');
       
       // Verify error message
       await expect(page.locator('[data-testid="error-message"]')).toContainText('expired');
     });
   });
   ```

3. **Create Test Data Management**
   ```javascript
   // tests/helpers/test-data.js
   class TestDataManager {
     static getValidPayment() {
       return {
         amount: '100.00',
         cardNumber: '4111111111111111',
         expiryDate: '12/25',
         cvv: '123',
         email: 'test@example.com'
       };
     }
     
     static getExpiredCardPayment() {
       return {
         ...this.getValidPayment(),
         expiryDate: '12/20'
       };
     }
     
     static getInvalidAmountPayment() {
       return {
         ...this.getValidPayment(),
         amount: '-50.00'
       };
     }
   }
   
   module.exports = TestDataManager;
   ```

**Checkpoint:** Complete E2E test suite covering critical user paths

### Validation & Testing
**How to verify everything works:**
1. **Unit Test Validation** - `npm test -- --coverage` shows >90% coverage
2. **Integration Test Validation** - `npm run test:integration` passes all scenarios
3. **E2E Test Validation** - `npx playwright test` completes user journeys successfully

**Expected Results:**
- **Unit Tests:** 20+ tests covering all payment scenarios, <1 second execution
- **Integration Tests:** 8+ tests covering service interactions, <10 seconds execution
- **E2E Tests:** 5+ tests covering user workflows, <2 minutes execution
- **Total Test Runtime:** Under 3 minutes for complete suite
- **Bug Detection:** Tests catch payment edge cases that previously reached production

---

## 🔧 New Tools in Your Arsenal

### Jest Testing Framework
- **Purpose:** Unit and integration testing with powerful assertion library
- **Key Commands:**
  - `npm test` - Run all tests
  - `jest --watch` - Run tests in watch mode
  - `jest --coverage` - Generate coverage report
- **Pro Tips:** Use `describe` blocks to organize related tests
- **Common Pitfalls:** Don't test implementation details, test behavior

### Playwright Browser Automation
- **Purpose:** End-to-end testing with real browser automation
- **Key Features:**
  - Multi-browser support (Chrome, Firefox, Safari)
  - Network interception and mocking
  - Screenshots and video recording
- **Pro Tips:** Use data-testid attributes for stable element selection
- **Common Pitfalls:** Keep E2E tests focused on critical user paths only

### Test-Driven Development Workflow
- **Purpose:** Drive development with failing tests first
- **Key Steps:**
  - Red: Write failing test
  - Green: Make test pass with minimal code
  - Refactor: Improve code while keeping tests green
- **Pro Tips:** Start with simplest test case, gradually add complexity
- **Common Pitfalls:** Don't write too much code before making tests pass

---

## 🔧 Common Issues & Solutions

### Issue #1: "Unit tests are too slow"
**Symptoms:** Test suite takes minutes to run, developers avoid running tests
**Cause:** Tests are doing too much integration work, database calls, network requests
**Solution:**
```javascript
// Bad: Unit test making real API call
test('should validate payment with bank API', async () => {
  const result = await bankAPI.validateCard('4111111111111111');
  expect(result.valid).toBe(true);
});

// Good: Unit test with mock
test('should validate payment with bank API', async () => {
  bankAPI.validateCard = jest.fn().mockResolvedValue({ valid: true });
  const result = await bankAPI.validateCard('4111111111111111');
  expect(result.valid).toBe(true);
});
```
**Prevention:** Mock external dependencies in unit tests

### Issue #2: "E2E tests are flaky and unreliable"
**Symptoms:** Tests pass sometimes, fail other times, no clear pattern
**Cause:** Race conditions, timing issues, non-deterministic behavior
**Solution:**
```javascript
// Bad: Using fixed timeouts
await page.waitForTimeout(3000);

// Good: Waiting for specific conditions
await page.waitForSelector('[data-testid="success-message"]', { state: 'visible' });
await expect(page.locator('[data-testid="transaction-id"]')).toBeVisible();
```
**Prevention:** Always wait for specific elements/conditions, not arbitrary time periods

### Issue #3: "Tests pass but bugs still reach production"
**Symptoms:** Comprehensive test suite shows green, but users find bugs
**Cause:** Tests don't cover real user scenarios or edge cases
**Solution:**
```javascript
// Bad: Only testing happy path
test('should process payment', () => {
  const result = processPayment({ amount: 100, card: '4111111111111111' });
  expect(result.success).toBe(true);
});

// Good: Testing edge cases based on production bugs
test('should handle payment with special characters in name', () => {
  const payment = {
    amount: 100,
    card: '4111111111111111',
    name: "O'Connor-Smith Jr." // Real user name patterns
  };
  const result = processPayment(payment);
  expect(result.success).toBe(true);
});
```
**Prevention:** Use production bug reports to drive new test cases

---

## 🎭 Character Evolution This Week

### Anxious Ana's Transformation
**Monday:** "Automation will replace me! I'll be out of a job!"
**Tuesday:** "Wait... these unit tests catch bugs I would have missed manually..."
**Wednesday:** "I can focus on complex scenarios instead of repetitive testing!"
**Thursday:** "Writing tests helps me understand the requirements better."
**Friday:** "I'm not a bottleneck anymore - I'm a quality champion!"
**Key Quote:** "Automation isn't replacing me, it's amplifying my expertise."

### Legacy Luka's Testing Journey
**Monday:** "Writing tests slows down development. It's extra work."
**Tuesday:** "This TDD thing is weird... writing tests before code feels backwards."
**Wednesday:** "Actually, tests help me think through edge cases I would have missed."
**Thursday:** "My code is more reliable now. Tests caught three bugs before Ana even saw them."
**Friday:** "I can refactor confidently knowing tests will catch any regressions."
**Key Quote:** "Tests aren't slowing me down - they're making me faster and more confident."

### Firefighter Filip's Quality Revolution
**Monday:** "More testing infrastructure to maintain? Great, more complexity."
**Tuesday:** "These automated tests run faster than I can manually verify fixes."
**Wednesday:** "Production incidents are down 60% since we started catching bugs early."
**Thursday:** "I can deploy fixes confidently knowing the test suite validates everything."
**Friday:** "My weekend emergency calls have almost disappeared."
**Key Quote:** "Prevention through testing beats midnight firefighting every time."

### Manager Maja's Strategic Insight
**Monday:** "How much time will this testing automation take to implement?"
**Tuesday:** "Wait, we can catch bugs in minutes instead of weeks?"
**Wednesday:** "Client confidence is up - they love that we find issues before they do."
**Thursday:** "Our velocity is actually increasing with better quality practices."
**Friday:** "Testing isn't a cost center - it's a competitive advantage."
**Key Quote:** "Quality isn't expensive. Poor quality is what costs us."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 3 Improvements:**

| Metric | Week 2 State | After This Week | Target (Week 15) |
|--------|--------------|------------------|------------------|
| Lead Time | 28 days | 25 days | 2 hours |
| Bug Detection Time | 3-5 days | 5 minutes | Immediate |
| Test Execution Time | 8 hours (manual) | 3 minutes (automated) | <1 minute |
| Test Coverage | 0% | 85% | 95% |
| Production Bugs | 30/month | 8/month | <2/month |
| QA Bottleneck Factor | 5 days/release | 2 hours/release | Real-time |
| Developer Confidence | 4/10 | 7/10 | 9/10 |

**Key Improvements:**
- **Quality Shift Left:** Bugs caught in development instead of production
- **Speed Increase:** 160x faster test execution (8 hours → 3 minutes)
- **Confidence Boost:** Developers can refactor and deploy without fear

---

## 🇭🇷 Croatian IT Market Reality
Test automation skills are in extremely high demand in Croatian companies. Many organizations still rely heavily on manual testing, creating bottlenecks similar to Ana's situation. Companies like Infobip, Rimac, and Span actively seek developers and QA professionals who can implement automated testing strategies. Your test automation expertise makes you immediately valuable in the Croatian market, where these skills command premium salaries.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about the testing crisis?** How one person becoming a bottleneck affects the entire development process
2. **Which character's transformation was most dramatic?** Ana's shift from fear of automation to embracing it as an amplifier
3. **How did TDD change your approach to coding?** Writing tests first made requirements clearer and code more reliable
4. **What questions do you still have?** How to balance test automation with exploratory testing needs

**Team Discussion Points:**
- How would each VelocityTech character feel about mandatory test coverage requirements?
- What resistance might we encounter from other teams who see testing as "overhead"?
- How would you convince skeptical managers to invest in test automation infrastructure?

**Preparation for Next Week:**
Luka arrives Wednesday morning: "My code works perfectly on my laptop, but the build server says it failed. Something about 'missing dependencies' and 'environment differences'?" The build automation challenge begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Test Driven Development: By Example" by Kent Beck - TDD fundamentals
- "The Art of Unit Testing" by Roy Osherove - Comprehensive testing guide

**Hands-on Practice:**
- Practice TDD with coding katas (simple programming exercises)
- Set up automated testing for your personal projects

**Industry Insights:**
- "State of Testing Report" (annual) - Industry testing trends and tools
- Croatian QA community meetups - Network with local testing professionals

**Advanced Topics:**
- Property-based testing for edge case discovery
- Visual regression testing for UI changes
- Performance testing integration with CI/CD

---

*Next up: Week 4 - Build Mayhem: "It Works on My Machine" →*

> *"The best time to write a test is before you write the code. The second best time is right now."* - Testing Philosophy


# setup 
- npm init playwright@latest 
then, follow instructions...

# Run test case
- npm test  -> to run all test cases
- npx playwright test folder_name/file_name.spec.ts -> run specific test case
ex:- npx playwright test tests/example.spec.ts 


Note:-   npx playwright show-report -> ek browser open(http://localhost:9323/) hoga and waha pe full details show hoga test case ka


# INTRODUCTION
- Playwright ek open-source automation tool hai jo web browsers ke tasks automate karta hai.
Iska use mainly web application testing ke liye hota hai, lekin aap ise scraping, form filling, navigation, aur screenshots ke liye bhi use kar sakte ho.

- Playwright is for E2E testing.  Playwright is not used for unit testing framework

* what is End-to-End (E2E) Testing?
- Unit tests check small pieces of code (functions/classes).
  But the real application has multiple layers:
     - Frontend UI (buttons, forms)
     - Backend APIs / database
     - Third-party services

  -> E2E tests check that all layers together work as expected.

Note :
-> Unit tests = test functions, classes
-> Integration tests = test single file or a group of files.
-> E2E tests = the whole application like a real user sees it


- Chromium, Firefox, WebKit these are browser engines — matlab, ye wo software hote hain jo HTML, CSS, JavaScript  ko render (display) karte hain and Events handle karta hai (clicks, inputs, navigation, etc.) aur webpage run karta hain.

- 
| Engine              | Use karta browser   | Kaam                         |
| ------------------- | ------------------- | ---------------------------- |
| **Chromium**        | Chrome, Edge, Brave | Google ka open-source engine |
| **Firefox**         | Mozilla Firefox     | Mozilla ka engine            |
| **WebKit**          | Safari              | Apple ka engine              |

- - Playwright ka main kaam hai: “Browser ko automatically control karna (open, click, type, navigate, etc.)”

- To agar tumhe browser me kuch karwana hai (button click, link open, text fill, etc.),
to tumhe uske engine ke sath baat karni padti hai.

- Playwright kaise baat karta hai browser se?
  => Playwright har browser engine ke liye ek “driver” rakhta hai.
     Ye driver Playwright ke commands (like page.click(), page.goto()) ko browser engine samajhne layak format me convert karta hai.

# FILE NAME 
- to write any test case ,so file name should be " file_name.specs.ts " (specs-specifications) (.ts ke jagah .js, .java, .py , .cs(for c#)  bhi ho sakta h) 


#  How Playwright is used in CI/CD ?
- Playwright is used for writing E-2-E test cases for frontend , backend...  
- Github action me hum uss test case ka work flow likhke test case run karte h .



# 1. Import in TypeScript
import { chromium, firefox, webkit, Browser, Page, test, expect } from '@playwright/test';

Explanation:->
- chromium, firefox, webkit → Browser engines
- Browser-Browser instance ka type
  ex:- Agar aap chromium.launch() karte ho, to ek Chromium browser start hota hai.

- Page → New Tab inside browser
- test → Test case define karne ke liye
- expect ka use result verify karne ke liye hota hai.
   Example: expect(title).toBe("My Page") → Check karta hai ki page ka title correct hai ya nahi.




# 2. Launch Browser
const browser: Browser = await chromium.launch({ headless: false });
const page: Page = await browser.newPage();
await page.goto('https://example.com');
await browser.close();

Explanation:->
- launch() → Browser start karna
- headless: false → Browser visible hoga (true → browser background me run hoga)
- newPage() → Tab open karna
- goto(any_url) → Navigate to URL 
- close() → Browser close karna





# 3. Page Actions (Selectors & Interactions)

* syntax:->

| **Selector Type**    | **Syntax**                   | **Explanation / Use**                                                                                               |
| -------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| ID                   | `#id`                        | Element ko **id attribute** ke basis par select karta hai.<br>Example: `#username` → `<input id="username">`        |
| Class                | `.class`                     | Element ko **class name** ke basis par select karta hai.<br>Example: `.input-field` → `<input class="input-field">` |
| Exact Text           | `text="Login"`               | Page me **exact visible text** match karta hai.<br>Example: `text="Login"` → `<button>Login</button>`               |
| Partial Text         | `text=/Log/i`                | Page me text ka **partial match** karta hai (case-insensitive).<br>Example: `/Log/i` matches "Login" ya "Logout"    |
| XPath                | `xpath=//div[@id="main"]`    | Element ko **HTML structure** ke basis par select karta hai.<br>Powerful, lekin complex.                            |
| Data Attribute       | `[data-testid="login-btn"]`  | **Recommended for stable tests**.<br>Testing-specific attribute → `<button data-testid="login-btn">`                |
| Attribute            | `[name="email"]`             | Kisi bhi **attribute ke value** ke basis par select karta hai.<br>Example: `<input name="email">`                   |
| Role / Accessibility | `role=button[name="Submit"]` | ARIA role aur accessible name ke basis par element locate karta hai.                                                |
| nth-of-type          | `css=div:nth-of-type(2)`     | Ek type ke element me **n-th element** select karta hai.                                                            |
| Combined             | `#login-form .submit-btn`    | Multiple selectors combine karke **precise targeting** karta hai.                                                   |




* Example:->

| **Method**       | **Syntax**                                      | **Meaning / Use**                                   |
| ---------------- | ----------------------------------------------- | --------------------------------------------------- |
| Click            | `await page.click('#login')`                    | Element ko **click** karna                          |
| Fill input       | `await page.fill('#username', 'John')`          | Input field me **text enter** karna                 |
| Type slowly      | `await page.type('#search', 'Playwright')`      | Text **user ke jaise type** karna (slow typing)     |
| Press key        | `await page.press('#search', 'Enter')`          | **Keyboard key press** karna                        |
| Select dropdown  | `await page.selectOption('#country', 'India')`  | Dropdown me **option select** karna                 |
| Check checkbox   | `await page.check('#agree')`                    | Checkbox **select** karna                           |
| Uncheck checkbox | `await page.uncheck('#agree')`                  | Checkbox **deselect** karna                         |
| Hover            | `await page.hover('.menu')`                     | Mouse **hover** karna (hover effect, dropdown open) |
| Screenshot       | `await page.screenshot({ path: 'screen.png' })` | Page ka **screenshot** lena                         |




# 4. Assertions (Test Verification)
syntax:-   test("name of test case",callback_function)

Ex:->
test('Login page title test', async ({ page }) => {
  await page.goto('https://example.com/login');
  await expect(page).toHaveTitle('Login');
});


* Common Assertions
    - expect(page).toHaveTitle('Title') → Page title check
    - expect(locator).toHaveText('Text') → Element text check
    - expect(locator).toBeVisible() → Element visible hai ya nahi
    - expect(locator).toHaveCount(n) → Multiple elements count check


Explanation:->
- { page } → ek naya tab kholta hai.
- .toHaveTitle('Login') → Check karta hai ki page ka title exactly "Login" hai ya nahi.
     -> Agar title match nahi karega → Test fail ho jayega.



# 5. Waiting / Timing
- Playwright auto-wait karta hai for element.
- But, we can add wait time manually.

Ex:-
await page.waitForSelector('#login'); // Wait until element appears
await page.waitForTimeout(2000);      // Wait 2 seconds



# 6. Network Interception / Mocking
await page.route('**/api/**', route => {
  route.fulfill({ status: 200, body: '{"success": true}' });
});



# 7.Multiple Pages
const context = await browser.newContext(); // Isolated session
const page1 = await context.newPage();
const page2 = await context.newPage();

# 8.Keyboard & Mouse Events
await page.keyboard.type('Hello World');
await page.keyboard.press('Enter');
await page.mouse.click(100, 200);
await page.mouse.move(100, 200);

# 9. Cross-browser Testing
const browsers = [chromium, firefox, webkit];

for (const browserType of browsers) {
  const browser = await browserType.launch({ headless: true });
  const page = await browser.newPage();
  await page.goto('https://example.com');
  console.log(await page.title());
  await browser.close();
}

# 10. Screenshots & PDF
await page.screenshot({ path: 'page.png', fullPage: true });
await page.pdf({ path: 'page.pdf', format: 'A4' });



# 11. Advanced Features
i. Page Object Model (POM)
// loginPage.ts
import { Page } from '@playwright/test';

export class LoginPage {
  constructor(private page: Page) {}

  async navigate() {
    await this.page.goto('https://example.com/login');
  }

  async login(username: string, password: string) {
    await this.page.fill('#username', username);
    await this.page.fill('#password', password);
    await this.page.click('#submit');
  }
}

ii. Parallel Testing
npx playwright test --workers=3


iii. Debugging
await page.pause();  // Opens Playwright Inspector


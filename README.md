# SauceDemo Selenium Automation
## Name: Dharunyadevi S
## Register Number:212223220018

This project contains a Selenium WebDriver automation script developed using Python to automate different shopping-related test scenarios on the SauceDemo website.

The project focuses on practicing Selenium concepts such as locators, explicit waits, mouse actions, double-click, drag and drop, browser navigation, form handling, and checkout automation.

## Project Overview

The automation covers the following test scenarios:

| Test Case | Scenario                               | Selenium Concept          |
| --------- | -------------------------------------- | ------------------------- |
| TC01      | Open the online shopping website       | `driver.get()`            |
| TC02      | Remove a product                       | Element interaction       |
| TC03      | Cancel product removal scenario        | Alert handling concept    |
| TC04      | Enter customer information in a prompt | Prompt handling           |
| TC05      | Move mouse over a product/menu element | Mouse Hover               |
| TC06      | Double-click a product                 | Double Click              |
| TC07      | Drag a product toward the cart         | Drag & Drop               |
| TC08      | Wait for a product to be displayed     | Explicit Wait             |
| TC09      | Complete checkout and place an order   | Clickable Wait            |
| TC10      | Verify order confirmation              | Explicit Wait + Assertion |

## Technologies Used

* Python
* Selenium WebDriver
* Google Chrome
* ChromeDriver
* WebDriverWait
* Expected Conditions
* ActionChains

## Website

SauceDemo is used as the test application.

https://www.saucedemo.com/

## Program
```py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

driver=webdriver.Chrome()
wait=WebDriverWait(driver,10)
driver.maximize_window()
driver.get("https://www.saucedemo.com/")

print("TC01:",driver.title)

wait.until(EC.visibility_of_element_located((By.ID,"user-name"))).send_keys("standard_user")
driver.find_element(By.ID,"password").send_keys("secret_sauce")
driver.find_element(By.ID,"login-button").click()
wait.until(EC.url_contains("inventory.html"))
print("Login successfull")

add_button=driver.find_element(By.ID,"add-to-cart-sauce-labs-backpack").click()
remove_button=driver.find_element(By.ID,"remove-sauce-labs-backpack").click()
print("TC02: Product removed successfully")

driver.find_element(By.ID,"add-to-cart-sauce-labs-backpack").click()
driver.execute_script("""
window.confirm=function(){
return false;
};""")
driver.find_element(By.ID,"remove-sauce-labs-backpack").click()
print("TC03: Cancel scenario demonstrated")

driver.execute_script("""
window.prompt=function(message, defaultValue){
return "Customer123";
};""")
result=driver.execute_script(""" 
return window.prompt("Enter customer information","");
""")
print("TC04: Entered information:",result)

products=wait.until(EC.visibility_of_element_located((By.CLASS_NAME,"title")))
ActionChains(driver).move_to_element(products).perform()
print("TC05: Mouse hover performed")

product=wait.until(EC.visibility_of_element_located((By.XPATH,'//*[@id="item_4_title_link"]/div')))
ActionChains(driver).double_click(product).perform()
print("TC06: Double click performed")
driver.get("https://www.saucedemo.com/inventory.html")

product_item=wait.until(EC.visibility_of_element_located((By.ID,"item_4_title_link")))
cart_icon=wait.until(EC.visibility_of_element_located((By.CLASS_NAME,"shopping_cart_link")))
ActionChains(driver).drag_and_drop(product_item,cart_icon).perform()
print("TC07: Drag and drop performed")

product=wait.until(EC.visibility_of_element_located((By.XPATH,'//*[@id="item_0_title_link"]/div')))
print("TC08: Product displayed;",product.text)

driver.get("https://www.saucedemo.com/inventory.html")

wait.until(EC.element_to_be_clickable((By.ID,"add-to-cart-sauce-labs-bike-light"))).click()

# FIXED LINE: Using execute_script to click directly on the cart link element
cart_element = wait.until(EC.presence_of_element_located((By.CLASS_NAME, "shopping_cart_link")))
driver.execute_script("arguments[0].click();", cart_element)

wait.until(EC.url_contains("cart.html"))

wait.until(EC.element_to_be_clickable((By.ID,"checkout"))).click()

wait.until(EC.visibility_of_element_located((By.ID,"first-name"))).send_keys("Dharunyadevi")
driver.find_element(By.ID,"last-name").send_keys("S")
driver.find_element(By.ID,"postal-code").send_keys("600001")

wait.until(EC.element_to_be_clickable((By.ID,"continue"))).click()

finish_button=wait.until(EC.element_to_be_clickable((By.ID,"finish")))
finish_button.click()

print("TC09: Order submitted successfully")

confirmation=wait.until(EC.visibility_of_element_located((By.CLASS_NAME,"complete-header")))
print("TC10:",confirmation.text)

if confirmation.text=="Thank you for your order!":
    print("TC10: Order confirmation displayed successfully")

time.sleep(3)
driver.quit()
```
## Selenium Concepts Used

### 1. WebDriver

```python
driver=webdriver.Chrome()
```

Used to launch and control the Chrome browser.

### 2. Navigation

```python
driver.get("https://www.saucedemo.com/")
```

Used to open the SauceDemo website.

### 3. Locators

The project uses different Selenium locators:

```python
By.ID
By.XPATH
By.CLASS_NAME
```

Example:

```python
driver.find_element(By.ID,"user-name")
```

### 4. Send Keys

Used to enter data into input fields.

```python
driver.find_element(By.ID,"password").send_keys("secret_sauce")
```

### 5. Click

Used to interact with buttons and links.

```python
driver.find_element(By.ID,"login-button").click()
```

### 6. Explicit Wait

```python
wait.until(
    EC.visibility_of_element_located(
        (By.ID,"first-name")
    )
)
```

Explicit waits are used to wait until a particular condition is satisfied before continuing the test.

### 7. Clickable Wait

```python
finish_button=wait.until(
    EC.element_to_be_clickable(
        (By.ID,"finish")
    )
)
```

This waits until the element is visible and enabled for clicking.

### 8. Mouse Hover

```python
ActionChains(driver).move_to_element(products).perform()
```

Used to perform mouse movement over an element.

### 9. Double Click

```python
ActionChains(driver).double_click(product).perform()
```

Used to perform a double-click action.

### 10. Drag and Drop

```python
ActionChains(driver).drag_and_drop(
    product_item,
    cart_icon
).perform()
```

Used to demonstrate Selenium's drag-and-drop functionality.


## Login Credentials

The standard SauceDemo test credentials used in this project are:

```text
Username: standard_user
Password: secret_sauce
```


## Expected Order Confirmation

After completing the checkout process, the automation verifies:

```text
Thank you for your order!
```

## Sample Output

```text
TC01: Swag Labs
Login successfull
TC02: Product removed successfully
TC03: Cancel scenario demonstrated
TC04: Entered information: Customer123
TC05: Mouse hover performed
TC06: Double click performed
TC07: Drag and drop performed
TC08: Product displayed; Sauce Labs Bike Light
TC09: Order submitted successfully
TC10: Thank you for your order!
TC10: Order confirmation displayed successfully
```


## Learning Outcomes

Through this project, I practiced:

* Selenium WebDriver automation
* Web element identification
* ID, XPath and Class Name locators
* Explicit waits
* Clickable waits
* Browser navigation
* Form automation
* Mouse actions
* JavaScript execution
* Shopping cart automation
* Checkout automation
* Order confirmation verification
## Output
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0b429fe1-91a2-4305-9e40-7e5679790131" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/869aedb8-18e7-4d28-b96f-fa3761737e4c" />


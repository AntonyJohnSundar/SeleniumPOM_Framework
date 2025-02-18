# SeleniumPOM_Framework
- Selenium POM Framework - Reusable Structure

# Selenium POM Framework Setup Guide

## 📌 Introduction
This guide will walk you through setting up a **Selenium Page Object Model (POM) framework** integrated with **TestNG** and **Extent Reports**. The framework is **reusable, modular, and configurable**, ensuring maintainability and scalability.

---

## 📂 Project Structure
```
SeleniumPOMFramework/
│-- src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── com/framework/base/BaseTest.java
│   │   │   ├── com/framework/pages/AmazonLoginPage.java
│   │   │   ├── com/framework/pages/AmazonSearchPage.java
│   │   │   ├── com/framework/utils/ConfigReader.java
│   │   │   ├── com/framework/utils/ReportManager.java
│   │   ├── resources/
│   │   │   ├── config.properties
│   │   │   ├── log4j.properties
│   ├── test/
│   │   ├── java/
│   │   │   ├── com/framework/tests/AmazonLoginTest.java
│   │   │   ├── com/framework/tests/AmazonSearchTest.java
│-- pom.xml
│-- testng.xml
```

---

## 🚀 Setup Instructions
### 1️⃣ Prerequisites
Ensure you have the following installed:
- **Java JDK (11 or later)**
- **Maven**
- **Selenium WebDriver**
- **TestNG Plugin** (for Eclipse/IDE)
- **Extent Reports** (for reporting)

---

### 2️⃣ Clone the Repository
```sh
 git clone https://github.com/YourRepo/SeleniumPOMFramework.git
 cd SeleniumPOMFramework
```

---

### 3️⃣ Configure the Framework
Update **`src/main/resources/config.properties`** with your preferred browser settings:
```
browser=chrome
baseUrl=https://www.amazon.com
```

---

### 4️⃣ Base Test Setup
```java
package com.framework.base;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import java.time.Duration;
import java.util.Properties;
import com.framework.utils.ConfigReader;
import com.framework.utils.ReportManager;

public class BaseTest {
    protected static WebDriver driver;
    static Properties properties = ConfigReader.getProperties();

    @BeforeClass
    public static void initializeDriver() {
        String browser = properties.getProperty("browser");
        if (browser.equalsIgnoreCase("chrome")) {
            driver = new ChromeDriver();
        } else if (browser.equalsIgnoreCase("firefox")) {
            driver = new FirefoxDriver();
        } else {
            throw new IllegalArgumentException("Unsupported browser: " + browser);
        }
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        driver.manage().window().maximize();
        ReportManager.startReport();
    }

    @AfterClass
    public static void quitDriver() {
        if (driver != null) {
            driver.quit();
        }
        ReportManager.endReport();
    }
}
```

---

### 5️⃣ Implementing Page Objects
#### **Amazon Login Page**
```java
package com.framework.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class AmazonLoginPage {
    WebDriver driver;
    
    By emailField = By.id("ap_email");
    By continueButton = By.id("continue");
    By passwordField = By.id("ap_password");
    By signInButton = By.id("signInSubmit");
    
    public AmazonLoginPage(WebDriver driver) {
        this.driver = driver;
    }
    
    public void login(String email, String password) {
        driver.findElement(emailField).sendKeys(email);
        driver.findElement(continueButton).click();
        driver.findElement(passwordField).sendKeys(password);
        driver.findElement(signInButton).click();
    }
}
```

#### **Amazon Search Page**
```java
package com.framework.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class AmazonSearchPage {
    WebDriver driver;
    
    By searchBox = By.id("twotabsearchtextbox");
    By searchButton = By.id("nav-search-submit-button");
    
    public AmazonSearchPage(WebDriver driver) {
        this.driver = driver;
    }
    
    public void search(String keyword) {
        driver.findElement(searchBox).sendKeys(keyword);
        driver.findElement(searchButton).click();
    }
}
```

---

### 6️⃣ Writing Test Cases
#### **Amazon Login Test**
```java
package com.framework.tests;

import org.testng.annotations.Test;
import com.framework.base.BaseTest;
import com.framework.pages.AmazonLoginPage;

public class AmazonLoginTest extends BaseTest {
    @Test
    public void testAmazonLogin() {
        driver.get("https://www.amazon.com");
        AmazonLoginPage loginPage = new AmazonLoginPage(driver);
        loginPage.login("testuser@example.com", "password123");
    }
}
```

#### **Amazon Search Test**
```java
package com.framework.tests;

import org.testng.annotations.Test;
import com.framework.base.BaseTest;
import com.framework.pages.AmazonSearchPage;

public class AmazonSearchTest extends BaseTest {
    @Test
    public void testAmazonSearch() {
        driver.get("https://www.amazon.com");
        AmazonSearchPage searchPage = new AmazonSearchPage(driver);
        searchPage.search("Laptop");
    }
}
```

---

### 7️⃣ Running Tests with TestNG
Execute the tests using:
```sh
mvn test
```
OR run the **`testng.xml`** file:
```xml
<suite name="Test Suite">
    <test name="Amazon Tests">
        <classes>
            <class name="com.framework.tests.AmazonLoginTest"/>
            <class name="com.framework.tests.AmazonSearchTest"/>
        </classes>
    </test>
</suite>
```

---

## 🎯 Conclusion
Congratulations! 🎉 You have successfully set up the **Selenium POM Framework** with TestNG and reporting tools. You can now expand this framework with additional test cases and functionalities. 🚀

📫 Feel free to **contribute, improve, and customize** this framework for your needs!


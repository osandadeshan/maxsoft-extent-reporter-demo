# MaxSoft Extent Reporter

## Introduction
The main reason for developing this plugin is to provide an easy way to generate the Extent report for test automation.  
![](https://github.com/osandadeshan/maxsoft-extent-reporter-demo/blob/master/extent-report-sample.PNG)

## Advantages
- Automatically generates the Extent report after the test execution.
- Reporter details can be configured through a property file.
- No need to implement classes for Extent reporting.
- Easy to use.

## Technologies/Frameworks Used
- Java
- Extent Report
- Selenium
- Apache Maven

## Supported Platforms
- Windows
- Linux
- Mac OS

## Supported Languages
- Java

## How to use
**Pre-Requisites:**
1. Java
2. Maven

**Steps:**
1. Add "**MaxSoft Extent Reporter**" dependency into "**pom.xml**".
```xml
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
	
    <dependencies>
        <dependency>
            <groupId>com.github.osandadeshan</groupId>
            <artifactId>maxsoft-extent-reporter</artifactId>
            <version>1.1.0</version>
        </dependency>
    </dependencies>
```

2. Create "**extent.properties**" in "***src/test/resources/extent.properties***".
```xml
# Extent Report Configs
extent_reporter_theme=dark
capture_screenshot_on_failure=true
extent_document_title=Test Execution Report
extent_reporter_name=Test Execution Report
application_name=AutomationPractice.com
environment=Production
browser=Chrome
operating_system=Windows 10 - 64 Bit
test_developer=Osanda Nimalarathna
```

3. In the test automation code, find the place you are launching the WebDriver.

4. Pass your WebDriver object to the "**setDriver()**" method which can be imported from "***com.maxsoft.extentreport.DriverHolder.setDriver***".
```java
WebDriver driver = new ChromeDriver();
setDriver(driver);
```

5.  Update the places where your are using WebDriver object, into "**getDriver()**" method which can be imported from "***com.maxsoft.extentreport.DriverHolder.getDriver***".
```java
getDriver().manage().window().maximize();
```

6. An example test class.
```java
package test;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

import static com.maxsoft.extentreport.DriverHolder.getDriver;
import static com.maxsoft.extentreport.DriverHolder.setDriver;
import static com.maxsoft.extentreport.PropertyFileReader.getProperty;
import static org.testng.Assert.assertEquals;

/**
 * Project Name    : maxsoft-extent-reporter-demo
 * Developer       : Osanda Deshan
 * Version         : 1.0.0
 * Date            : 11/21/2020
 * Time            : 1:08 PM
 * Description     : This is a test class to test the login functionality
 **/

public class LoginTest {

    private WebElement emailTextBox;
    private WebElement passwordTextBox;
    private WebElement signInButton;

    @BeforeMethod
    public void before() {
        WebDriverManager.chromedriver().setup();
        WebDriver driver = new ChromeDriver();
        setDriver(driver);
        getDriver().manage().window().maximize();
        getDriver().get(getProperty("application_url"));
        emailTextBox = getDriver().findElement(By.id("email"));
        passwordTextBox = getDriver().findElement(By.id("passwd"));
        signInButton = getDriver().findElement(By.id("SubmitLogin"));
    }

    @Test(description = "Verify that a valid user can login to the application")
    public void testValidLogin() {
        emailTextBox.sendKeys("osanda@mailinator.com");
        passwordTextBox.sendKeys("1qaz2wsx@");
        signInButton.click();
        assertEquals(getDriver().findElement(By.xpath("//div[@class='header_user_info']//span")).getText(), "Osanda Nimalarathna");
    }

    @Test(description = "Verify that an invalid user cannot login to the application")
    public void testInvalidLogin() {
        emailTextBox.sendKeys("osanda@mailinator.com");
        passwordTextBox.sendKeys("1qaz2wsx@");
        signInButton.click();
        assertEquals(getDriver().getTitle(), "Login - My Store");
    }

    @AfterMethod
    public void after() {
        getDriver().quit();
    }
}

```

7. Create the "**TestNG.xml**" by adding the "**MaxSoft Extent Reporter listener**" class.
```xml
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="Regression Test Suite">
    <listeners>
        <listener class-name="com.maxsoft.extentreport.ExtentReportListener"/>
    </listeners>
    <test name="Regression Test">
        <classes>
            <class name="test.LoginTest" />
        </classes>
    </test>
</suite>
```

8. Run the "**TestNG.xml**".

## License
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/License_icon-mit-2.svg/2000px-License_icon-mit-2.svg.png" alt="MIT License" width="100" height="100"/> [MaxSoft Extent Reporter](https://medium.com/maxsoft-extent-reporter) is released under [MIT License](https://opensource.org/licenses/MIT)

## Copyright
Copyright 2020 MaxSoft.

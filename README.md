# Step 1: Create a new Maven project

1. Open IntelliJ IDEA.
2. Click on `File` > `New` > `Project...`.
3. Select `Maven` from the options on the left.
4. Check `Create from archetype`.
5. Select `org.apache.maven.archetypes:maven-archetype-quickstart`.
6. Click `Next`.
7. Enter `demonopcommerce` as the `GroupId` and `ArtifactId`.
8. Click `Next` and then `Finish` to create the project.

### Step 2: Ensure Cucumber for Java plugin is installed

1. Go to `File` > `Settings` > `Plugins`.
2. Under the `Marketplace` tab, search for `Cucumber for Java`.
3. Install the plugin if it's not already installed.

### Step 3: Add essential dependencies to `pom.xml`

Add the following dependencies for Selenium, Cucumber Java, and Cucumber TestNG to your `pom.xml`:

```xml
<dependencies>
    <!-- Selenium WebDriver -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.1.0</version> <!-- Replace with the latest version -->
    </dependency>

    <!-- Cucumber Dependencies -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-java</artifactId>
        <version>7.3.1</version> <!-- Replace with the latest version -->
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-testng</artifactId>
        <version>7.3.1</version> <!-- Replace with the latest version -->
        <scope>test</scope>
    </dependency>
</dependencies>
```

### Step 4: Create your first feature file and run it

1. Create a `features` directory under `src/main/resources`.
2. Inside `features`, create a feature file named `F01_Register.feature` with the following content:

```gherkin
@smoke
Feature: F01_Register | users could register with new accounts

  Scenario: guest user could register with valid data successfully
    Given user go to register page
```

3. Right-click on `F01_Register.feature` and run it as a Cucumber test. Ensure it runs successfully.

### Step 5: Set up directory structure for test execution and reports

1. Create the following directory structure under `src/test/java/org/example`:
   - `stepDefs`
   - `pages`
   - `testRunner`

### Step 6: Create relative Step Definition class

1. Under `src/test/java/org/example/stepDefs`, create a Java class named `D01_registerStepDef`.
2. Inside `D01_registerStepDef`, create a method for the Given step as follows:

```java
package org.example.stepDefs;

import io.cucumber.java.en.Given;

public class D01_registerStepDef {
    @Given("user go to register page")
    public void goRegisterPage() {
        System.out.println("This is a test before start coding");
    }
}
```

### Step 7: Implement Hooks for setup and teardown

1. Under `src/test/java/org/example/stepDefs`, create a Java class named `Hooks`.
2. Implement the Hooks class with setup (`@Before`) and teardown (`@After`) methods using WebDriver:

```java
package org.example.stepDefs;

import io.cucumber.java.After;
import io.cucumber.java.Before;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

import java.util.concurrent.TimeUnit;

public class Hooks {
    public static WebDriver driver;

    @Before
    public void openBrowser() {
        // Set up WebDriver configuration
        System.setProperty("webdriver.chrome.driver", "C:\\Path\\To\\chromedriver.exe");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
        driver.get("https://demo.nopcommerce.com/");
    }

    @After
    public void closeBrowser() throws InterruptedException {
        Thread.sleep(3000);
        driver.quit();
    }
}
```

### Conclusion

Make sure to adjust paths and versions (like `webdriver.chrome.driver` path) according to your local setup. This setup should provide you with a foundational structure to start implementing your automation tests using Cucumber with Java and Selenium WebDriver. Adjustments might be needed based on specific project requirements or updates in libraries.



To set up a Maven project for automating scenarios on the nopCommerce demo site using Cucumber framework, follow these steps:

### Step 1: Create Maven Project
1. **Create New Maven Project**: Open IntelliJ IDEA, select `File` -> `New` -> `Project...`.
   - Choose `Maven` on the left panel.
   - Click `Next`.
   - Enter `Group ID`, `Artifact ID`, and `Version`.
   - Click `Next` and then `Finish` to create the project.

### Step 2: Install Cucumber Plugin
2. **Install Cucumber Plugin**: Go to `File` -> `Settings` -> `Plugins`.
   - Click on `Marketplace` tab.
   - Search for `Cucumber for Java` and install it if not already installed.

### Step 3: Add Dependencies
3. **Add Dependencies**: Edit `pom.xml` to include necessary dependencies.
   - **Selenium**: Add Selenium Java dependency.
     ```xml
     <dependency>
         <groupId>org.seleniumhq.selenium</groupId>
         <artifactId>selenium-java</artifactId>
         <version>4.1.0</version> <!-- Check for the latest version -->
     </dependency>
     ```
   - **Cucumber**: Add Cucumber Java dependency.
     ```xml
     <dependency>
         <groupId>io.cucumber</groupId>
         <artifactId>cucumber-java</artifactId>
         <version>7.3.0</version> <!-- Check for the latest version -->
     </dependency>
     ```
   - **Cucumber TestNG**: Add Cucumber TestNG dependency (optional if using TestNG).
     ```xml
     <dependency>
         <groupId>io.cucumber</groupId>
         <artifactId>cucumber-testng</artifactId>
         <version>7.3.0</version> <!-- Check for the latest version -->
         <scope>test</scope>
     </dependency>
     ```

### Step 4: Create Feature File
4. **Create Feature File**: Under `src/main/resources`, create a directory `features`.
   - Inside `features`, create a feature file named `F01_Register.feature`.
   - Example content for `F01_Register.feature`:
     ```gherkin
     @smoke
     Feature: F01_Register | Users could register with new accounts

       Scenario: Guest user could register with valid data successfully
         Given user go to register page
     ```

### Step 5: Create Package Structure
5. **Create Package Structure**: Under `src/test/java`, create packages as follows:
   - `org.example.stepDefs`
   - `org.example.pages`
   - `org.example.testRunner`

   Ensure the structure is:
   ```
   java
   └── org
       └── example
           ├── pages
           ├── stepDefs
           └── testRunner
   ```

### Step 6: Create Step Definition
6. **Create Step Definition Class**: Under `src/test/java/org/example/stepDefs`, create a class `D01_registerStepDef`.
   - Example content for `D01_registerStepDef.java`:
     ```java
     package org.example.stepDefs;

     import io.cucumber.java.en.Given;
     import org.openqa.selenium.By;
     import org.openqa.selenium.WebElement;
     import org.example.pages.P01_register;
     import org.example.stepDefs.Hooks;

     public class D01_registerStepDef {
         P01_register register = new P01_register();

         @Given("user go to register page")
         public void goRegisterPage() {
             register.registerlink().click();
         }
     }
     ```

### Step 7: Create Page Object Model (POM)
7. **Create Page Object Model (POM)**: Under `src/test/java/org/example/pages`, create a class `P01_register`.
   - Example content for `P01_register.java`:
     ```java
     package org.example.pages;

     import org.openqa.selenium.By;
     import org.openqa.selenium.WebElement;
     import org.example.stepDefs.Hooks;

     public class P01_register {
         public WebElement registerlink() {
             return Hooks.driver.findElement(By.className("ico-register"));
         }
     }
     ```

### Step 8: Create Hooks
8. **Create Hooks**: Under `src/test/java/org/example/stepDefs`, create a class `Hooks`.
   - Example content for `Hooks.java`:
     ```java
     package org.example.stepDefs;

     import io.cucumber.java.After;
     import io.cucumber.java.Before;
     import org.openqa.selenium.WebDriver;
     import org.openqa.selenium.chrome.ChromeDriver;
     import java.util.concurrent.TimeUnit;

     public class Hooks {
         public static WebDriver driver;

         @Before
         public static void OpenBrowser() {
             System.setProperty("webdriver.chrome.driver", "C:\\Program Files\\chromedriver.exe");
             driver = new ChromeDriver();
             driver.manage().window().maximize();
             driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
             driver.get("https://demo.nopcommerce.com/");
         }

         @After
         public static void quitDriver() throws InterruptedException {
             Thread.sleep(3000);
             driver.quit();
         }
     }
     ```

### Step 9: Create Test Runner
9. **Create Test Runner**: Under `src/test/java/org/example/testRunner`, create a class `Runners`.
   - Example content for `Runners.java`:
     ```java
     package org.example.testRunner;

     import io.cucumber.testng.AbstractTestNGCucumberTests;
     import io.cucumber.testng.CucumberOptions;

     @CucumberOptions(
         features = "src/main/resources/features",
         glue = {"org.example.stepDefs"},
         plugin = {
             "pretty",
             "html:target/cucumber.html",
             "json:target/cucumber.json",
             "junit:target/cukes.xml",
             "rerun:target/rerun.txt"
         },
         tags = "@smoke"
     )
     public class Runners extends AbstractTestNGCucumberTests {
     }
     ```

### Step 10: Run Your Tests and Generate Reports
10. **Run Tests and Generate Reports**:
    - Execute Maven lifecycle `clean verify` to run tests and generate reports.
    - View the HTML report at `target/cucumber-html-reports/overview-features.html`.

This setup provides a structured framework using Cucumber for automated testing of the nopCommerce demo site, integrating Selenium for web interactions and generating comprehensive test reports using Maven plugins. Adjust versions and configurations as needed based on your project requirements and updates in dependencies.


### Feature File: F01_Register.feature

```gherkin
@smoke
Feature: F01_Register | users could register with new accounts

  Scenario: guest user could register with valid data successfully
    Given user go to register page
    When user select gender type
    And user enter first name "automation" and last name "tester"
    And user enter date of birth
    And user enter email "test@example.com" field
    And user fills Password fields "P@ssw0rd" "P@ssw0rd"
    And user clicks on register button
    Then success message is displayed
```

### Step Definition Class: D01_registerStepDef.java

```java
package org.example.stepDefs;

import io.cucumber.java.en.Given;
import io.cucumber.java.en.When;
import io.cucumber.java.en.Then;
import org.example.pages.P01_register;

public class D01_registerStepDef {

    P01_register register = new P01_register();

    @Given("user go to register page")
    public void go_to_register() {
        register.navigateToRegisterPage();
    }

    @When("user select gender type")
    public void select_gender_type() {
        register.selectGender();
    }

    @When("user enter first name {string} and last name {string}")
    public void enter_first_and_last_name(String firstName, String lastName) {
        register.enterFirstName(firstName);
        register.enterLastName(lastName);
    }

    @When("user enter date of birth")
    public void enter_date_of_birth() {
        register.enterDateOfBirth();
    }

    @When("user enter email {string} field")
    public void enter_email(String email) {
        register.enterEmail(email);
    }

    @When("user fills Password fields {string} {string}")
    public void enter_passwords(String password1, String password2) {
        register.enterPassword(password1, password2);
    }

    @When("user clicks on register button")
    public void click_register_button() {
        register.clickRegisterButton();
    }

    @Then("success message is displayed")
    public void verify_success_message() {
        register.verifyRegistrationSuccess();
    }
}
```

### Page Object Class: P01_register.java

```java
package org.example.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.Select;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.support.Color;
import org.openqa.selenium.support.ui.ExpectedConditions;
import static org.testng.Assert.assertEquals;
import org.testng.asserts.SoftAssert;
import org.example.utilities.Driver;

import java.util.List;

public class P01_register {

    WebDriverWait wait = new WebDriverWait(Driver.get(), 10);
    SoftAssert softAssert = new SoftAssert();

    public void navigateToRegisterPage() {
        Driver.get().get("https://demo.nopcommerce.com/register");
    }

    public void selectGender() {
        WebElement gender = Driver.get().findElement(By.id("gender-male")); // Example locator, adjust as per your actual page
        gender.click();
    }

    public void enterFirstName(String firstName) {
        WebElement firstNameField = Driver.get().findElement(By.id("FirstName")); // Example locator
        firstNameField.sendKeys(firstName);
    }

    public void enterLastName(String lastName) {
        WebElement lastNameField = Driver.get().findElement(By.id("LastName")); // Example locator
        lastNameField.sendKeys(lastName);
    }

    public void enterDateOfBirth() {
        // Implement logic to enter date of birth
    }

    public void enterEmail(String email) {
        WebElement emailField = Driver.get().findElement(By.id("Email")); // Example locator
        emailField.sendKeys(email);
    }

    public void enterPassword(String password1, String password2) {
        WebElement passwordField = Driver.get().findElement(By.id("Password")); // Example locator
        WebElement confirmPasswordField = Driver.get().findElement(By.id("ConfirmPassword")); // Example locator
        passwordField.sendKeys(password1);
        confirmPasswordField.sendKeys(password2);
    }

    public void clickRegisterButton() {
        WebElement registerButton = Driver.get().findElement(By.id("register-button")); // Example locator
        registerButton.click();
    }

    public void verifyRegistrationSuccess() {
        WebElement successMessage = wait.until(ExpectedConditions.visibilityOfElementLocated(By.className("result"))); // Example locator
        softAssert.assertTrue(successMessage.isDisplayed(), "Success message is displayed");

        // Verify success message color
        String color = successMessage.getCssValue("color");
        String hexColor = Color.fromString(color).asHex();
        softAssert.assertEquals(hexColor, "#4cb17c", "Success message color is green");

        softAssert.assertAll();
    }
}




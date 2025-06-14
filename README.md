# SeleniumCode:explicitWait, broken links, click button, fill in field


# ExplicitWaitExample

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class ExplicitWaitExample {
    public static void main(String[] args) {
        // Set up the ChromeDriver (ensure chromedriver path is correct)
        System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
        WebDriver driver = new ChromeDriver();

        try {
            driver.get("https://example.com");

            // Create WebDriverWait object with 10 seconds timeout
            WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

            // Wait until the element is visible
            WebElement element = wait.until(
                ExpectedConditions.visibilityOfElementLocated(By.id("someElementId"))
            );

            // Perform action on the element
            element.click();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            driver.quit();
        }
    }
}


/////////////////////////////////////////////////////////////////////////////////////////////
# find all broken links

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

import java.net.HttpURLConnection;
import java.net.URL;
import java.util.List;

public class BrokenLinkChecker {
    public static void main(String[] args) {
        // Set path to your ChromeDriver
        System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
        WebDriver driver = new ChromeDriver();

        try {
            // Load the target page
            driver.get("https://example.com");

            // Get all <a> tags (hyperlinks)
            List<WebElement> links = driver.findElements(By.tagName("a"));

            System.out.println("Total links found: " + links.size());

            for (WebElement link : links) {
                String url = link.getAttribute("href");

                // Skip null or empty hrefs
                if (url == null || url.isEmpty()) {
                    continue;
                }

                // Check if URL is valid
                try {
                    HttpURLConnection connection = (HttpURLConnection) (new URL(url).openConnection());
                    connection.setRequestMethod("HEAD");
                    connection.connect();

                    int responseCode = connection.getResponseCode();
                    if (responseCode >= 400) {
                        System.out.println("🔴 Broken link: " + url + " → " + responseCode);
                    } else {
                        System.out.println("✅ Valid link: " + url + " → " + responseCode);
                    }
                } catch (Exception e) {
                    System.out.println("⚠️ Error checking link: " + url + " → " + e.getMessage());
                }
            }

        } finally {
            driver.quit();
        }
    }
}


////////////////////////////////////////////////////////////////////////
# click button

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class ClickButtonExample {
    public static void main(String[] args) {
        // Set up ChromeDriver
        System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
        WebDriver driver = new ChromeDriver();

        try {
            driver.get("https://example.com");

            // Locate the button by its ID and click it
            WebElement button = driver.findElement(By.id("submitBtn"));
            button.click();

        } finally {
            driver.quit();
        }
    }
}

//////////////////////////////////////////////////////////////////////////////////

# fill in field

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class FillFieldExample {
    public static void main(String[] args) {
        System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
        WebDriver driver = new ChromeDriver();

        try {
            driver.get("https://example.com");

            // Locate the text input field and fill it in
            WebElement inputField = driver.findElement(By.id("username"));
            inputField.sendKeys("myUsername");

        } finally {
            driver.quit();
        }
    }
}

https://github.com/Dharshini/mini- testing-project-
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

import java.io.FileWriter;
import java.io.IOException;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class LinkedInScraper {

    public static void main(String[] args) {

        //  Set your ChromeDriver path here
        System.setProperty("webdriver.chrome.driver", "drivers/chromedriver.exe");

        //  Create a new Chrome browser instance
        WebDriver driver = new ChromeDriver();

        try {
            //  Implicit wait
            driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

            //  Step 1: Open LinkedIn login page
            driver.get("https://www.linkedin.com/login");

            //  Step 2: Enter login credentials
            driver.findElement(By.id("username")).sendKeys("sirishamudumanchi12@gmail.com");               
             driver.findElement(By.id("password")).sendKeys("Siri@8919735982");         
            driver.findElement(By.xpath("//button[@type='submit']")).click();

            // Wait for login to complete
            Thread.sleep(4000);

            //  Step 3: Go to search results for "hospitals"
            driver.get("https://www.linkedin.com/search/results/people/?keywords=hospitals");

            //  Wait for results to load
            Thread.sleep(4000);

            //  Step 4: Get profile names from search result cards
            List<WebElement> nameElements = driver.findElements(By.cssSelector("span.entity-result__title-text > a > span[aria-hidden='true']"));

            // Step 5: Write names to a CSV file
            FileWriter writer = new FileWriter("output.csv");
            writer.append("Name\n");

            for (WebElement nameElement : nameElements) {
                String name = nameElement.getText().trim();
                System.out.println("Found: " + name);
                writer.append(name).append("\n");
            }

            writer.flush();
            writer.close();

            System.out.println("✅ Scraping completed. Data saved to output.csv");

        } catch (InterruptedException | IOException e) {
            e.printStackTrace();
        } finally {
            // Close the browser
            driver.quit();
        }
    }
}

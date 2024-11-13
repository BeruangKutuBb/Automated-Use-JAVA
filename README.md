# Automated-Use-JAVA
import java.lang.reflect.Array;
import java.time.Duration;
import java.util.Arrays;
import java.util.List;
import java.util.concurrent.TimeUnit;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class Base {

	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub

		
		WebDriver driver = new ChromeDriver();
		driver.manage().window().maximize();

		//dalam kasus ini kita menggunakan implicit wait untuk meranung code ini.  
		//driver.manage().timeouts().implicitlyWait(5, TimeUnit.SECONDS); //maximum 5 detik
		
		//Explicit Wait
		WebDriverWait exWait =new WebDriverWait(driver,Duration.ofSeconds(5));
		
		
		String[] itemsNeeded = { "Cucumber", "Brocolli", "Beetroot", "Carrot" }; // jika ada produk baru tinggal
																					// tambahkan di sini

		driver.get("https://rahulshettyacademy.com/seleniumPractise/#/");
		Thread.sleep(3000);
		addItems(driver, itemsNeeded);
		driver.findElement(By.xpath("//img[@alt='Cart']")).click();
		driver.findElement(By.xpath("//button[contains(text(),'PROCEED TO CHECKOUT')]")).click();
		
		//ecplicit wait
		exWait.until(ExpectedConditions.visibilityOfElementLocated(By.cssSelector("input.promoCode")));
		driver.findElement(By.cssSelector("input.promoCode")).sendKeys("rahulshettyacademy");
		
		driver.findElement(By.cssSelector("button.promoBtn")).click();
		
		//ecplicit wait
		exWait.until(ExpectedConditions.visibilityOfElementLocated(By.cssSelector("span.promoInfo")));
		System.out.println(driver.findElement(By.cssSelector("span.promoInfo")).getText());
		
		

	}

	public static void addItems(WebDriver driver, String[] itemsNeeded) {
		int j = 0;
		List<WebElement> products = driver.findElements(By.cssSelector("h4.product-name"));

		for (int i = 0; i < products.size(); i++) {
			// Brocolli - 1 Kg
			// Brocolli, 1 Kg

			String[] name = products.get(i).getText().split("-"); // ketiak di split menjadi 2 value dan menjadi array
			String formattedName = name[0].trim();
			// format it to get actual vegetable name
			// convert array to array list
			// check whether name you extracted is present in ArrayList or not

			List itemsNeededList = Arrays.asList(itemsNeeded);

			if (itemsNeededList.contains(formattedName)) {
				j++;
				// click on to add chart
				// driver.findElements(By.xpath("//button[text()='ADD TO
				// CART']")).get(i).click(); //element tidak cocok untuk dynamic button
				driver.findElements(By.xpath("//div[@class='product-action']/button ")).get(i).click(); // untuk dynamic
																										// button

				// 3times
				// if (j == 3) { //bisa langsung pake total value
				if (j == itemsNeeded.length) { // untuk array list pake .size sedangkan array pakai .length
					break;
				}

			}
		}
	}

}

//}

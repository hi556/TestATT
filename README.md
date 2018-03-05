# TestATT
Test one
import java.util.concurrent.TimeUnit;

import org.junit.*;

import static org.junit.Assert.*;

import org.openqa.selenium.*;
import org.openqa.selenium.firefox.FirefoxDriver;

public class Login {
	private WebDriver driver;
	private String baseUrl;

	@Before
	public void setUp() throws Exception {
		System.setProperty("webdriver.gecko.driver", "geckodriver");
		driver = new FirefoxDriver();
//		baseUrl = "http://hrm.seleniumminutes.com";
		baseUrl = "http://hrm-online.portnov.com";
		driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
		
		driver.get(baseUrl);
	}

	@After
	public void tearDown() throws Exception {
		driver.quit();	}

	@Test
	public void testValidLogin() {
		driver.findElement(By.id("txtUsername")).sendKeys("admin");
		driver.findElement(By.id("txtPassword")).sendKeys("password");
		driver.findElement(By.id("btnLogin")).click();
		
		String actualGreeting = driver.findElement(By.id("welcome")).getText();
		
		assertEquals("Welcome Admin", actualGreeting);
	}
	
	@Test
	public void testInvalidUsername() {
		String expected = "Invalid credentials";
		String invalid = "abcdef";
		
		driver.findElement(By.id("txtUsername")).sendKeys(invalid);
		driver.findElement(By.id("txtPassword")).sendKeys("Password");
		driver.findElement(By.id("btnLogin")).click();
		
		String actualMessage = driver.findElement(By.id("spanMessage")).getText();
		
		String message = "Expected to see '" + expected + "' but got '" + actualMessage + "' instead.";
		
		assertEquals(message, expected, actualMessage);
	}
	
	@Test
	public void testInvalidPassword() {
		String expected = "Invalid credentials";
		String invalid = "abcdef";
		
		driver.findElement(By.id("txtUsername")).sendKeys("admin");
		driver.findElement(By.id("txtPassword")).sendKeys(invalid);
		driver.findElement(By.id("btnLogin")).click();
		
		String actualMessage = driver.findElement(By.id("spanMessage")).getText();
		
		String message = "Expected to see '" + expected + "' but got '" + actualMessage + "' instead.";
		
		assertEquals(message, expected, actualMessage);
	}
	
	@Test
	public void testEmptyUsername() {
		String expected = "Username cannot be empty";
		
		driver.findElement(By.id("txtPassword")).sendKeys("Password");
		driver.findElement(By.id("btnLogin")).click();
		
		sleep(1);
		String actualMessage = driver.findElement(By.id("spanMessage")).getText();
		
		String message = "Expected to see '" + expected + "' but got '" + actualMessage + "' instead.";
		
		assertEquals(message, expected, actualMessage);
	}
	
	@Test
	public void testEmptyPassword() {
String expected = "Password cannot be empty";
		
		driver.findElement(By.id("txtUsername")).sendKeys("admin");
		driver.findElement(By.id("btnLogin")).click();
		
		String actualMessage = driver.findElement(By.id("spanMessage")).getText();
		
		String message = "Expected to see '" + expected + "' but got '" + actualMessage + "' instead.";
		
		assertEquals(message, expected, actualMessage);
	}
	
	private void sleep(int seconds) {
		try {
			Thread.sleep(seconds * 1000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}

# Dropbox
[15.07.2016 18:54:50] Robert: import org.openqa.selenium.By;
import org.openqa.selenium.NoSuchElementException;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.*;

/**
 * Created by MBCNEWMAIN on 12.07.2016.
 */
public class Dropbox_tests implements Dropbox_variables {

    private static WebDriver driver;

    public static boolean checkElementPresenseCss(String selector) {

        try {
                driver.findElement(By.cssSelector(selector));
                return true;

        } catch (NoSuchElementException e) {
            return false;
        }
    }

    public static boolean checkElementPresenseXpath(String selector) {

        try {
            driver.findElement(By.xpath(selector));
            return true;

        } catch (NoSuchElementException e) {
            return false;
        }
    }

    public static void sleep(int time) {
        try {
            Thread.sleep(time * 1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @BeforeMethod
    public static void setUp() {

        String STRING_KEY = "webdriver.chrome.driver";
        String STRING_PATH = ("bin/chromedriver.exe");
        System.setProperty(STRING_KEY, STRING_PATH);
        driver = new ChromeDriver();

    }

    @AfterMethod
    public static void tearDown() {
        driver.quit();
    }

    public static void successfulLogin() {

        driver.get(LOGIN_PAGE);

        sleep(1);

        WebElement login = driver.findElement(By.cssSelector(LOGIN_INPUT));
        WebElement password = driver.findElement(By.cssSelector(PASSWORD_INPUT));
        login.sendKeys(USERNAME);
        password.sendKeys(PASS);

        driver.findElement(By.cssSelector(LOGIN_BUTTON)).click();

        sleep(5);

        boolean IS_LOGGED_IN = checkElementPresenseCss(".avatar-wrapper");

        Assert.assertTrue(IS_LOGGED_IN);

        sleep(2);

    }

    public static void viewFile() {
        driver.findElement(By.cssSelector(FILE)).click();
        sleep(4);
        Assert.assertTrue(checkElementPresenseCss(OPEN_FILE));
    }

    public static void downloadFile() {
        driver.findElement(By.cssSelector(".more-button")).click();
        sleep(1);
        driver.findElement(By.cssSelector(".bubble-menu-item")).click();
        sleep(4);
    }

    @Test
    public static void testLogin() {
        successfulLogin();
    }

    @Test
    public static void testFileView() {
        successfulLogin();
        viewFile();
    }

    @Test
    public static void testFileDownload() {
        successfulLogin();
        viewFile();
        downloadFile();
    }

}
[15.07.2016 18:54:56] Robert: /**
 * Created by MBCNEWMAIN on 12.07.2016.
 */
public interface Dropbox_variables {

    String LOGIN_PAGE = "https://www.dropbox.com/login";
    String LOGIN_INPUT = "input[name='login_email']";
    String PASSWORD_INPUT = "input[name='login_password']";
    String USERNAME = "doron@skyfence.com";
    String PASS = "1q1q1q";
    String LOGIN_BUTTON = ".login-button";
    String FILE = "a[href='//www.dropbox.com/pri/get/billingccn2.docx?_subject_uid=283204911&w=AAC1YBt-Lc--ZUmOgQpm8a_8LUS8mwCm8UaYl2Fh5vx3vg']";
    String OPEN_FILE = ".share-button";

}

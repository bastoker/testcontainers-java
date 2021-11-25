Testcontainers module to run Playwright browser in docker.

Goal is to support Playwright (i.e. not Playwright Test) with its Java flavour as a test container.

GitHub project for Playwright Java is https://github.com/microsoft/playwright-java

With it this should be possible:

```java
import com.microsoft.playwright.*;
import org.junit.jupiter.*;

public class BaseTests {
    PlaywrightContainer<?> browsers = new PlaywrightContainer<>();

    private static Browser browser;

    @BeforeAll
    public static void setUp() {
        browser = browsers.create().webkit().launch(); // This is where the magic happens
        Page page = browser.newPage();
        page.navigate("http://whatsmyuseragent.org/");
        page.screenshot(new Page.ScreenshotOptions().setPath(Paths.get("example.png")));
    }

    @Test
    public void myTest() {
        Page page = browser.newPage();
        page.navigate("http://whatsmyuseragent.org/");
        page.screenshot(new Page.ScreenshotOptions().setPath(Paths.get("example.png")));
    }
}
```

Without Testcontainers Playwright will install everything locally with the default way it is to be used from Java:

```java
import com.microsoft.playwright.*;
import java.nio.file.Paths;

public class WebKitScreenshot {
    public static void main(String[] args) {
        try (Playwright playwright = Playwright.create()) {
            Browser browser = playwright.webkit().launch();
            Page page = browser.newPage();
            page.navigate("http://whatsmyuseragent.org/");
            page.screenshot(new Page.ScreenshotOptions().setPath(Paths.get("example.png")));
       }
    }
}
```

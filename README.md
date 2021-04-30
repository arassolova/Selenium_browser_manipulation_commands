# Selenium_browser_manipulation_commands

https://www.selenium.dev/documentation/en/webdriver/browser_manipulation/ 

## Навигация:

driver.get(“url...”);

driver.navigate().to(“url...”);
driver.navigate().back();
driver.navigate().forward();
driver.navigate().refresh();

driver.getCurrentUrl();

driver.getTitle();

## Окна и вкладки:

driver.getWindowHandle(); - узнаем id открытой вкладки/окна

### переключение вкладок/окон:

//Сохраняем ID окна/вкладки
String originalWindow = driver.getWindowHandle();

//Проверяем, что нет других открытых окон/вкладок:
assert driver.getWindowHandles().size() == 1;

//Кликаем по ссылке, которая открывается в новой вкладке/окне:
driver.findElement(By.linkText(“new window”)).click();

//Ждем новую вкладку/окно:
wait.until(numberOfWindowsToBe(2));

//Проходимся по вкладкам/окнам пока не найдем новое окно/вкладку:
for(String windowHandle : driver.getWindowHandles()) {
	if(!originalWindow.contentEquals(windowHandle)) {
	driver.switchTo().window(windowHandle);
	break;
}
}

//Ждем пока новая вкладка закончит загрузку контента:
wait.until(titleIs(“Selenium documentation”));


### создание новой вкладки/окна:

//открыть новую вкладку и перейти на нее:
driver.switchTo().newWindow(WindowType.TAB);

//открыть новое окно и перейти на него:
driver.switchTo().newWindow(WindowType.WINDOW);

### закрытие окна и вкладки:

//Закрыть вкладку или окно:
driver.close();

//Переключиться обратно на старую вкладку/окно:
//driver.switchTo().window(originalWindow);

!!! Если после закрытия вкладки/окна забыть переключиться на другую вкладку/окно, то драйвер останется на только что закрытой и возникнет Now such window exception

### Выход из браузера в конце сессии:

driver.quit();

!!! Что сделает:
закроет все вкладки и окна, открытые вебдрайвером
закроет все процессы браузера
закроет сопутствующие процессы драйвера
уведомит Selenium Grid, что браузер больше не используется и может быть использован для новой сессии (если вы используете Selenium Grid)

**Лайфхак, чтобы закрыть в конце (не всем подходит):**

/**
 * Example using JUnit
 * https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/AfterAll.html
 */
@AfterAll
public static void tearDown() {
    driver.quit();
}


  
try {
    //WebDriver code here...
} finally {
    driver.quit();
}

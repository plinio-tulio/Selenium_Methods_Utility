# Selenium_Methods_Utility
Métodos comuns que podem ser utilizados em seu projeto de automação web utilizando Selenium e linguagem Java. Todos os métodos estão considerando como parâmetro o elemento da página do tipo By. Adaptações podem ser necessárias para que possa aplicar em seu projeto, como por exemplo, a forma que utiliza o driver.

#### 1 - Realizar Scroll até o elemento que está escondido na tela para que possa ser clicado. O método já efetua o scroll e o click.

```java
public static void scrollClique(By element) {
   WebElement ele = getDriver().findElement(element);
   ((JavascriptExecutor) getDriver()).executeScript("arguments[0].scrollIntoView(true);",ele);
   JavascriptExecutor executor = (JavascriptExecutor) getDriver();
   executor.executeScript("arguments[0].click();", ele);
}
```

#### 2 - Aguardar elemento ficar visível.

```java
public void waitElementVisible(By element){
   WebDriverWait wait = new WebDriverWait(getDriver(), 10);
   wait.until(ExpectedConditions.visibilityOf(getDriver().findElement(element)));
}
```

#### 3 - Encontrar um elemento por texto em uma lista de elementos.

```java
protected WebElement fingElementByText(java.util.List<WebElement> webElements, String text) {
   return webElements
         .stream()
         .filter(webElement -> Objects.equals(webElement.getText().trim(), text.trim()))
         .findFirst()
         .orElseThrow(() -> new NoSuchElementException("No WebElement found containing " + text));
}
```

#### 4 - Encontrar um elemento por valor em uma lista de elementos.

```java
protected WebElement findElementByValue(java.util.List<WebElement> webElements, String text) {
   return webElements
         .stream()
         .filter(webElement -> Objects.equals(webElement.getAttribute("value").trim(), text.trim()))
         .findFirst()
         .orElseThrow(() -> new NoSuchElementException("No WebElement found containing " + text));
}
```


#### 5 - Aguardar a página ser carregada.

```java
public void waitForPageLoad() {
   WebDriverWait wait = new WebDriverWait(getDriver(), 30);
   wait.until(new Function<WebDriver, Boolean>() {
      public Boolean apply(WebDriver driver) {
         System.out.println("Current Window State       : "
               + String.valueOf(((JavascriptExecutor) driver).executeScript("return document.readyState")));
         return String
               .valueOf(((JavascriptExecutor) driver).executeScript("return document.readyState"))
               .equals("complete");
      }
   });
}
```

#### 6 - Capturar o locator do elemento.

```java
private static String getLocatorFromWebElement(WebElement element) {

   return element.toString().split("->")[1].replaceFirst("(?s)(.*)\\]", "$1" + "");
}
```

#### 7 - Transformar um objeto do tipo WebElement para o tipo By.

```java
private static By getByFromElement(WebElement element) {

   By by = null;
   //[[ChromeDriver: chrome on XP (d85e7e220b2ec51b7faf42210816285e)] -> xpath: //input[@title='Search']]
   String[] pathVariables = (element.toString().split("->")[1].replaceFirst("(?s)(.*)\\]", "$1" + "")).split(":");

   String selector = pathVariables[0].trim();
   String value = pathVariables[1].trim();

   switch (selector) {
      case "id":
         by = By.id(value);
         break;
      case "className":
         by = By.className(value);
         break;
      case "tagName":
         by = By.tagName(value);
         break;
      case "xpath":
         by = By.xpath(value);
         break;
      case "cssSelector":
         by = By.cssSelector(value);
         break;
      case "linkText":
         by = By.linkText(value);
         break;
      case "name":
         by = By.name(value);
         break;
      case "partialLinkText":
         by = By.partialLinkText(value);
         break;
      default:
         throw new IllegalStateException("locator : " + selector + " not found!!!");
   }
   return by;
}
```

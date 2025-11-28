### iframes

A frame (or iframe) is a web page **embedded inside another web page**.

Selenium cannot directly interact with elements inside an iframe unless you switch to it first.

### Why Do We Need to Switch to Frames?

Because Selenium always starts on the main DOM.
Elements inside an iframe belong to a different DOM, so Selenium must switch to that frame before interacting.

## methods to switch 

### 1. Switch Using Index
```
driver.switchTo().frame(0);   // First frame on the page

driver.switchTo().frame(1);   // Second frame
```
✔ Use this only when frames do not have names or IDs
❌ Index may change if developers reorder frames

### 2. Switch Using Name or ID
```
driver.switchTo().frame("frameName");
driver.switchTo().frame("frameID");
```
### 3. Switch Using WebElement

```
WebElement frameElement = driver.findElement(By.xpath("//iframe[@class='content-frame']"));
driver.switchTo().frame(frameElement);
```

### Switch Back Out of the Frame
🔹 To go back to main page:

```
driver.switchTo().defaultContent();
```
## eg:

```
driver.switchTo().frame("emailFrame");

driver.findElement(By.id("email")).sendKeys("test@example.com");

// returning to main page
driver.switchTo().defaultContent();
```

### to count no.of frames

```
int frameCount = driver.findElements(By.tagName("iframe")).size();
System.out.println(frameCount);
```


# Extension hands-on

## Step 1. Create a folder [ and remember where it is 👮 ]

## Step 2. Create a manifest file ( manifest.json)

```json
{
	"name": "<name for extension>",
	"version": "1.0",
	"description": "<description>",
	"manifest_version": 2
}
```

## Step 3. Add extension to browser ( both chrome and edge )

- open edge://extensions/ in your browsers
- DEVELOPER MODE has to be enabled to be able to upload the extension
- see the extension next to address bar and pin it.

## Step 4. Create popup page

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div>
    <input id="text" type="number">
    <button id="btn">submit</button>
  </div>
</body>
</html>
```

## Step 5. Add popup page in manifest

```json
"browser_action": {
	"default_popup": "popup.html"
}
```

## Step 6. Copy paste this CSS

```css
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  width: 300px;
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
}

div * {
  padding: 11px;
  font-size: 1.2em;
  max-width: 100px;
}

button {
  cursor: pointer;
}
```

## Step 7. Copy this javascript and debug the popup script

- do some simple JS to show where to debug

```jsx
document.getElementById('btn').addEventListener('click', () => {
	chrome.runtime.sendMessage({greeting:document.getElementById('text').value}, function(response) {
		console.log(response, 'popup');
	});
})
```

- link to send message documentation to understand what it does
- add `popup.js` link in html

## Step 8. Background script

- dummy script to debug background

## Step 9. Add background script to manifest

```json
"background": {
    "scripts": ["background.js"],
    "persistent": true
  }
```

- explain `persistent` keyword

## Step 10. Add message listeners in background

```jsx
let id = null;
chrome.runtime.onMessage.addListener(
  function(msg, sender, sendResponse) {
    console.log(msg, 'bg');

    
    let interval = +msg.greeting;
    if (isNaN(interval)) interval = 10000;
    else interval *= 1000;
    
    clearInterval(id);

    id = setInterval(() => msgContentScript(msg), interval);

    sendResponse('from bg')
    
  }
);

function msgContentScript(msg) {
  chrome.tabs.query({active: true, currentWindow: true}, function(tabs) {
    chrome.tabs.sendMessage(tabs[0].id, {greeting: msg.greeting});
  });
}
```

- explain in parts when by first adding listener and then adding message passing to content
- link to documentation

## Step 11. Add a content script

- dummy content script and **debug** it
- refresh extension to update css changes

## Step 12. Add content script in manifest

```json
"content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"],
      "css": ["content.css"]
    }
  ]
```

## Step 13. Add message listener to content script

```jsx
let id = null;
console.log('hi content');
chrome.runtime.onMessage.addListener(
  function(msg) {
    

    addDiv();

  }
);

function addDiv() {
  const div = document.createElement('div');
  div.classList.add('overlayFromExtension');
  div.innerText = "hello from extension";
  document.body.appendChild(div);

  setTimeout(() => div.remove(), 2000);
}
```

- add new tab to see it works there as well.

## Step 14. Add CSS as well for content script

```jsx
.overlayFromExtension{
  color: red;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0,0,0,.8);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3em;
  z-index: 9999;
}
```

## Step 16. Disable extension in the add to avoid inject content script everywhere.

## Demo installing and running in edge

- chrome://extensions/

## Tell about options page

## ideas for extensions

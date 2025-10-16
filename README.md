# Observer.js
This script is designed to track and forward user interactions within a webpage using the `SiteOSClient`. It captures mouse movements, clicks, keyboard usage, window focus/blur/resizing and custom events which are emitted to the parent window.
## Including the Script
```html
<script src="https://caveon-public.s3.us-west-2.amazonaws.com/observer.js" type="module"></script>
```
## Registered Listeners
Listeners that are added in this script:
- click
- mousemove
- contextmenu
- keydown
- keyup
- focus
- blur
- resize
- observer-event
`mousemove` and `resize` events are throttled to fire at most once every 100ms.
## Sending Custom Events
If you want to send custom data you can dispatch an `observer-event`:
```js
const payload = {
    logMessage: '',  // String - message to store in the log
    chatMessage: '', // String - message to show in the chat
    warn: false      // Boolean - whether to count this event as a warning
}
const customEvent = new CustomEvent('observer-event', { detail: payload })
window.dispatchEvent(customEvent)
```
### Posting directly to the parent
If you want to send a message without dispatching an event through this script:
```js
const payload = {
    name: 'observer-event',
    args: [
        {
            logMessage: '',  // String - message to store in the log
            chatMessage: '', // String - message to show in the chat
            warn: false      // Boolean - whether to count this event as a warning
        }
    ]
}
window.parent.postMessage(payload, 'https://observer.caveon.com')
```
## Listening for Events
The script may emit custom events of type `'observer'` received from the parent window. You can listen for them like this:
```js
window.addEventListener('observer', (event) => {
  console.log('received observer event:', event.detail)
})
```

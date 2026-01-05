# Observer.js

This script is designed to track and forward user interactions within a webpage using the `SiteOSClient`. It captures mouse movements, clicks, keyboard usage, window focus/blur/resizing and custom events which are emitted to the parent window.

## Including the Script

```html
<script src="https://caveon-public.s3.us-west-2.amazonaws.com/observer.min.js" type="module"></script>
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
- observer-end
- observer-response

`mousemove` and `resize` events are throttled to fire at most once every 100ms.

## Sending Custom Events

Send custom logs and chat messages with `observer-event`:

```js
const payload = {
    log_message: '',  // String  - message to store in the log
    chat_message: '', // String  - message to show in the chat
    warn: false       // Boolean - whether to count this event as a warning
}

const customEvent = new CustomEvent('observer-event', { detail: payload })

window.dispatchEvent(customEvent)
```

Signal that the test has ended with `observer-end`:
```js
const payload = {
    redirect_url: '' // String - url to redirect to on test end
}

const customEvent = new CustomEvent('observer-end', { detail: payload })

window.dispatchEvent(customEvent)
```

Send item responses with `observer-response`:

```js
const payload = {
    item_id: '',      // String  (Optional) - id of item
    item_content: '', // String  (Optional) - content of item
    item_type: '',    // String  (Optional) - type of item
    item_length: 0    // Integer (Optional) - character count of item
}

const customEvent = new CustomEvent('observer-response', { detail: payload })

window.dispatchEvent(customEvent)
```

### Posting directly to the parent

If you want to send a custom event without the use of this script:

Observer Event

```js
const payload = {
    name: 'observer-event',
    args: [
        {
            log_message: '',  // String  - message to store in the log
            chat_message: '', // String  - message to show in the chat
            warn: false       // Boolean - whether to count this event as a warning
        }
    ]
}

window.parent.postMessage(payload, 'https://observer.caveon.com')
```

Observer End

```js
const payload = {
    name: 'observer-end',
    args: [
        {
            redirect_url: '' // String - url to redirect to on test end
        }
    ]
}

window.parent.postMessage(payload, 'https://observer.caveon.com')
```

Observer Response

```js
const payload = {
    name: 'observer-response',
    args: [
        {
            item_id: '',      // String  (Optional) - id of item
            item_content: '', // String  (Optional) - content of item
            item_type: '',    // String  (Optional) - type of item
            item_length: 0    // Integer (Optional) - character count of item
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

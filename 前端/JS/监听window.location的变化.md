如下，只需要监听两个事件的变化即可，window.history.pushState和window.history.replaceState

```js
const originalPushState = window.history.pushState;
const originalReplaceState = window.history.replaceState;

window.history.pushState = function (data, title, url) {
  window.dispatchEvent(new window.Event("locationchange"));
  return originalPushState.call(window.history, data, title, url);
};

window.history.replaceState = function (data, title, url) {
  window.dispatchEvent(new window.Event("locationchange"));
  return originalReplaceState.call(window.history, data, title, url);
};

window.addEventListener("popstate", function () {
  console.log("locationchange");
  console.log("最新的location href", window.location.href);
});
```
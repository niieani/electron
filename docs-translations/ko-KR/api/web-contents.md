# webContents

`webContents`는 [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter)를
상속받았습니다. 웹 페이지의 렌더링과 관리를 책임지며
[`BrowserWindow`](browser-window.md)의 속성입니다. 다음은 `webContents` 객체에
접근하는 예제입니다:

```javascript
const BrowserWindow = require('electron').BrowserWindow;

var win = new BrowserWindow({width: 800, height: 1500});
win.loadURL("http://github.com");

var webContents = win.webContents;
```

## Events

`webContents` 객체는 다음과 같은 이벤트들을 발생시킵니다:

### Event: 'did-finish-load'

탐색 작업이 끝났을 때 발생하는 이벤트입니다. 브라우저의 탭의 스피너가 멈추고 `onload`
이벤트가 발생했을 때를 말합니다.

### Event: 'did-fail-load'

Returns:

* `event` Event
* `errorCode` Integer
* `errorDescription` String
* `validatedURL` String

이 이벤트는 `did-finish-load`와 비슷하나, 로드가 실패했거나 취소되었을 때 발생합니다.
예를 들면 `window.stop()`이 실행되었을 때 발생합니다. 발생할 수 있는 전체 에러 코드의
목록과 설명은 [여기](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h)서
확인할 수 있습니다.

### Event: 'did-frame-finish-load'

Returns:

* `event` Event
* `isMainFrame` Boolean

프레임(Frame)이 탐색을 끝냈을 때 발생하는 이벤트입니다.

### Event: 'did-start-loading'

브라우저 탭의 스피너가 회전을 시작한 때와 같은 시점에 대응하는 이벤트입니다.

### Event: 'did-stop-loading'

브라우저 탭의 스피너가 회전을 멈추었을 때와 같은 시점에 대응하는 이벤트입니다.

### Event: 'did-get-response-details'

Returns:

* `event` Event
* `status` Boolean
* `newURL` String
* `originalURL` String
* `httpResponseCode` Integer
* `requestMethod` String
* `referrer` String
* `headers` Object

요청한 리소스에 관련된 자세한 정보를 사용할 수 있을 때 발생하는 이벤트입니다.
`status`는 리소스를 다운로드하기 위한 소켓 연결을 나타냅니다.

### Event: 'did-get-redirect-request'

Returns:

* `event` Event
* `oldURL` String
* `newURL` String
* `isMainFrame` Boolean
* `httpResponseCode` Integer
* `requestMethod` String
* `referrer` String
* `headers` Object

리소스를 요청하는 동안에 리다이렉트 응답을 받았을 때 발생하는 이벤트입니다.

### Event: 'dom-ready'

Returns:

* `event` Event

주어진 프레임의 문서가 로드되었을 때 발생하는 이벤트입니다.

### Event: 'page-favicon-updated'

Returns:

* `event` Event
* `favicons` Array - URL 배열

페이지가 favicon(파비콘) URL을 받았을 때 발생하는 이벤트입니다.

### Event: 'new-window'

Returns:

* `event` Event
* `url` String
* `frameName` String
* `disposition` String - `default`, `foreground-tab`, `background-tab`,
  `new-window`, `other`중 하나일 수 있습니다.
* `options` Object - 새로운 `BrowserWindow` 객체를 만들 때 사용되는 옵션 객체입니다.

페이지가 `url`에 대하여 새로운 윈도우를 열기위해 요청한 경우 발생하는 이벤트입니다.
`window.open`이나 `<a target='_blank'>`과 같은 외부 링크에 의해 요청될 수 있습니다.

기본값으로 `BrowserWindow`는 `url`을 기반으로 생성됩니다.

`event.preventDefault()`를 호출하면 새로운 창이 생성되는 것을 방지할 수 있습니다.

### Event: 'will-navigate'

Returns:

* `event` Event
* `url` String

사용자 또는 페이지가 새로운 페이지로 이동할 때 발생하는 이벤트입니다.
`window.location` 객체가 변경되거나 사용자가 페이지의 링크를 클릭했을 때 발생합니다.

이 이벤트는 `webContents.loadURL`과 `webContents.back` 같은 API를 이용하여
프로그램적으로 시작된 탐색에 대해서는 발생하지 않습니다.

`event.preventDefault()`를 호출하면 탐색을 방지할 수 있습니다.

### Event: 'crashed'

렌더러 프로세스가 예기치 못하게 종료되었을 때 발생되는 이벤트입니다.

### Event: 'plugin-crashed'

Returns:

* `event` Event
* `name` String
* `version` String

플러그인 프로세스가 예기치 못하게 종료되었을 때 발생되는 이벤트입니다.

### Event: 'destroyed'

`webContents`가 소멸될 때 발생되는 이벤트입니다.

### Event: 'devtools-opened'

개발자 도구가 열렸을 때 발생되는 이벤트입니다.

### Event: 'devtools-closed'

개발자 도구가 닫혔을 때 발생되는 이벤트입니다.

### Event: 'devtools-focused'

개발자 도구에 포커스가 가거나 개발자 도구가 열렸을 때 발생되는 이벤트입니다.

### Event: 'certificate-error'

Returns:

* `event` Event
* `url` URL
* `error` String - 에러 코드
* `certificate` Object
  * `data` Buffer - PEM 인코딩된 데이터
  * `issuerName` String
* `callback` Function

`url`에 대한 `certificate` 인증서의 유효성 검증에 실패했을 때 발생하는 이벤트입니다.

사용법은 [`app`의 `certificate-error` 이벤트](app.md#event-certificate-error)와
같습니다.

### Event: 'select-client-certificate'

Returns:

* `event` Event
* `url` URL
* `certificateList` [Objects]
  * `data` Buffer - PEM 인코딩된 데이터
  * `issuerName` String - 인증서 발급자 이름
* `callback` Function

클라이언트 인증이 요청되었을 때 발생하는 이벤트 입니다.

사용법은 [`app`의 `select-client-certificate` 이벤트](app.md#event-select-client-certificate)와
같습니다.

### Event: 'login'

Returns:

* `event` Event
* `request` Object
  * `method` String
  * `url` URL
  * `referrer` URL
* `authInfo` Object
  * `isProxy` Boolean
  * `scheme` String
  * `host` String
  * `port` Integer
  * `realm` String
* `callback` Function

`webContents`가 기본 인증을 수행하길 원할 때 발생되는 이벤트입니다.

[`app`의 `login`이벤트](app.md#event-login)와 사용 방법은 같습니다.

## Instance Methods

`webContents`객체는 다음과 같은 인스턴스 메서드들을 가지고 있습니다.

### `webContents.loadURL(url[, options])`

* `url` URL
* `options` Object (optional), 속성들:
  * `httpReferrer` String - HTTP Referrer url.
  * `userAgent` String - 요청을 시작한 유저 에이전트.
  * `extraHeaders` String - "\n"로 구분된 Extra 헤더들.

윈도우에 웹 페이지 `url`을 로드합니다. `url`은 `http://` or `file://`과 같은
프로토콜 접두사를 가지고 있어야 합니다. 만약 반드시 http 캐시를 사용하지 않고 로드해야
하는 경우 `pragma` 헤더를 사용할 수 있습니다.

```javascript
const options = {"extraHeaders" : "pragma: no-cache\n"}
webContents.loadURL(url, options)
```

### `webContents.getURL()`

현재 웹 페이지의 URL을 반환합니다.

```javascript
var win = new BrowserWindow({width: 800, height: 600});
win.loadURL("http://github.com");

var currentURL = win.webContents.getURL();
```

### `webContents.getTitle()`

현재 웹 페이지의 제목을 반환합니다.

### `webContents.isLoading()`

현재 웹 페이지가 리소스를 로드중인지 여부를 반환합니다.

### `webContents.isWaitingForResponse()`

현재 웹 페이지가 페이지의 메인 리소스로부터 첫 응답을 기다리고있는지 여부를 반환합니다.

### `webContents.stop()`

대기중인 탐색 작업을 모두 멈춥니다.

### `webContents.reload()`

현재 웹 페이지를 새로고침합니다.

### `webContents.reloadIgnoringCache()`

현재 웹 페이지의 캐시를 무시한 채로 새로고침합니다.

### `webContents.canGoBack()`

브라우저가 이전 웹 페이지로 돌아갈 수 있는지 여부를 반환합니다.

### `webContents.canGoForward()`

브라우저가 다음 웹 페이지로 이동할 수 있는지 여부를 반환합니다.

### `webContents.canGoToOffset(offset)`

* `offset` Integer

웹 페이지가 `offset`로 이동할 수 있는지 여부를 반환합니다.

### `webContents.clearHistory()`

탐색 기록을 삭제합니다.

### `webContents.goBack()`

브라우저가 이전 웹 페이지로 이동하게 합니다.

### `webContents.goForward()`

브라우저가 다음 웹 페이지로 이동하게 합니다.

### `webContents.goToIndex(index)`

* `index` Integer

브라우저가 지정된 절대 웹 페이지 인덱스로 탐색하게 합니다.

### `webContents.goToOffset(offset)`

* `offset` Integer

"current entry"에서 지정된 offset으로 탐색합니다.

### `webContents.isCrashed()`

렌더러 프로세스가 예기치 않게 종료되었는지 여부를 반환합니다.

### `webContents.setUserAgent(userAgent)`

* `userAgent` String

현재 웹 페이지의 유저 에이전트를 덮어씌웁니다.

### `webContents.getUserAgent()`

현재 웹 페이지의 유저 에이전트 문자열을 반환합니다.

### `webContents.insertCSS(css)`

* `css` String

CSS 코드를 현재 웹 페이지에 삽입합니다.

### `webContents.executeJavaScript(code[, userGesture])`

* `code` String
* `userGesture` Boolean (optional)

페이지에서 자바스크립트 코드를 실행합니다.

기본적으로 `requestFullScreen`와 같은 몇몇 HTML API들은 사용자의 조작에 의해서만
호출될 수 있습니다. `userGesture`를 `true`로 설정하면 이러한 제약을 무시할 수
있습니다.

### `webContents.setAudioMuted(muted)`

+ `muted` Boolean

현재 웹 페이지의 소리를 음소거합니다.

### `webContents.isAudioMuted()`

현재 페이지가 음소거 되어있는지 여부를 반환합니다.

### `webContents.undo()`

웹 페이지에서 `undo` 편집 커맨드를 실행합니다.

### `webContents.redo()`

웹 페이지에서 `redo` 편집 커맨드를 실행합니다.

### `webContents.cut()`

웹 페이지에서 `cut` 편집 커맨드를 실행합니다.

### `webContents.copy()`

웹 페이지에서 `copy` 편집 커맨드를 실행합니다.

### `webContents.paste()`

웹 페이지에서 `paste` 편집 커맨드를 실행합니다.

### `webContents.pasteAndMatchStyle()`

웹 페이지에서 `pasteAndMatchStyle` 편집 커맨드를 실행합니다.

### `webContents.delete()`

웹 페이지에서 `delete` 편집 커맨드를 실행합니다.

### `webContents.selectAll()`

웹 페이지에서 `selectAll` 편집 커맨드를 실행합니다.

### `webContents.unselect()`

웹 페이지에서 `unselect` 편집 커맨드를 실행합니다.

### `webContents.replace(text)`

* `text` String

웹 페이지에서 `replace` 편집 커맨드를 실행합니다.

### `webContents.replaceMisspelling(text)`

* `text` String

웹 페이지에서 `replaceMisspelling` 편집 커맨드를 실행합니다.

### `webContents.hasServiceWorker(callback)`

* `callback` Function

ServiceWorker가 등록되어있는지 확인하고 `callback`에 대한 응답으로 boolean 값을
반환합니다.

### `webContents.unregisterServiceWorker(callback)`

* `callback` Function

ServiceWorker가 존재하면 모두 등록을 해제하고 JS Promise가 만족될 때 `callback`에
대한 응답으로 boolean을 반환하거나 JS Promise가 만족되지 않을 때 `false`를 반환합니다.

### `webContents.print([options])`

`options` Object (optional), properties:

* `silent` Boolean - 사용자에게 프린트 설정을 묻지 않습니다. 기본값을 `false`입니다.
* `printBackground` Boolean - 웹 페이지의 배경 색과 이미지를 출력합니다. 기본값은
	`false`입니다.

윈도우의 웹 페이지를 프린트합니다. `silent`가 `false`로 지정되어있을 땐, Electron이
시스템의 기본 프린터와 기본 프린터 설정을 가져옵니다.

웹 페이지에서 `window.print()`를 호출하는 것은
`webContents.print({silent: false, printBackground: false})`를 호출하는 것과
같습니다.

**참고:** Windows에서의 프린터 API는 `pdf.dll`에 의존합니다. 따라서 어플리케이션이
print기능을 사용하지 않는 경우 전체 바이너리 크기를 줄이기 위해 `pdf.dll`을 삭제해도
됩니다.

### `webContents.printToPDF(options, callback)`

`options` Object, properties:

* `marginsType` Integer - 사용할 마진의 종류를 지정합니다.
  * 0 - default
  * 1 - none
  * 2 - minimum
* `pageSize` String - 생성되는 PDF의 페이지 크기를 지정합니다.
  * `A4`
  * `A3`
  * `Legal`
  * `Letter`
  * `Tabloid`
* `printBackground` Boolean - CSS 배경을 프린트할지 여부를 정합니다.
* `printSelectionOnly` Boolean - 선택된 영역만 프린트할지 여부를 정합니다.
* `landscape` Boolean - landscape을 위해선 `true`를, portrait를 위해선 `false`를
	사용합니다.

`callback` Function - `function(error, data) {}`

* `error` Error
* `data` Buffer - PDF 파일 내용.

Chromium의 미리보기 프린팅 커스텀 설정을 이용하여 윈도우의 웹 페이지를 PDF로
프린트합니다.

기본으로 비어있는 `options`은 다음과 같이 여겨지게 됩니다:

```javascript
{
  marginsType: 0,
  printBackground: false,
  printSelectionOnly: false,
  landscape: false
}
```

```javascript
const BrowserWindow = require('electron').BrowserWindow;
const fs = require('fs');

var win = new BrowserWindow({width: 800, height: 600});
win.loadURL("http://github.com");

win.webContents.on("did-finish-load", function() {
  // Use default printing options
  win.webContents.printToPDF({}, function(error, data) {
    if (error) throw error;
    fs.writeFile("/tmp/print.pdf", data, function(error) {
      if (error)
        throw error;
      console.log("Write PDF successfully.");
    })
  })
});
```

### `webContents.addWorkSpace(path)`

* `path` String

특정 경로를 개발자 도구의 워크스페이스에 추가합니다.

### `webContents.removeWorkSpace(path)`

* `path` String

특정 경로를 개발자 도구의 워크스페이스에서 제거합니다.

### `webContents.openDevTools([options])`

* `options` Object (optional). Properties:
  * `detach` Boolean - 새 창에서 개발자 도구를 엽니다.

개발자 도구를 엽니다.

### `webContents.closeDevTools()`

개발자 도구를 닫습니다.

### `webContents.isDevToolsOpened()`

개발자 도구가 열려있는지 여부를 반환합니다.

### `webContents.toggleDevTools()`

개발자 도구를 토글합니다.

### `webContents.isDevToolsFocused()`

개발자 도구에 포커스가 가있는지 여부를 반화합니다.

### `webContents.inspectElement(x, y)`

* `x` Integer
* `y` Integer

(`x`, `y`)위치의 엘레먼트를 조사합니다.

### `webContents.inspectServiceWorker()`

서비스 워커 컨텍스트(service worker context)를 위한 개발자 도구를 엽니다.

### `webContents.send(channel[, arg1][, arg2][, ...])`

* `channel` String
* `arg` (optional)

`channel`을 통하여 렌더러 프로세스에 비동기 메시지를 보냅니다. 임의의 인수를 보낼수도
있습니다. 렌더러 프로세스는 `ipcRenderer` 모듈을 통하여 `channel`를 리슨하여 메시지를
처리할 수 있습니다.

메인 프로세스에서 렌더러 프로세스로 메시지를 보내는 예시 입니다:

```javascript
// In the main process.
var window = null;
app.on('ready', function() {
  window = new BrowserWindow({width: 800, height: 600});
  window.loadURL('file://' + __dirname + '/index.html');
  window.webContents.on('did-finish-load', function() {
    window.webContents.send('ping', 'whoooooooh!');
  });
});
```

```html
<!-- index.html -->
<html>
<body>
  <script>
    require('electron').ipcRenderer.on('ping', function(event, message) {
      console.log(message);  // Prints "whoooooooh!"
    });
  </script>
</body>
</html>
```

### `webContents.enableDeviceEmulation(parameters)`

`parameters` Object, properties:

* `screenPosition` String - 에뮬레이트 할 화면 종료를 지정합니다
    (기본값: `desktop`)
  * `desktop`
  * `mobile`
* `screenSize` Object - 에뮬레이트 화면의 크기를 지정합니다 (screenPosition ==
  mobile)
  * `width` Integer - 에뮬레이트 화면의 너비를 지정합니다
  * `height` Integer - 에뮬레이트 화면의 높이를 지정합니다
* `viewPosition` Object - 화면에서 뷰의 위치 (screenPosition == mobile) (기본값:
  `{x: 0, y: 0}`)
  * `x` Integer - 좌상단 모서리로부터의 x 축의 오프셋
  * `y` Integer - 좌상단 모서리로부터의 y 축의 오프셋
* `deviceScaleFactor` Integer - 디바이스의 스케일 팩터(scale factor)를 지정합니다.
	(0일 경우 기본 디바이스 스케일 팩터를 기본으로 사용합니다. 기본값: `0`)
* `viewSize` Object - 에뮬레이트 된 뷰의 크기를 지정합니다 (빈 값은 덮어쓰지 않는
  다는 것을 의미합니다)
  * `width` Integer - 에뮬레이트 된 뷰의 너비를 지정합니다
  * `height` Integer - 에뮬레이트 된 뷰의 높이를 지정합니다
* `fitToView` Boolean - 에뮬레이트의 뷰가 사용 가능한 공간에 맞추어 스케일 다운될지를
		지정합니다 (기본값: `false`)
* `offset` Object - 사용 가능한 공간에서 에뮬레이트 된 뷰의 오프셋을 지정합니다 (fit
  to view 모드 외에서) (기본값: `{x: 0, y: 0}`)
  * `x` Float - 좌상단 모서리에서 x 축의 오프셋을 지정합니다
  * `y` Float - 좌상단 모서리에서 y 축의 오프셋을 지정합니다
* `scale` Float - 사용 가능한 공간에서 에뮬레이드 된 뷰의 스케일 (fit to view 모드
  외에서, 기본값: `1`)

`parameters`로 디바이스 에뮬레이션을 사용합니다.

### `webContents.disableDeviceEmulation()`

`webContents.enableDeviceEmulation`로 활성화된 디바이스 에뮬레이선을 비활성화 합니다.

### `webContents.sendInputEvent(event)`

* `event` Object
  * `type` String (**required**) - 이벤트의 타입. 다음 값들을 사용할 수 있습니다:
    `mouseDown`,     `mouseUp`, `mouseEnter`, `mouseLeave`, `contextMenu`,
    `mouseWheel`, `mouseMove`, `keyDown`, `keyUp`, `char`.
  * `modifiers` Array - 이벤트의 수정자(modifier)들에 대한 배열. 다음 값들을 포함
    할 수 있습니다: `shift`, `control`, `alt`, `meta`, `isKeypad`, `isAutoRepeat`,
    `leftButtonDown`, `middleButtonDown`, `rightButtonDown`, `capsLock`,
    `numLock`, `left`, `right`.

Input `event`를 웹 페이지로 전송합니다.

키보드 이벤트들에 대해서는 `event` 객체는 다음 속성들을 사용할 수 있습니다:

* `keyCode` Char or String (**required**) - 키보드 이벤트로 보내지는 문자. 단일
  UTF-8 문자를 사용할 수 있고 이벤트를 발생시키는 다음 키 중 하나를 포함할 수 있습니다:
  `enter`, `backspace`, `delete`, `tab`, `escape`, `control`, `alt`, `shift`,
  `end`, `home`, `insert`, `left`, `up`, `right`, `down`, `pageUp`, `pageDown`,
  `printScreen`

마우스 이벤트들에 대해서는 `event` 객체는 다음 속성들을 사용할 수 있습니다:

* `x` Integer (**required**)
* `y` Integer (**required**)
* `button` String - 눌린 버튼. 다음 값들이 가능합니다. `left`, `middle`, `right`
* `globalX` Integer
* `globalY` Integer
* `movementX` Integer
* `movementY` Integer
* `clickCount` Integer

`mouseWheel` 이벤트에 대해서는 `event` 객체는 다음 속성들을 사용할 수 있습니다:

* `deltaX` Integer
* `deltaY` Integer
* `wheelTicksX` Integer
* `wheelTicksY` Integer
* `accelerationRatioX` Integer
* `accelerationRatioY` Integer
* `hasPreciseScrollingDeltas` Boolean
* `canScroll` Boolean

### `webContents.beginFrameSubscription(callback)`

* `callback` Function

캡처된 프레임과 프레젠테이션 이벤트를 구독하기 시작합니다. `callback`은
프레젠테이션 이벤트가 발생했을 때 `callback(frameBuffer)` 형태로 호출됩니다.

`frameBuffer`는 raw 픽셀 데이터를 가지고 있는 `Buffer` 객체입니다. 많은 장치에서
32비트 BGRA 포맷을 사용하여 효율적으로 픽셀 데이터를 저장합니다. 하지만 실질적인
데이터 저장 방식은 프로세서의 엔디안 방식에 따라서 달라집니다. (따라서 현대의 많은
프로세서에선 little-endian 방식을 사용하므로 위의 포맷을 그대로 표현합니다. 하지만
몇몇 프로세서는 big-endian 방식을 사용하는데, 이 경우 32비트 ARGB 포맷을 사용합니다)

### `webContents.endFrameSubscription()`

프레임 프레젠테이션 이벤트들에 대한 구독을 중지합니다.

### `webContents.savePage(fullPath, saveType, callback)`

* `fullPath` String - 전체 파일 경로.
* `saveType` String - 저장 타입을 지정합니다.
  * `HTMLOnly` - 페이지의 HTML만 저장합니다.
  * `HTMLComplete` - 페이지의 완성된 HTML을 저장합니다.
  * `MHTML` - 페이지의 완성된 HTML을 MHTML로 저장합니다.
* `callback` Function - `function(error) {}`.
  * `error` Error

만약 페이지를 저장하는 프로세스가 성공적으로 끝났을 경우 true를 반환합니다.

```javascript
win.loadURL('https://github.com');

win.webContents.on('did-finish-load', function() {
  win.webContents.savePage('/tmp/test.html', 'HTMLComplete', function(error) {
    if (!error)
      console.log("Save page successfully");
  });
});
```

## Instance Properties

`WebContents`객체들은 다음 속성들을 가지고 있습니다:

### `webContents.session`

이 webContents에서 사용하는 [session](session.md) 객체를 반환합니다.

### `webContents.devToolsWebContents`

이 `WebContents`에 대한 개발자 도구의 `WebContents`를 가져옵니다.

**참고:** 사용자가 절대로 이 객체를 저장해서는 안 됩니다. 개발자 도구가 닫혔을 때,
`null`이 반환될 수 있습니다.

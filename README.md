# Cindy 的實驗室 - basicScroll

[![Donate via PayPal](https://img.shields.io/badge/paypal-donate-009cde.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CYKBESW577YWE)

這是我的第一個測試 GitHub 網站。

basicScroll 能依照捲動位置變更 CSS 變數。直接在 CSS 中使用這些變數，讓你可以製作任何想要的動畫。靈感來自 [skrollr](https://github.com/Prinzhorn/skrollr) 與 [Reactive Animations with CSS Variables](http://slides.com/davidkhourshid/reactanim#/)。

## 目錄

- [示範](#示範)
- [教學](#教學)
- [功能](#功能)
- [需求](#需求)
- [安裝](#安裝)
- [API](#api)
- [實例 API](#實例-api)
- [資料](#資料)
- [相關資源](#相關資源)
- [小技巧](#小技巧)

## 示範

| 名稱 | 描述 | 連結 | 作者 |
|:-----------|:------------|:------------|:------------|
| 預設 | 涵蓋大部分功能 | [在 CodePen 上試用](http://codepen.io/electerious/pen/QGNxxx) |
| 回呼 | 透過回呼以 JS 動態設定屬性 | [在 CodePen 上試用](https://codepen.io/electerious/pen/goZRBv) |
| 視差場景 | 多層次移動的構圖 | [在 CodePen 上試用](http://codepen.io/electerious/pen/gLLozQ) | [@electerious](https://twitter.com/electerious) |
| 轉動的眼睛 | 追蹤捲動的自訂元素 | [在 CodePen 上試用](https://codepen.io/electerious/pen/MZJZxm) | [@electerious](https://twitter.com/electerious) |
| 標題爆炸 | 逐字動畫 | [在 CodePen 上試用](https://codepen.io/electerious/pen/EQzxxJ) | [@electerious](https://twitter.com/electerious) |
| 捲動與變形 | 使用 CSS clip-path 變形文字 | [在 CodePen 上試用](https://codepen.io/ainalem/pen/jZzxrP) | [@mikaelainalem](https://twitter.com/mikaelainalem) |
| JavaScript 視差 | 多個範例與除錯模式 | [在 CodePen 上試用](https://codepen.io/animaticss/pen/rNBJwmq) | [AnimatiCSS](https://www.youtube.com/channel/UC73Tk5wfEBh67Vm7gM_zaAw) |

## 教學

| 名稱 | 連結 |
|:-----------|:------------|
| 📃 使用 JS 控制的 CSS 變數實作視差捲動 | [在 Medium 閱讀](https://medium.com/@electerious/parallax-scrolling-with-js-controlled-css-variables-63cfe96820c7) |
| 🎬 蘋果風格的捲動動畫 | [在 YouTube 觀看](https://www.youtube.com/watch?v=hPd1srSWDU4) |
| 🎬 視差效果教學（🇪🇸） | [在 YouTube 觀看](https://www.youtube.com/watch?v=QeRg4t3I2zc) |

## 功能

- 不依賴任何框架
- 極佳的效能
- 同時支援行動裝置與桌面裝置
- 支援 CommonJS 與 AMD
- 簡單的 JS API

## 需求

basicScroll 依賴以下瀏覽器功能與 API：

- [CSS 自訂屬性](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)
- [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [window.requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)

部分 API 可在舊版瀏覽器中透過 polyfill 支援。請查看上述連結以確認你是否需要 polyfill 才能達到想要的瀏覽器支援度。

## 安裝

我們建議使用 [npm](https://npmjs.com) 或 [yarn](https://yarnpkg.com) 安裝 basicScroll。

```sh
npm install basicscroll
```

```sh
yarn add basicscroll
```

在 `body` 標籤的末端加入 JS 檔案…

```html
<script src="dist/basicScroll.min.js"></script>
```

…或跳過 JS 檔案，直接以模組方式使用 basicScroll：

```js
const basicScroll = require('basicscroll')
```

```js
import * as basicScroll from 'basicscroll'
```

## 使用方式

以下示範如何依捲動改變元素透明度。當元素頂端觸及視窗底部時便開始淡出，當元素的中央位於視窗中央時會達到 `.99` 的透明度。

小提示：從 `.01` 動畫到 `.99` 能避免元素在完全透明與半透明之間切換時產生的重新繪製。

```js
const instance = basicScroll.create({
	elem: document.querySelector('.element'),
	from: 'top-bottom',
	to: 'middle-middle',
	props: {
		'--opacity': {
			from: .01,
			to: .99
		}
	}
})

instance.start()
```

```css
.element {
	/*
	 * 與實例中指定的 CSS 變數保持一致。
	 */
	opacity: var(--opacity);
	/*
	 * will-change 可告訴瀏覽器元素預期會有的變化，
	 * 讓瀏覽器能提前做好最佳化。
	 */
	will-change: opacity;
}
```

## API

### .create(html, opts)

建立新的 basicScroll 實例。

別忘了將實例指定給變數。透過實例你可以…

* …啟動或停止動畫。
* …檢查實例是否正在運作。
* …取得目前的 props。
* …在視窗尺寸改變時重新計算 props。

範例：

```js
const instance = basicScroll.create({
	from: '0',
	to: '100px',
	props: {
		'--opacity': {
			from: 0,
			to: 1
		}
	}
})
```

```js
const instance = basicScroll.create({
	elem: document.querySelector('.element'),
	from: 'top-bottom',
	to: 'bottom-top',
	props: {
		'--translateY': {
			from: '0',
			to: '100%',
			timing: 'elasticOut'
		}
	}
})
```

```js
const instance = basicScroll.create({
	elem: document.querySelector('.element'),
	from: 'top-middle',
	to: 'bottom-middle',
	inside: (instance, percentage, props) => {
		console.log('viewport is inside from and to')
	},
	outside: (instance, percentage, props) => {
		console.log('viewport is outside from and to')
	}
})
```

參數：

- `data` `{Object}` 一組 [資料](#資料)。

回傳：

- `{Object}` 創建的實例。

## 實例 API

每個 basicScroll 實例都有數個方便的函式。以下列出所有函式與簡短說明。

### .start()

開始對該實例進行動畫。basicScroll 會追蹤捲動位置並相應調整實例的 [props](#props)。只有在捲動位置改變時才會更新。

範例：

```js
instance.start()
```

### .stop()

停止對該實例進行動畫。實例的所有 [props](#props) 都會維持最後的值。

範例：

```js
instance.stop()
```

### .destroy()

銷毀實例。當實例不再需要時應該呼叫。實例的所有 [props](#props) 都會維持最後的值。

範例：

```js
instance.destroy()
```

### .update()

觸發實例更新，即使實例當前已停止。

範例：

```js
const props = instance.update()
```

回傳：

- `{Object}` 套用後的 props。

### .calculate()

將實例的 [開始與結束位置](#開始與結束位置) 轉換為絕對值。basicScroll 依據這些數值在正確位置啟動與停止動畫。計算會在實例建立時執行一次。當元素位置改變或網站/視窗大小變動時，應該呼叫 `.calculate()`。

範例：

```js
instance.calculate()
```

### .isActive()

實例啟動時回傳 `true`，停止時回傳 `false`。

範例：

```js
instance.isActive()
```

回傳：

- `{Boolean}`

### .getData()

回傳計算後的資料。基本上是建立實例時使用的 [資料](#資料) 之解析版本。呼叫 [calculate](#calculate) 後資料可能會變更。

範例：

```js
instance.getData()
```

回傳：

- `{Object}` 解析後的 [資料](#資料)。

## 資料

資料物件可包含以下屬性：

```js
{
	/*
	 * DOM 元素或節點。
	 */
	elem: null,
	/*
	 * 開始與結束位置。
	 */
	from: null,
 	to: null,
	/*
	 * 直接模式。
	 */
	direct: false,
	/*
	 * 追蹤視窗尺寸變化。
	 */
	track: true,
	/*
	 * 回呼函式。
	 */
	inside: (instance, percentage, props) => {},
	outside: (instance, percentage, props) => {},
	/*
	 * 屬性。
	 */
	props: {
		/*
		 * 屬性名稱 / CSS 自訂屬性。
		 */
		'--name': {
			/*
			 * 開始與結束值。
			 */
			from: null,
			to: null,
			/*
			 * 動畫時間函式。
			 */
			timing: 'ease'
		}
	}
}
```

### DOM 元素或節點

類型：`Node` 預設：`null` 選填：`true`

DOM 元素或節點。

元素的位置與大小會用來將 [開始與結束位置](#開始與結束位置) 轉換為絕對值。沒有 DOM 元素時，basicScroll 無法判斷相對值動畫應在何時開始或停止。

當你使用絕對值時可以略過此屬性。

範例：

```js
{
	elem: document.querySelector('.element')
	/* ... */
}
```

### 開始與結束位置

類型：`Integer|String` 預設：`null` 選填：`false`

當捲動位置高於 `from` 且低於 `to` 時，basicScroll 會開始對 [props](#props) 進行動畫。可使用絕對值或相對值。

相對值需要 [DOM 元素或節點](#dom-元素或節點)。值的第一部分描述元素位置，最後一部分描述視窗位置：`<element>-<viewport>`。`from` 中的 `middle-bottom` 代表當元素中間到達視窗底部時開始動畫。

已知的相對值：`top-top`、`top-middle`、`top-bottom`、`middle-top`、`middle-middle`、`middle-bottom`、`bottom-top`、`bottom-middle`、`bottom-bottom`

若想依 [特定視窗高度](https://github.com/electerious/basicScroll/issues/26#issuecomment-449130809) 或 [帶有偏移的起迄點](https://github.com/electerious/basicScroll/issues/17#issuecomment-449134650) 來動畫，也可以追蹤自訂錨點。

範例：

```js
{
	/* ... */
	from: '0px',
	to: '100px',
	/* ... */
}
```

```js
{
	/* ... */
	from: 0,
	to: 360,
	/* ... */
}
```

```js
{
	/* ... */
	from: 'top-middle',
	to: 'bottom-middle',
	/* ... */
}
```

### 直接模式

類型：`Boolean|Node` 預設：`false` 選填：`true`

basicScroll 預設將所有 [props](#props) 全域套用。即使實例只追蹤單一元素，你也能在任何 CSS 中使用這些變數。將 `direct` 設為 `true` 或 DOM 元素/節點，可把所有 [props](#props) 直接套用到 [DOM 元素或節點](#dom-元素或節點) 或指定的 DOM 元素/節點。這也讓你可以動畫化 CSS 屬性，而不只是 CSS 變數。

- `false`：全域套用 props（預設）
- `true`：套用至 [DOM 元素或節點](#dom-元素或節點)
- `Node`：套用至你指定的 DOM 元素/節點

範例：

```html
<!-- direct: false -->
<html style="--name: 0;">
	<div class="trackedElem"></div>
	<div class="anotherElem"></div>
</html>
```

```html
<!-- direct: true -->
<html>
	<div class="trackedElem" style="--name: 0;"></div>
	<div class="anotherElem"></div>
</html>
```

```html
<!-- direct: document.querySelector('.anotherElem') -->
<html>
	<div class="trackedElem"></div>
	<div class="anotherElem" style="--name: 0;"></div>
</html>
```

### 追蹤視窗尺寸變化

類型：`Boolean` 預設：`true` 選填：`true`

basicScroll 會在視窗尺寸改變時自動重新計算並更新實例。若想自行處理，可為每個實例停用追蹤。

注意：basicScroll 只會追蹤視窗尺寸。當你修改網站而影響版面時，仍需手動重新計算並更新實例。任何會改變頁面佈局的修改都應觸發此更新。

範例：

```js
const instance = basicScroll.create({
	elem: document.querySelector('.element'),
	from: 'top-bottom',
	to: 'bottom-top',
	track: false,
	props: {
		'--opacity': {
			from: 0,
			to: 1
		}
	}
})

// 若停用了追蹤，請自行在視窗尺寸改變時重新計算與更新。
// 在正式環境中請為此函式加上防抖以避免不必要的計算。
window.onresize = function() {

	instance.calculate()
	instance.update()

}
```

### 回呼函式

類型：`Function` 預設：`() => {}` 選填：`true`

- 當使用者捲動且視窗位於給定 [開始與結束位置](#開始與結束位置) 之間時，會執行 `inside` 回呼。
- 當使用者捲動且視窗位於給定 [開始與結束位置](#開始與結束位置) 之外時，會執行 `outside` 回呼。

兩個回呼都會收到目前的實例、百分比與計算後的屬性：

- `< 0%` 百分比 = 捲動位置在 `from` 下方
- `= 0%` 百分比 = 捲動位置等於 `from`
- `= 100%` 百分比 = 捲動位置等於 `to`
- `> 100%` 百分比 = 捲動位置在 `from` 上方

範例：

```js
{
	/* ... */
	inside: (instance, percentage, props) => {},
	outside: (instance, percentage, props) => {},
	/* ... */
}
```

### Props

類型：`Object` 預設：`{}` 選填：`true`

當捲動位置變化時需要動畫化的數值。

物件中的每個 prop 都代表一個 CSS 屬性或 CSS 自訂屬性（CSS 變數）。自訂 CSS 屬性一律以兩個破折號開頭。名稱為 `--name` 的 prop 可在 CSS 內用 `var(--name)` 取得。

更多資訊請參閱 [CSS 自訂屬性](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)。

範例：

```js
{
	/* ... */
	props: {
		'--one-variable': { /* ... */ },
		'--another-variable': { /* ... */ }
	}
}
```

### 開始與結束值

類型：`Integer|String` 預設：`null` 選填：`false`

支援各種單位。當 `from` 沒有單位時，basicScroll 會使用 `to` 的單位。

範例：

```js
'--name': {
	/* ... */
	from: '0',
	to: '100px',
	/* ... */
}
```

```js
'--name': {
	/* ... */
	from: '50%',
	to: '100%',
	/* ... */
}
```

```js
'--name': {
	/* ... */
	from: '0',
	to: '1turn',
	/* ... */
}
```

### 動畫時間函式

類型：`String|Function` 預設：`linear` 選填：`true`

可使用已知的時間函式或自訂函式。緩動函式只會收到一個介於 0 與 1 之間的值（動畫已完成的百分比）。函式應回傳 0 到 1 之間的值，但部分時間函式允許小於 0 或大於 1 的結果。

已知的時間函式：`backInOut`、`backIn`、`backOut`、`bounceInOut`、`bounceIn`、`bounceOut`、`circInOut`、`circIn`、`circOut`、`cubicInOut`、`cubicIn`、`cubicOut`、`elasticInOut`、`elasticIn`、`elasticOut`、`expoInOut`、`expoIn`、`expoOut`、`linear`、`quadInOut`、`quadIn`、`quadOut`、`quartInOut`、`quartIn`、`quartOut`、`quintInOut`、`quintIn`、`quintOut`、`sineInOut`、`sineIn`、`sineOut`

範例：

```js
'--name': {
	/* ... */
	timing: 'circInOut'
}
```

```js
'--name': {
	/* ... */
	timing: (t) => t * t
}
```

## 相關資源

- [ngx-basicscroll](https://github.com/theunreal/ngx-basicscroll) - basicScroll 的 Angular 封裝
- [react-basic-scroll](https://github.com/liorbd/react-basic-scroll) - basicScroll 的 React 封裝

## 小技巧

- 只動畫 `transform` 與 `opacity`，並使用 `will-change` 來 [提示瀏覽器可能的變化](https://developer.mozilla.org/de/docs/Web/CSS/will-change)。如此能讓瀏覽器提前做好最佳化準備。
- 盡量減少實例數量。更多實例代表更多檢查、計算與樣式變更。
- 不要一次動畫所有東西，也不要一次動畫太多屬性。瀏覽器不喜歡這樣。
- 在元素上加上短暫的 transition 來讓動畫更順暢：`transform: translateY(var(--ty)); transition: transform .1s`。
- basicScroll 預設將所有 [props](#props) 全域套用。試著重複使用變數，而不是建立更多實例。

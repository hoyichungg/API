# JavaScript-使用API串接公開第三方資源

## 何為API?

API(Application Program Interface)意即應用程式的接口，用於定義軟體與軟體之間銜接所需的溝通方法。換句話說，API是讓軟體與軟體互相串接溝通橋樑。

網站可以使用Web APIs 銜接第三方軟體，獲得第三方端點(endpoint)中含有JSON格式的資料。

Web APIs透過HTTP協議方法，向第三方的公開URL端點發出需求。


## 串接一個API

- 閱讀API文件：Studio Ghibli API documentation
- 取得API endpoint：
在API文件中，滑到films部分，右側會看到GET /films，其中的URL就是API endpoint，點進這個URL會看到一個陣列中包含多個物件，並以JSON格式呈現

- 使用HTTP請求獲取資料：

    - 在.js檔案中，創造一個「XMLHttpRequest」物件，並指派給request變數。

-  使用open()函式連接，必要的參數包含「請求方法(‘GET’)」及「API端點的URL」
    - 請求完畢後，我們就有權限進使用onload()函式，進到資料中
    - 最後，send()正式送出請求

```javascript
// Create a request variable and assign a new XMLHttpRequest object to it.
var request = new XMLHttpRequest()

// Open a new connection, using the GET request on the URL endpoint
request.open('GET', 'https://ghibliapi.herokuapp.com/films')

request.onload = function () {
  // Begin accessing JSON data here
  }
```
- 處理HTTP回傳資料：
  - 回傳值為JSON格式，必須先使用用JSON.parse()將JSON轉換為javascript物件
  - 接下來就可以對資料做處理了。例：這裡的資料為陣列，所以下方用forEach()讀取陣列中的物件，並且印出每個物件的title

```javascript
// Begin accessing JSON data here
var data = JSON.parse(this.response)

if (request.status >= 200 && request.status < 400) {
  data.forEach(movie => {
    console.log(movie.title)
  })
} else {
  console.log('error')
}
```
- 接續，在頁面上顯示API資料
  - index.html中有一個div容器，設為id=’root’
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link href="style.css" rel="stylesheet" />
  </head>

  <body>
    <div id="root"></div>
    
    <script src="scripts.js"></script>
  </body>
</html>
```
- 在script.js取得root元素

```javascript
const app = document.getElementById('root')
```

- 在網頁左上放一個logo

```javascript
const logo = document.createElement('img')
```

- 新增一個div標籤元素，並且用class定義其css樣式

```javascript
const container = document.createElement('div')
container.setAttribute('class', 'container')
```
- 將新增的元素放到網頁上

```javascript
app.appendChild(logo)
app.appendChild(container)
```

- 繼續在container中填充回傳值資料

```javascript
  data.forEach(movie => {
  // Create a div with a card class
  const card = document.createElement('div')
  card.setAttribute('class', 'card')

  // Create an h1 and set the text content to the film's title
  const h1 = document.createElement('h1')
  h1.textContent = movie.title

  // Create a p and set the text content to the film's description
  const p = document.createElement('p')
  movie.description = movie.description.substring(0, 300) // Limit to 300 chars
  p.textContent = `${movie.description}...` // End with an ellipses

  // Append the cards to the container element
  container.appendChild(card)

  // Each card will contain an h1 and a p
  card.appendChild(h1)
  card.appendChild(p)
})
```

>就完成啦!! 之後再美化一下

# 參考來源
- How to Connect to an API with JavaScript
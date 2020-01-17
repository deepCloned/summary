# express 获取 request body 中的数据

## 原生获取

```
router.post('/user/register', function (req, res, next) {
  var bodyData = ''
  req.on('data', function (chunk) {
    bodyData += chunk
  })
  req.on('end', function () {
    console.log(bodyData)
  })
})
```

## 中间件 body-parser
* 不借助中间件

```
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
```

* 借助中间件
> npm install body-parser --save

```
const bodyParser = require('body-parser')
app.use(bodyParser.json())
app.usr(bodyParser.urlencoded({ extended: false }))
```
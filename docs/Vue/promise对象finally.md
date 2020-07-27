## promise 对象无论是成功还是失败都会进入的方法

```JavaScript
  httpRequest.wpa.queryIndustrialWasteWaterLoad(data)
  .then(res => {
    console.log(res)
    this.tableData = res.data
  }).catch((err) => {
    console.log(err)
  }).finally(() => {
    this.isShow = true  // 无论失败还是成功都会进入此方法
  })
```

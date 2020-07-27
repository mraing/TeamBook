## 将downloadImg处理bese64格式的图片封装成工具类


````JavaScript
import {Message} from 'element-ui'

export function downloadImg (content, fileName) {
  var base64ToBlob = function (code) {
    console.log('code', code)
    let parts = code.split('base64,')
    let contentType = parts[0].split(':')[1]
    let raw = window.atob(parts[1])
    let rawLength = raw.length
    let uInt8Array = new Uint8Array(rawLength)
    for (let i = 0; i < rawLength; ++i) {
      uInt8Array[i] = raw.charCodeAt(i)
    }
    return new Blob([uInt8Array], {
      type: contentType
    })
  }
  let aLink = document.createElement('a')
  // new Blob([content])
  let blob = base64ToBlob(content)
  let evt = document.createEvent('HTMLEvents')
  // initEvent 不加后两个参数在FF下会报错  事件类型，是否冒泡，是否阻止浏览器的默认行为
  evt.initEvent('click', true, true)
  aLink.download = fileName
  aLink.href = URL.createObjectURL(blob)
  aLink.click()
  Message.success('下载成功')
}
````

# 借助 dom-to-image 实现图片下载
## 通过 dom-to-images .topng方法将dom元素, 转化为bese64位, 再通过封装的downloadImg方法将图片下载下来

````JavaScript 
import domtoimage from 'dom-to-image'
import {downloadImg} from '@/utils/common.js'
    /** *************DIv转Png-开始***************** **/
    handleDomToImg () {
      let _this = this
      let element = document.getElementById('plumbInsWrapper')
      domtoimage.toPng(element)
        .then(function (dataUrl) {
          var img = new Image()
          img.src = dataUrl
          downloadImg(dataUrl, '总负荷统计关系图')
        })
        .catch(function (error) {
          _this.$message.error('下载图片失败，请稍后再试!')
          console.error('oops, something went wrong!', error)
        })
    }
    /** *************DIv转Png-结束***************** **/
````


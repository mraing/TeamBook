## watch监听属性如果不会改变, 就不会触发的handler方法事件

```JavaScript
export {
  watch: {
    tableData: {  // 第一种形式
      deep: true,  // 深度监听
      immediate: true,  // 进来第一次就监听
      handler (nv, ov) {
        this.$nextTick(() => {  // 等待 dom 加载完毕触发的方法
          // 监听每一次的数据改变, 都会触发初始化的方法, 这样数据也会实时更新
          this._initData()  
        })
      }
    },
    tabComponent () {  // 当 tabComponent (基本数据类型) 发生改变, 那么这个方法就会被调用
      this.checkMark = false
    }
  }
}

```

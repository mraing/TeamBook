# 封装的table表格组件 数据源 tableData 的使用

## tableData.titleData  表头格式  [ {}, {}, ...]  每一列代表一个对象, 对象中有type, title, readOnly, width等属性
## tableData.table 表格数据  [[...], [...], ...]  二维数组是行的数据, 有多少行就有多少二维数组

````JavaScript
export default {
	props: {
    tableData: {
      type: Object
    },
    domId: {
      type: String,
      default: 'excle'
    }
		...
	},
	data () {
		return {
			spreadsheet: null, // 实例
			options: {
				// 表格的配置属性
			}
		}
	}
	mounted () {
    	this._initTable()
    },
    watch: {
    	tableData: {
    		deep: true,  // 深度监听
    		immediate: true,  // 进来第一次就监听
    		handler (nv, ov) {
          this.$nextTick(() => {
              // 监听每一次的数据改变, 都会触发初始化的方法, 这样数据也会实时更新
              this._initData()  
            })
          }
    	}
    }
    methods: {
    	_initTable () {
    	 // 设置表头
        let column = []
        this.tableData.titleData.forEach((item, index) => {
        column.push({
            type: item.type,  // 表头的数据类型
            title: item.title,  // 表头的名称
            readOnly: item.readOnly === 1, // 是否设置为只读
            width: item.width  // 一列的width
        })
        this.options.columns = column
        if (this.tableData.nestedHeaders) {  // 二级表头
          this.options.nestedHeaders = this.tableData.nestedHeaders
        }
        let ele = document.getElementById(this.domId) // 默认去找 dom元素ID为excle去挂载
        this.spreadsheet = jexcel(ele, this.options)
        this._initData()  // 处理表格数据
    	},
    	_initData () {
    	  let tableData = []
        if (this.tableData.table.length === 0) {
          this.tableData.titleData.forEach(item => {
            tableData.push('')
          })
          this.tableData.table.push(tableData)
        } else {
          tableData = this.tableData.table
        }
        this.spreadsheet.setData(tableData)
    	}
    }
}
````
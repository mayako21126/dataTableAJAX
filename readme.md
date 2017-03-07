# 基于VUe的动态表格组件

> mayako

## 安装
npm install datatableajax

##使用
 <my-table :sdata="dataA" style="width: 1200px"></my-table>
- {
    "url": "链接",
    "httpData": {
       //参数
    },
    "currentPage": 0,
    "total": 10,
    "expand": true,
    "height": 600,
    "pageSize": 6,
    "filter": {}，//过滤器函数 
    "columns": [
        {
            "prop": "id",
            "label": "编号",
            "width": 150,
            "fixed": false
        }
    ],
    "search": [
        {
            "name": "SearchKey",
            "value": ""
        }
    ],
    "tempSearch": [
        {
            "name": "SearchKey",
            "value": ""
        }
    ],
    " inputs": [
        {
            "name": "搜索",
            "input": true,
            "select": false,
            "date": false,
            "label": true,
            "value": "SearchKey"
        }
    ],
    "tableData": [
    ]
}
编辑中。。

<template>
  <div>
    <my-table :sdata="dataA"></my-table>
  </div>
</template>
<script type="text/ecmascript-6">
  import barTop from '../components/DataTableAjax.vue';

  export default{
    components: {
      'my-table': barTop
    },
    methods: {},
    data: function () {
      return {
        dataA: {
          //url可以用tab.json
          url: 'http://localhost:8081/school/WebSite/admin/components/tab.json',
          httpData: {PowerID: 0},
          currentPage: 1,
          total: 150,
          pageSize: 5,
          search: [
            {name: "input", value: "搜索框"},
            {name: "input2", value: "搜索框"},
            {name: "select", value: "2"},
            {name: "date", value: ""}
          ],
          tempSearch: [
            {name: "input", value: "搜索框"},
            {name: "input2", value: "搜索框"},
            {name: "select", value: "2"},
            {name: "date", value: ""}
          ],
          inputs: [
            {
              name: "搜索框",
              input: true,
              select: false,
              date: false,
              label: true,
              value: "input"
            },
            {
              name: "搜索框2",
              input: true,
              select: false,
              date: false,
              label: true,
              value: "input2"
            },
            {
              name: "选择框",
              input: false,
              select: true,
              date: false,
              label: true,
              options: [{
                value: '1',
                label: '黄1金糕'
              }, {
                value: '2',
                label: '双1皮奶'
              }, {
                value: '3',
                label: '蚵1仔煎'
              }, {
                value: '4',
                label: '龙1须面'
              }]
            },
            {
              name: "日期选择",
              input: false,
              select: false,
              date: true,
              label: true,
              value: ""
            },
            {
              name: "检索",
              button: true,
              label: false
            }
          ],
          buttons: [{
            name: '查看',
            action: function (a, b) {
              window.alert(a)
            }
          }, {
            name: '删除',
            action: function (a, b, c) {
              c._self.tableData.splice(a, 1)
            }
          }],
          tableData: [{
            date: '2016-05-03',
            name: '王小3虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-02',
            name: '王小3虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-04',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-01',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-04',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-01',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-04',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-01',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-04',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-01',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-04',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }, {
            date: '2016-05-01',
            name: '王小虎',
            province: '上海',
            city: '普陀区',
            address: '上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄',
            zip: 200333
          }]
        }
      }
    }
  }
</script>

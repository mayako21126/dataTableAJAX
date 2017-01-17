<template>
  <div>
    <my-table :sdata="dataA" style="width: 1200px"> </my-table>
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
        dialogVisible: false,
        formData:{
          ClassroomID:"",
          ClassroomName:"",
          ClassroomTip:""
        },
        dataA: {
          url: "http://115.28.200.119:8001/" + 'Admin_2000.ashx',
          httpData: {PowerID: 2302},
          currentPage: 0,
          total: 10,
          pageSize: 6,
          columns: [
            {"prop": "Num", "label": "编号", "width": 150, "fixed": true},
            {"prop": "ClassroomID", "label": "教室编号", "width": 200, "fixed": false},
            {"prop": "ClassroomName", "label": "教室名称", "width": 200, "fixed": false},
            {"prop": "ClassroomTip", "label": "教室描述", "width": 500, "fixed": false},
            {"prop": "action", "label": "操作", "fixed": false, "action": true, "min-width": 150}
          ],
          search: [
            {
              name: "SearchKey",
              value: ""
            }
          ],
          tempSearch: [
            {
              name: "SearchKey",
              value: ""
            }
          ],
          inputs: [
            {
//							name: "搜索1",
              input: true,
              select: false,
              date: false,
              label: true,
              value: "SearchKey"
            },
            {
              name: "搜索",
              button: true,
              label: false
            }],
          buttons: [{
            name: '编辑',
            vm:this,
            action: function (a, b, c) {
              this.vm.dialogVisible = true
              this.vm.formData.ClassroomID= b.ClassroomID;
              this.vm.formData.ClassroomName= b.ClassroomName;
              this.vm.formData.ClassroomTip= b.ClassroomTip;

            }
          }, {
            name: '删除',
            action: function (a, b, c) {
              c.store.table.tableData.splice(a, 1)
            }
          },{
            name: '添加',
            action: function (a, b, c) {
              // console.log(JSON.stringify(c.store.table.tableData))
              c.store.table.tableData.push({"Num":99,"ClassroomID":16,"ClassroomName":"1206","ClassroomTip":"123"})
            }
          }],
          tableData: []

        }

      }

    },
    mounted: function () {
      this.$nextTick(function () {
//				this.$http.post(host + 'Admin.ashx', {
//					PowerID: 2302,
//					pageSize: 10,
//					currentPage: 0,
//					SearchKey: ''
//				}).then(function (res) {
////					alert(res);
//					if (!authCode(res)) {
//						return false;
//					} else {
//						var data = formatBody(res)
//						console.log(data)
//						this.dataA.tableData = data.data.tableData;
//						console.log(JSON.stringify(this.dataA.tableData))
//						this.dataA.pageSize = 10;
//						this.dataA.currentPage = data.data.currentPage;
//						this.dataA.total = data.data.total;
//						console.log(this.dataA)
//					}
//
//
//				})
      })
    }
  }
</script>

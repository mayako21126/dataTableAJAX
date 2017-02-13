<template>
  <div>
    <my-table :sdata="dataA" style="width: 1200px"> </my-table>
    <el-dialog title="温馨提示:" v-model="dialogVisible" size="tiny">
      <el-form label-position="top" :model="formData" class="demo-form-stacked">
        <el-form-item label="角色编号">
          <el-input v-model="formData.ClassroomID" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="角色名称">
          <el-input v-model="formData.ClassroomName"></el-input>
        </el-form-item>
        <el-form-item label="角色描述">
          <el-input v-model="formData.ClassroomTip"></el-input>
        </el-form-item>
      </el-form>
    </el-dialog>
    <el-dialog title="温馨提示:" v-model="dialogVisible2" size="tiny">
      <el-form label-position="top" :model="formData" class="demo-form-stacked">
        <button v-if="ban">封停</button>
        <button v-if="recover">恢复</button>
      </el-form>
    </el-dialog>
  </div>
</template>
<script type="text/ecmascript-6">
  import barTop from '../components/DataTableAjax.vue';
  import Mock from 'mockjs';
  Mock.mock(/\.json/, {
    'userlist|1-10': [{
      'id|+1': 1,
      'name': '@ctitle(3)',
      'phone': '13573547784',
      'city':'青岛',
      'proxy':'xx',
      'pm25|+1':35,
      'air':'优秀',
      'list|1-3':[{
        'itemName':'123asd ',
        'size':'333',
        'days':'30',
        'action':''
      }]
    }]
  })
  export default{
    components: {
      'my-table': barTop
    },
    methods: {},
    data: function () {
      return {
        dialogVisible: false,
        dialogVisible2: false,
        ban:false,
        recover:false,
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
          filter:function (table){
            var _table;
            for(let item of table){
              if(item.ClassroomName =='111'||item.ClassroomName =='333a'){
                item.ClassroomName ='未命名'
              }
            }
            return table;
          },
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
              console.log(c)
              this.vm.dialogVisible = true
              this.vm.formData.ClassroomID= b.ClassroomID;
              this.vm.formData.ClassroomName= b.ClassroomName;
              this.vm.formData.ClassroomTip= b.ClassroomTip;

            }
          },{
            name: '状态',
            vm:this,
            action: function (a, b, c) {
              this.vm.dialogVisible2 = true;
              console.log(b.ClassroomID)
              if(b.ClassroomID=='120'){
                this.vm.ban=true;
                this.vm.recover =true;
              }else{
                this.vm.ban = true;
                this.vm.recover = false;
              }


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

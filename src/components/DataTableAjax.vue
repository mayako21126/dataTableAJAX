<template scope>
  <div style="margin: 15px" id="tableDataAjax">
    <el-row :gutter="20">
      <el-col :span="6" v-for="(input, key) in inputs" style="margin-bottom: 15px">
        <label for="" v-if="input.label">{{input.name}}</label>
        <el-input placeholder="请输入内容" v-if="input.input" style="width: 193px"
                  v-model="tempSearch[key].value"></el-input>
        <el-select placeholder="请选择" v-if="input.select" v-model="tempSearch[key].value">
          <el-option
            v-for="item in input.options"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-date-picker v-if="input.date"
                        v-model="tempSearch[key].value"
                        align="right"
                        type="date"
                        placeholder="选择日期"
                        :picker-options="input.value">
        </el-date-picker>
        <el-button type="primary" v-if="input.button" @click="searchBy">{{input.name}}</el-button>
      </el-col>
    </el-row>
    <el-table
      :data="tableData"
      :height="height"
      border

    >
      <el-table-column type="expand" v-if="expand">
        <template scope="props" >
          <div v-for="(item, index) in props.row.list" style="width: 100%" class="datatableBox">
            <div class="item">产品名: {{ item.itemName }}</div>
            <div class="item">规格: {{ item.size }}</div>
            <div class="item">剩余天数: {{ item.days }}</div>
            <div class="item"><el-button type="info">查看数据</el-button></div>
          </div>
        </template>
      </el-table-column>

      <el-table-column v-for="(column,key) in columns"
                       :fixed="column.fixed"
                       :prop="column.prop"
                       :label="column.label"
                       :width="column.width"
      >
        <template scope="scope" >
          <div v-if="column.action">
            <el-button type="text" size="small" v-for="button in buttons"
                       @click="button.action(scope.$index, scope.row,scope)">{{button.name}}
            </el-button>
          </div>
          <div v-else>
            {{scope.row[column.prop]}}
          </div>

        </template>

      </el-table-column>

    </el-table>

    <div class="block">
      <span class="demonstration">直接前往</span>
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-size="pageSize"
        layout="prev, pager, next, jumper"
        :total="total">
      </el-pagination>
    </div>
  </div>


</template>

<script type="text/ecmascript-6">
  /**
   * Created by mayako on 2017/1/5.
   */
  function authCode(req){
    var data;
    if(typeof req.body == "object"){
      data = req.body
    }else{
      data = JSON.parse(req.body);
    }

    var result;
    switch (data.errorCode){
      case "-1006":result=false;break;
      case "-1007":result=false;break;
      case "-1008":result=false;break;
      case "-1009":result=false;break;
      case "1":result=true;break;
    }
    return result;

  }
  function formatBody (res){
    var data;
    if(typeof res.body == "object"){
      data = res.body
    }else{
      data = JSON.parse(res.body);
    }
    return data;
  }

  export default {
    props: ['sdata'],
    filters:{
      tableFilters:function(table){
        return table;
      }
    },
    mounted:  function () {
      var self = this;
      this.$nextTick(function () {
        var httpDataD=JSON.parse(JSON.stringify(this.sdata.httpData));
        for (var i in this.sdata.search) {
          httpDataD[this.sdata.search[i].name] = this.sdata.search[i].value
        }
        httpDataD['currentPage'] = this.sdata.currentPage;
        httpDataD['pageSize'] = this.sdata.pageSize;
        this.$http.post(this.sdata.url, httpDataD).then(function (res) {
          console.log(res)
          if (!authCode(res)) {
            return false;
          } else {
            var data = formatBody(res);
            this.height = this.sdata.height||250;
            this.pageSize = this.sdata.pageSize;
            this.httpData= this.sdata.httpData;
            this.search= this.sdata.search;
            this.inputs= this.sdata.inputs;
            this.tempSearch= this.sdata.tempSearch;
            this.columns = this.sdata.columns;
            this.buttons=this.sdata.buttons;
            this.expand =this.sdata.expand||false;
            this.url= this.sdata.url||"http://localhost:8081/school/WebSite/admin/components/tab.json";
            this.currentPage = this.sdata.currentPage+1;
            this.total = data.data.total;
            console.log(this)
            var tmp = this;
            setTimeout(function(){
              if(tmp.sdata.filter){
                tmp.tableData = tmp.sdata.filter(data.data.tableData);
              }else{
                tmp.tableData = data.data.tableData;
              }
              // tmp.request(tmp.currentPage)
            },200)
            console.log(this.$data)
          }
        })

      })
    },
    methods: {
      handleClick: function (a, b) {
        console.log(a);
        console.log(b);
      },
      handleSizeChange: function (val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange: function (val) {
        this.currentPage = val;
        this.request(val)
      },
      request: function (currentPage, isSearch) {
        var _isSearch = true;
        var vm = this;
        var data = JSON.parse(JSON.stringify(this.httpData))
        data['currentPage'] = currentPage-1;
        data['pageSize'] = this.pageSize;
        if (_isSearch == true) {
          for (var i in this.search) {
            data[this.search[i].name] = this.search[i].value
          }
        }
        console.log(data)
        this.$http.post(this.url, data
        ).then(
          function (res) {
            var data;
            if (typeof res.body == 'object') {
              data = res.body
            } else {
              data = JSON.parse(res.body)
            }
            if(vm.sdata.filter){
              vm.tableData = vm.sdata.filter(data.data.tableData);
            }else{
              vm.tableData = data.data.tableData;
            }
            vm.total = data.data.total;
          }, function (res) {

          }
        )
      },
      searchBy: function () {
        for (var i in this.tempSearch) {
          this.search[i].value = this.tempSearch[i].value
        }
        if(this.currentPage==1){
          this.request(1)
        }else{
          this.currentPage = 1;
        }

        //this.request(1)
      }
    },
    data: function () {
      return {
        url: "",
        httpData: {},
        currentPage: 1,
        total: 150,
        pageSize: 5,
        columns:[],
        expand:false,
        tempSearch: [

        ],
        search: [
        ],
        inputs: [
        ],
        buttons: [],
        tableData: []
      }
    }
  }
</script>
<style>
  .el-table__body-wrapper .el-table__body{
    margin-left: -1px;
  }
  .datatableBox {
    display: flex;
    height: auto;
    border-bottom: 1px solid rgba(179, 179, 179, 0.5);
  }
  .datatableBox:last-child{
    border-bottom: none;
  }
  .datatableBox .item {
    min-height: 50px;
    flex-grow: 1;
    display: flex;
    align-items: center;

    /* flex-grow定义该项目不分配剩余空间 */
  }
</style>

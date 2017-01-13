<template>
  <div style="margin: 15px">
    <!--<el-row :gutter="20">-->
    <!--<el-col :span="6"><div class="grid-content bg-purple"></div></el-col>-->
    <!--<el-col :span="6"><div class="grid-content bg-purple"></div></el-col>-->
    <!--<el-col :span="6"><div class="grid-content bg-purple"></div></el-col>-->
    <!--<el-col :span="6"><div class="grid-content bg-purple"></div></el-col>-->
    <!--</el-row>-->
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
      height="250"
      border
      style="width: 100%">
      <el-table-column
        fixed
        prop="date"
        label="日期"
        width="150">
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="120">
      </el-table-column>
      <el-table-column
        prop="province"
        label="省份"
        width="120">
      </el-table-column>
      <el-table-column
        prop="city"
        label="市区"
        width="120">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址"
        width="300">
      </el-table-column>
      <el-table-column
        prop="zip"
        label="邮编"
        width="120">
      </el-table-column>
      <el-table-column

        label="操作"
        width="100">
        <template scope="scope">
          <el-button type="text" size="small" v-for="button in buttons"
                     @click="button.action(scope.$index, scope.row,scope)">{{button.name}}
          </el-button>
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
  export default {
    props: ['sdata'],
    mounted: function () {
      this.$nextTick(function () {
       // console.log(JSON.parse(JSON.stringify(this.$data)))
        this.buttons=this.sdata.buttons
        this.currentPage= this.sdata.currentPage
        this.httpData= this.sdata.httpData
        this.pageSize= this.sdata.pageSize
        this.search= this.sdata.search
        this.tableData= this.sdata.tableData
        this.inputs= this.sdata.inputs
        this.tempSearch= this.sdata.tempSearch
        this.total= this.sdata.total
        this.url= this.sdata.url||"http://localhost:8081/school/WebSite/admin/components/tab.json"
        console.log(this.$data)
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
        data['currentPage'] = currentPage;
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
            vm.tableData = data.tableData;
            vm.total = data.total;
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

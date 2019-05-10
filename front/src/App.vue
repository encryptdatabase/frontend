<template>

<el-container >
    <el-header style=" font-size: 14px">
       <img src="/assets/cyber_logo.png">
      <el-switch  v-model="show" active-text="明文" inactive-text="密文"></el-switch>
      <el-dropdown>
        <i class="el-icon-setting" style="margin-right: 15px"></i>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>新增</el-dropdown-item>
          <el-dropdown-item>退出</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <span>lidongjie</span>
    </el-header>
  <el-container>
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
    <el-menu :default-openeds="['1', '2']">
      <el-submenu index="1">
        <template slot="title"><i class="el-icon-message"></i>数据表展示</template>
        <el-menu-item-group>
          <el-menu-item index="1-1">明文</el-menu-item>
          <el-menu-item index="1-2">密文</el-menu-item>
          <el-menu-item index="1-3">统计图展示</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
    </el-menu>
  </el-aside>
  <el-container>
    <el-main>
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item label="属性"> 
          <el-cascader :options="options" v-model="selectedOptions" > </el-cascader>
        </el-form-item>
        <el-form-item label="取值">
          <el-input v-model="selectvalue" placeholder="请输入内容"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">查询</el-button>
          </el-form-item>
        </el-form>

<el-container>
  <el-table v-if="show" :data="painlist" @sort-change="sortchange" border style="width: 100%">
  <el-table-column prop="_id" label="id" sortable="custom" > </el-table-column>
  <el-table-column prop="user_name" label="姓名"></el-table-column>
  <el-table-column prop="user_age" label="年龄" sortable="custom"> </el-table-column>
  <el-table-column prop="user_hobby" label="爱好"></el-table-column>
  <el-table-column prop="mtext" label="备注"> </el-table-column>
  <el-table-column fixed="right" label="操作">
    <template slot-scope="scope">
        <el-button
        size="mini"
        @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
      <el-button
        size="mini"
        type="danger"
        @click="handleDelete(scope.$index, scope.row)">删除</el-button>
    </template>
  </el-table-column>
</el-table>
<el-table v-else :data="cipherlist"  @sort-change="sortchange" border style="width: 100%">
  <el-table-column prop="_id" label="id" sortable="custom"  > </el-table-column>
  <el-table-column prop="user_name" label="姓名"></el-table-column>
  <el-table-column prop="user_age" label="年龄" sortable="custom" > </el-table-column>
  <el-table-column prop="user_hobby" label="爱好"></el-table-column>
  <el-table-column prop="mtext" label="备注"> </el-table-column>
  <el-table-column fixed="right" label="操作">
    <template slot-scope="scope">
        <el-button
        size="mini"
        @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
      <el-button
        size="mini"
        type="danger"
        @click="handleDelete(scope.$index, scope.row)">删除</el-button>
    </template>
  </el-table-column>
</el-table>
</el-container>
    </el-main>
  </el-container>
  </el-container>
</el-container>
</template>

<script>
export default {
  name: 'App',
  data() {
      const item = {
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      };
      return {
        painlist:Array(20).fill(item),
        cipherlist:Array(20).fill(item),
        showtable:Array(20).fill(item),
        show:true,
        selectvalue :'',
        selectedOptions :'',
        selectvalue:'',
        options:[
            {
              value:'id' ,label: 'id',children:[{value: '>',label: '大于'},{value: '=',label: '等于'},{value: '<',label: '小于'}]},
            {
              value:'name' ,label:'姓名' ,children:[{value: '=',label: '等于'}]},
            { 
              value:'age',label: '年龄',children:[{value: '>',label: '大于'},{value: '=',label: '等于'},{value: '<',label: '小于'}]},
            {
              value:'hobby',label: '爱好' ,children:[{value: '=',label: '等于'}]},
            {
              value: 'mtxt' ,label:'备注',children:[{value: '=',label: '等于'}]}]
      }
  },
  mounted(){
    this.initdata()

  },
  methods:{
    async initdata(){
    var table1
    var that=this
    this.$axios.get('http://localhost:8080//getAll').
    then(function (response) {
      console.log(response.data);
      that.painlist=response.data[0]
      that.cipherlist=response.data[1]
      that.showtable=that.painlist
      })
    },
    sortchange(column,prop,order){
      var that=this
      console.log(column['prop'])
      console.log(column['order'])
      var t
      var d
      if(column['prop']=='_id') t=1
      if(column['prop']=='user_age') t=2
      if(column['order']=='descending') d=2
      if(column['order']=='ascending') d=1
      this.$axios.get('http://localhost:8080//order',{params: {type: t,dire: d}} ).then(function (response) {
      console.log(response.data);
      that.painlist=response.data[0]
      that.cipherlist=response.data[1]
      that.showtable=that.painlist
      })

    },
    onSubmit(){
      var options=this.selectedOptions
      var value=this.selectvalue
       var that=this
      console.log(typeof options[0])
      console.log(typeof value)
      this.$axios.get('http://localhost:8080//select',{params: {label1: options[0],label2: options[1],value: value}} ).then(function (response) {
      console.log(response.data);
      that.painlist=response.data[0]
      that.cipherlist=response.data[1]
      })
    }
  }
  
}
</script>
<style>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
</style>


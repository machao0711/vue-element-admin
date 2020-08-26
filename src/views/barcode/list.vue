<template>
  <div>
    <el-row>
      <el-form label-width="100px" class="view-form">
        <el-row :gutter="20" style="margin-left: -27px;">
          <el-col :span="6">
            <el-form-item label="澄清编号">
              <el-input v-model="query.clearNo" placeholder="请输入编号" clearable />
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="招标编号">
              <el-input v-model="query.pjtCode" placeholder="请输入编号" clearable />
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="处理状态">
              <el-select
                v-model="query.stateValue"
                placeholder="请选择状态"
                clearable
                @change="selectChange"
              >
                <el-option v-for="st in status" :key="st.value" :value="st.value" :label="st.name" />
              </el-select>
            </el-form-item>
          </el-col>
          <el-button type="primary" icon="el-icon-search" @click="expertSearch">查询</el-button>
        </el-row>
      </el-form>
    </el-row>
    <div
      style="background-color: #EDEDED;height: 10px;width: 110%;margin-left: -30px;
      margin-top: 14px;margin-bottom: 14px;"
    />
    <el-row :gutter="20" style="margin-top: 8px;">
      <el-col :span="8">
        <el-button type="primary" class="mt-10" @click="clearAdd">新增</el-button>
      </el-col>
    </el-row>
    <el-table border stripe :data="expertList" class="mt-10" height="calc(100vh - 380px)">
      <no-data slot="empty" />
      <el-table-column v-if="false" prop="id" label="ID" />
      <el-table-column v-if="false" prop="pBpId" label="标保id" />
      <el-table-column prop="clearNo" label="澄清编号" />
      <el-table-column prop="pjtName" label="招标名称" :show-overflow-tooltip="true" />
      <el-table-column prop="pjtCode" label="招标编号" />
      <el-table-column prop="bidName" label="分标名称" :show-overflow-tooltip="true" />
      <el-table-column prop="packCode" label="包号" />
      <el-table-column prop="pTpName" label="提交人" />
      <el-table-column prop="year" label="创建时间" />
      <el-table-column prop="cryQuestion" label="澄清问题" :show-overflow-tooltip="true" />
      <el-table-column prop="cryFileStatus" label="状态">
        <template slot-scope="scope">
          <span v-if="scope.row.cryFileStatus =='1'">已创建</span>
          <span v-if="scope.row.cryFileStatus =='2'">已受理</span>
          <span v-if="scope.row.cryFileStatus =='3'">已审核</span>
          <span v-if="scope.row.cryFileStatus =='4'">已完成</span>
        </template>
      </el-table-column>
      <el-table-column width="180px" label="操作">
        <template slot-scope="scope">
          <i class="el-icon-edit" @click="clearEditClick(scope.$index, scope.row)">
            <font color="#3F7CF">编辑</font>
          </i>
          <i class>&nbsp;</i>
          <i
            v-permission="FuncCode.cof_f_emp_del"
            class="el-icon-delete"
            @click="clearDeleteClick(scope.$index, scope.row)"
          >
            <font color="#3F7CF">删除</font>
          </i>
        </template>
      </el-table-column>
    </el-table>
    <Pagination
      :page-size="table.pageSize"
      :page-sizes="[10, 20, 50, 100, 200, 500, 1000]"
      :current-page="table.currentPage"
      :total="table.expertTotal"
      @size-change="expertChangePageSize"
      @current-change="expertChangePage"
    />
  </div>
</template>

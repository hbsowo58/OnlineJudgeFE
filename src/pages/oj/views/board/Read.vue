<template>
  <div style="margin:100px auto;">

    <el-button type="primary" @click="write">글쓰기</el-button>
    <el-table @row-click="detail" :data="data" style="width: 100%"
    >
      <el-table-column
      type="selection"
      width="50">
      </el-table-column>

      <el-table-column
      prop="id"
      label="번호"
      width="100">
      </el-table-column>

    <el-table-column
      prop="title"
      label="제목"
      width="745"
>
    </el-table-column>

    <el-table-column
      prop="created_by"
      label="작성자"
      width="150">
    </el-table-column>

    <el-table-column
      prop="created_time"
      label="작성날짜"
      width="215">
    </el-table-column>
  
    </el-table>

    <!-- <el-button type="danger">선택삭제</el-button> -->
    <!-- <el-button type="danger">숨기기</el-button> -->
    <el-button type="primary" @click="write" style="float:right; margin-bottom:20px;">글쓰기</el-button>

    <el-pagination
    style="text-align:center;"
    :page-size="20"
    :pager-count="11"
    layout="prev, pager, next"
    :total="1000">
    </el-pagination>
  </div>
</template>

<script>
import api from '@oj/api'
// import data from '../data'
export default {
  name: 'Read',
  data () {
    return {
      data: []
    }
  },
  async mounted () {
    const response = await api.getBoardList();
    const data = Object.entries(response).find(el => el[0] === "data");
    console.log(data);
    const result = data[1]['data']['results'];
    console.log(result);
    this.data = result;
    
    
  },
  methods: {
    write () {
      this.$router.push({
        path: 'create'
      })
    },
    detail (e) {
      console.log(e)
      console.log('hi')
      this.$router.push(`/detail/${e.id}`)
    }
  }
}
</script>

<style>

</style>
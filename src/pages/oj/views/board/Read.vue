<template>
  <el-container class="board-container">
    <el-header>Q&A</el-header>
    <el-main>
      <el-table border @cell-click="detail" :data="data">
        <!--
          <el-table-column
          type="selection"
          width="80px"
          >
          </el-table-column>
        -->
        <el-table-column type="index" align="center" width="140px">
        </el-table-column>

        <el-table-column prop="title" label="제목" align="center">
        </el-table-column>

        <el-table-column
          prop="real_name"
          label="작성자"
          width="200px"
          align="center"
        >
        </el-table-column>

        <el-table-column
          prop="created_time"
          label="작성날짜"
          width="200px"
          align="center"
        >
          <template slot-scope="scope">
            {{ scope.row.created_time | localtime }}
            <!--
              <a href='#' @click="clickMethod(props.row.data)" >{{props.row.maskingData}}</a>
            -->
          </template>
        </el-table-column>
        <!-- {{time.utcToLocal}} -->
      </el-table>
    </el-main>

    <!-- <el-button type="danger">선택삭제</el-button> -->
    <!-- <el-button type="danger">숨기기</el-button> -->
    <el-footer>
      <div class="page-wrapper">
        <el-button type="primary" @click="write">글쓰기</el-button>
        <el-pagination
          class="page"
          layout="prev, pager, next"
          :page-size="10"
          :current-page.sync="currentPage"
          @current-change="test"
          :total="total"
        >
        </el-pagination>
      </div>
    </el-footer>
  </el-container>
</template>

<script>
import time from "@/utils/time";
import api from "@oj/api";
// import data from '../data'
export default {
  name: "Read",
  data() {
    return {
      total: 0,
      data: [],
      currentPage: 1
    };
  },
  async mounted() {
    // const response = await api.getBoardList({limit:10, offset:30});
    this.test();
  },
  methods: {
    async test() {
      const response = await api.getBoardList({
        limit: 10,
        offset: (this.currentPage - 1) * 10
      });
      const data = Object.entries(response).find(el => el[0] === "data");
      console.log(data);
      this.total = data[1]["data"]["total"];
      const result = data[1]["data"]["results"];
      console.log(result);
      this.data = result;
    },
    write() {
      this.$router.push({
        path: "create"
      });
    },
    detail(id, column, cell, event) {
      console.log(column.property);
      if (column.property === "title") {
        this.$router.push(`/board/${id.id}`);
      }
      console.log(id);
      console.log(column);
      console.log(cell);
      console.log(event);
    }
  }
};
</script>

<style lang="less" scoped>
.el-header{
  font-size: 21px;
  font-weight: 500;
  padding: 19px 31px;
  height: 100% !important;
}
.board-container{
  // width: 80%;
  margin:0 auto;
  background: white;
}
.el-table td {
  padding: 6px 0 !important;
}
.page-wrapper {
  
  display: flex;
  justify-content: space-between;
}
</style>

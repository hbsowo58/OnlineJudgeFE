<template>
  <el-container class="board-container">
    <el-header>질문게시판</el-header>
    <el-main>
      <el-table
        border
        @cell-click="detail"
        :data="data"
        empty-text="검색한 정보가 없습니다"
      >
        <!--
          <el-table-column
          type="selection"
          width="80px"
          >
          </el-table-column>
        -->
        <!-- <el-table-column type="index" align="center" width="140px"> -->
        <!-- </el-table-column> -->
        <el-table-column prop="id" align="center" width="140px">
        </el-table-column>

        <el-table-column prop="title" label="제목" align="left">
          <template slot-scope="scope">
            <span style="padding-left:30px;"
              >{{ scope.row.title }} <span v-if="scope.row.board.length">[{{ scope.row.board.length }}]</span></span
            >
          </template>
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
            {{ scope.row.created_time && toLocal(scope.row.created_time) }}
            <div>
              ({{
                scope.row.created_time && toLocalTime(scope.row.created_time)
              }})
            </div>
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
        <div class="serach-wrapper">
          <el-input
            v-model="keyword"
            prefix-icon="el-icon-search"
            placeholder="제목 / 내용 / 이름 검색"
          ></el-input>
          <el-button type="primary" @click="getBoardList">검색</el-button>
        </div>
        <el-pagination
          class="page"
          layout="prev, pager, next"
          :page-size="10"
          :current-page.sync="currentPage"
          @current-change="getBoardList"
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
import { mapState } from "vuex";
// import data from '../data'
export default {
  name: "Read",
  data() {
    return {
      total: 0,
      data: [],
      currentPage: 1,
      keyword: ""
    };
  },
  async mounted() {
    // const response = await api.getBoardList({limit:10, offset:30});
    this.getBoardList();
    // const data = this.user.profile.user.email;
    // console.log(data);
    // if(data.indexOf() < -1){
    //   this.$router.push("/");
    // }
  },
  computed: {
    ...mapState(["user"])
  },
  methods: {
    toLocalTime(data) {
      const result = time.utcToLocal(data, "HH시 mm분");
      return result;
    },
    toLocal(data) {
      const result = time.utcToLocal(data, "YYYY년 M월 D일");

      return result;
    },
    async getBoardList() {
      const response = await api.getBoardList({
        limit: 10,
        offset: (this.currentPage - 1) * 10,
        keyword: this.keyword
      });
      const data = Object.entries(response).find(el => el[0] === "data");
      console.log(data);
      this.total = data[1]["data"]["total"];
      const result = data[1]["data"]["results"];
      console.log(result[0].board.length);
      this.data = result;
    },
    write() {
      this.$router.push({
        path: "create"
      });
    },
    detail(id, column, cell, event) {
      // console.log(column.property);
      if (column.property === "title") {
        this.$router.push(`/board/${id.id}`);
      }
      // console.log(id);
      // console.log(column);
      // console.log(cell);
      // console.log(event);
    }
  }
};
</script>

<style lang="less" scoped>
.el-header {
  font-size: 21px;
  font-weight: 500;
  padding: 19px 31px;
  height: 100% !important;
}
.board-container {
  // width: 80%;
  margin: 0 auto;
  background: white;
}
.el-table td {
  padding: 6px 0 !important;
}
.serach-wrapper {
  display: flex;
  align-items: center;
  width: 40%;
}
.page-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.board-container > * {
  width: 80%;
  margin: 0 auto;
}
.for-align {
  width: 80%;
  margin: 0 auto;
}
</style>

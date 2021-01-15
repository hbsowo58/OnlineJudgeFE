<template>
  <div>
    <!-- {{board['board']}} -->
    <el-container>
      <el-header>
        <div type="flex">
          <div class="detail_title">질의응답</div>
          <div class="detail_subtitle">
            <span> 작성자 : {{ board['board'].real_name }} </span>
            <span> 작성일 : {{ board['board'].created_time }} </span>
            <span> 조회(임시) </span> <span> 댓글(임시) </span>
          </div>
        </div>
      </el-header>
      <hr />
      <el-main>
        <div class="detail_content">
          <div class="detail_optionbtn">
            <el-button @click="list">목록</el-button>
            <el-button @click="updateData">수정</el-button>
             <el-button @click="deleteBoard">삭제</el-button>
          </div>
          <p>{{ board['board'].content }}</p>
        </div>
      </el-main>
      <el-footer>
        <div v-for="c in board['board'].comments" :key="c.id" class="detail_comment">
          <div class="board_comment-wrapper">
            <div class="board_comment">
              <span class="detail_comment_font">{{ c.real_name }} </span>
              <span class="detail_comment_font"> {{ c.created_time }}</span>
              <el-button style="margin-left:auto;">수정/삭제</el-button>
            </div>
            <div>{{ c.comment }}</div>
          </div>
        </div>
        <Comment />
      </el-footer>
    </el-container>
    <!--
      {{data}}
      <div>{{data.writer}}</div>
      <div>{{data.title}}</div>
      <div>{{data.content}}</div>
      <button @click="updateData">수정</button>
      <button @click="deleteData">삭제</button>
    -->
  </div>
</template>

<script>
import api from "@oj/api";
import Comment from "./Comment";
import {mapState,mapMutations,mapActions }from "vuex"
// import data from '../data'
export default {
  name: "Detail",
  components: { Comment },
  data() {
    const index = this.$route.params.board_id;
    return {
      // data: data[index],
      data: [],
      index: index
    };
  },
  computed:{
    ...mapState(['board'])
  }
  ,
  async mounted() {
    // await this.getData();
    const reuslt = await this.getBoard(this.$route.params["board_id"])
    console.log(this.board.board);
    // api.postBoard("치킨","jmt", loginId);
    // const test1 = await api.putBoard("믿고있었다고", "젠장!!!",parameter);
    // console.log(test1);
    // const test2 = await api.deleteBoard(parameter)
    // console.log(test2);
    // const test3 = await api.postComment("온유씨", parameter, loginId);
    // console.log(test3);
    // const test4 = await api.putComment(2, "응 아니야~")
    // console.log(test4)
    // const test5 = await api.deleteComment('2')
    // console.log(test5)
    // console.log(this.board);
  },
  methods: {
    ...mapActions(['getBoard']),
    // ...mapMutations(['POST_COMMENT']),
    async getData(){
        const parameter = this.$route.params["board_id"];
        // console.log(parameter);
        // this.postBoard(parameter);
        // const response = await api.getBoardDetail(parameter);
        // const data = Object.entries(response).find(el => el[0] === "data");
        // const result = data[1]["data"];
        // console.log(result);
        // this.POST_COMMENT(result);
    },
    async deleteBoard() {
      await api.deleteBoard(this.$route.params["board_id"]);
      this.$router.push({
        path: "/board"
      });
    },
    updateData() {
      this.$router.push(`/create/${this.$route.params["board_id"]}`);
    },
    list() {
      this.$router.push({
        path: "/board"
      });
    },
    write() {
      this.$router.push({
        path: "/create"
      });
    },
  }
};
</script>
<style>
.el-header {
  background: white;
  margin-top: 50px;
}
.el-footer {
  height: 100%;
}

.el-aside {
  /* background-color: #d3dce6;
  color: #333;
  text-align: center; */
}

.el-main {
  background: white;
}
.el-footer {
  height: 100% !important;
}

body > .el-container {
  margin-bottom: 40px;
}

.detail_title {
  /* background:red; */
  text-align: left;
  font-size: 16px;
  margin-bottom: 10px;
}

.detail_subtitle {
  margin-right: 10px;
  text-align: left;
}

.detail_content {
  /* position: relative; */
  /* background: red; */
  min-height: 100px;
  font-size: 20px;
  width: 100%;
}
.detail_optionbtn {
  /* position: absolute; */
  /* top:0;
  right:0; */
  /* background:blue; */
  text-align: right;
}

.detail_comment {
  border: 1px solid #000;
  background-color: #e9eef3;
  margin-top: 10px;
  margin-bottom: 20px;
  /* background:red; */
}

.detail_comment_font {
  font-size: 16px;
  margin-right: 20px;
}

.board_comment-wrapper {
  display: flex;
  flex-direction: column;
}
.board_comment {
  display: flex;
}
</style>

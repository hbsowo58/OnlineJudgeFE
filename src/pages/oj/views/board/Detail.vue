<template>
  <div>
    
    <!-- {{board['board']}} -->
    <el-container>
      <el-header>
      <!-- Q&A -->
        <div class="detail_title">Q&A</div>
        <div class="detail-header-wrapper">
          <div></div>
          <div class="detail_title" style="text-align:center;margin-left: 15%;">{{board["board"].title}}</div>
          <div class="detail_subtitle">
            <span> 작성자 : {{ board["board"].real_name }} </span>
            <span> 작성일 : {{ toLocal(board["board"].created_time) }} </span>
            <!-- <span> 조회(임시) </span> <span> 댓글(임시) </span> -->
          </div>
        </div>
      </el-header>
      <hr />
      <el-main>
        <div class="detail_content">
          <div class="detail_optionbtn">
            <el-button @click="list">목록</el-button>

            <el-button @click="updateData" v-if="user.profile.user.id ===board['board'].created_by">수정</el-button>
            <el-button @click="deleteBoard" v-if="user.profile.user.id ===board['board'].created_by">삭제</el-button>
          </div>
          <p>{{ board["board"].content }}</p>
        </div>
      </el-main>
      <el-footer>
        <div
          v-for="c in board['board'].comments"
          :key="c.id"
          class="detail_comment"
        >
          <div class="board_comment-wrapper">
            <div class="board_comment">
              <span class="detail_comment_font">{{ c.real_name }} </span>
              <span class="detail_comment_font">
                {{ toLocal(c.created_time) }}</span
              >
              <div style="margin-left:auto;">
                <el-button
                  class="detail_comment_button"
                  @click="chageflag(c.id);"
                  v-if="commentIndex !== c.id && user.profile.user.id === c.created_by"
                  >수정</el-button
                >
                 <!-- v-if="user.profile.user.id ===board['board'].created_by" -->
                <el-button
                  class="detail_comment_button"
                  @click="deleteComment(c.id);"
                  v-if="user.profile.user.id === c.created_by"
                  >삭제</el-button
                >
              </div>
            </div>
            <div v-if="flag && commentIndex === c.id">
              <el-input
                v-model="comment"
                placeholder="수정할 값을 입력하세요"
              ></el-input>
              <el-button
                class="detail_comment_button"
                @click="changeComment(c.id);"
                >변경</el-button
              >
            </div>
            <div>{{ c.comment }}</div>
          </div>
        </div>
        <Comment />
      </el-footer>
    </el-container>
  </div>
</template>

<script>
import time from "@/utils/time";
import api from "@oj/api";
import Comment from "./Comment";
import { mapState, mapMutations, mapActions } from "vuex";
// import data from '../data'
export default {
  name: "Detail",
  components: { Comment },
  data() {
    const index = this.$route.params.board_id;
    return {
      // data: data[index],
      data: [],
      index: index,
      flag: false,
      commentIndex: "",
      input: "",
      comment: ""
    };
  },
  computed: {
    ...mapState(["board", "user"])
  },
  async mounted() {
    console.log(time);
    // await this.getData();
    const reuslt = await this.getBoard(this.$route.params["board_id"]);
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
    ...mapActions(["getBoard"]),

    toLocal(data) {
      const result = time.utcToLocal(data, "YYYY-MM-D");

      return result;
    },
    // ...mapMutations(['POST_COMMENT']),
    async getData() {
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
    async deleteComment(id) {
      const hello1 = await api.deleteComment(id);
      const reuslt = await this.getBoard(this.$route.params["board_id"]);
    },
    chageflag(id) {
      this.flag = true;
      this.commentIndex = id;
      // await api.putComment(comment_id, content)
    },
    async changeComment(comment_id) {
      // console.log(this.comment)
      await api.putComment(comment_id, this.comment);
      const reuslt = await this.getBoard(this.$route.params["board_id"]);
      this.flag = false;
      this.comment = "";
      this.commentIndex = "";
    }
  }
};
</script>
<style scoped>
.el-header {
  background: white;
  padding: 40px 40px;
  height: 100% !important;
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
  min-height: 30vh;
}
.el-footer {
  height: 100% !important;
  padding: 0 !important;
}

.el-input input {
  border-top: 1px solid #eeeeee;
}
body > .el-container {
  margin-bottom: 40px;
}
.detail-header-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 14px;
}

.detail_title {
  /* background:red; */
  /* text-align: left; */

  font-size: 21px;
  font-weight: 500;
  margin-bottom: 10px;
}

.detail_subtitle {
  margin-right: 10px;
  text-align: left;
  display: flex;
}
.detail_subtitle > span {
  margin-left: 30px;
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

.comment {
}
.detail_comment {
  border: 1px solid #eeeeee;
  background-color: white;
  padding: 10px 24px;
  /* margin-top: 10px; */
  /* margin-bottom: 20px; */
  /* background:red; */
}
.detail_comment_button {
  margin-left: auto;
  font-size: 15px;
}
.detail_comment_font {
  font-size: 15px;
  margin-right: 20px;
}

.board_comment-wrapper {
  display: flex;
  flex-direction: column;
}
.board_comment {
  display: flex;
}
.board_comment button {
  padding: 11px !important;
}
</style>

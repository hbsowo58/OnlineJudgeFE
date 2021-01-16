<template>
  <div class="background:white;">
    <el-input v-model="title" placeholder="제목을 입력해주세요" />
    <el-input
      type="textarea"
      autosize
      placeholder="내용을 입력해주세요"
      v-model="content"
    />

    <el-button v-if="index" @click="update">수정</el-button>
    <el-button v-else @click="write">작성</el-button>

    <!--
      <el-button @click="index !== undefined ? update() : write()">{{index !== undefined ? "수정" : "작성"}}</el-button>
    -->
  </div>
</template>

<script>
import api from "@oj/api";
import data from "../data";
import { mapActions, mapState } from "vuex";
export default {
  name: "Create",
  data() {
    const index = this.$route.params.contentId;
    return {
      data: "",
      index: "",
      title: "",
      content: "",
      route: ""
    };
  },
  computed: {
    ...mapState(["board"]),
    ...mapState(["user"])
  },
  async mounted() {
    if (this.$route.params["board_id"]) {
      // console.log(this.board);
      this.index = this.$route.params["board_id"];
      await this.getBoard(this.$route.params["board_id"]);
      console.log(this.user);
      console.log(this.board.board.created_by);
      console.log(this.user.profile.user.id);
      // console.log(this.board);
      this.title = this.board.board.title;
      this.content = this.board.board.content;
      if (this.board.board.created_by !== this.user.profile.user.id) {
        this.$router.push({
          path: "/board"
        });
      }
    }
    // 수정글 작성시 해당 로그인 권한과 맞춰서 권한이 맞지 않을경우 수정이 불가능하게 한다
  },
  methods: {
    ...mapActions(["getBoard"]),
    async write() {
      await api.postBoard(this.title, this.content, this.user.profile.user.id);
      this.$router.push({
        path: "/board"
      });
    },
    async update() {
      // 수정 방지 처리

      if (this.board.board.created_by === this.user.profile.user.id) {
        const test = await api.putBoard(
          this.title,
          this.content,
          this.$route.params["board_id"]
        );
      }
      this.$router.push({
        path: `/board/${this.$route.params["board_id"]}`
      });
    }
  }
};
</script>

<style lang="less">
.el-input input {
  margin-top: 100px;
  margin-bottom: 20px;
}

// .el-textarea{
//   background: red;
// }

.el-textarea__inner {
  margin-bottom: 20px;
  min-height: 100px !important;
  // background: red;
}
</style>

<template>
  <div>
    <el-container>
      <el-header>
        <div type="flex">
          <div>질의응답</div>
          <span> {{ data.real_name }} </span> <span> {{ data.created_time }} </span>
          <span> 조회(임시) </span> <span> 댓글(임시) </span>
        </div>
      </el-header>
      <el-main>
        <div>
          {{ data.content }}
          <el-button>목록</el-button>
          <el-button>답변</el-button>
          <el-button>글쓰기</el-button>
        </div>
      </el-main>
      <el-footer style="height:100%">
        <!-- {{ data.comments }} -->
        <div v-for="c in data.comments" :key="c.id">
          {{c.real_name}} : {{c.comment}}
        </div>
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
// import data from '../data'
export default {
  name: "Detail",
  data() {
    const index = this.$route.params.board_id;
    return {
      // data: data[index],
      data: [],
      index: index
    };
  },
  async mounted() {
    const parameter = this.$route.params["board_id"];
    console.log(parameter);
    const response = await api.getBoardDetail(parameter);
    const data = Object.entries(response).find(el => el[0] === "data");
    const result = data[1]["data"];
    console.log(result);
    this.data = result;
    const loginId = this.$store.state.user.profile['user'].id
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
  },
  methods: {
    deleteData() {
      data.splice(this.index, 1);
      this.$router.push({
        path: "/board"
      });
    },
    updateData() {
      this.$router.push({
        name: "Create",
        params: {
          contentId: this.index
        }
      });
    }
  }
};
</script>
<style>
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  height: 100%;
  
}

.el-aside {
  background-color: #d3dce6;
  color: #333;
  text-align: center;
  
}

.el-main {
  background-color: #e9eef3;
  color: #333;
  /* text-align: center; */
  
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  
}

.el-container:nth-child(7) .el-aside {
  
}
</style>

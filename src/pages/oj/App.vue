<template>
  <div>
  <NavBar></NavBar>

    <div class="visual">
      <span class="test">세 가지를 꼭 지켜주세요.</span>

      <span class="test1">
      하나, 모든 소스코드는 처음부터 작성하기
      <br>
      두울, Trace로 끝가지 버그 잡아내기
      <br>
      세엣, 질문은 적극적이고 구체적으로
      </span>
      <img src="../../assets/visualimage.jpg"/>
    </div>
    <div class="content-app">
    
    <transition name="fadeInUp" mode="out-in">
        <router-view></router-view>
      </transition>
      <div class="footer">
        <p v-html="website.website_footer"></p>
        <p>Powered by <a href="https://github.com/QingdaoU/OnlineJudge"></a>
          <span v-if="version">&nbsp; Version: {{ version }}</span>
        </p>
      </div>
    </div>
    <BackTop></BackTop>
  </div>
</template>

<script>
  import { mapActions, mapState } from 'vuex'
  import NavBar from '@oj/components/NavBar.vue'

  export default {
    name: 'app',
    components: {
      NavBar
    },
    data () {
      return {
        version: process.env.VERSION
      }
    },
    created () {
      try {
        document.body.removeChild(document.getElementById('app-loader'))
      } catch (e) {
      }
    },
    mounted () {
      this.getWebsiteConfig()
    },
    methods: {
      ...mapActions(['getWebsiteConfig', 'changeDomTitle'])
    },
    computed: {
      ...mapState(['website'])
    },
    watch: {
      'website' () {
        this.changeDomTitle()
      },
      '$route' () {
        this.changeDomTitle()
      }
    }
  }
</script>

<style lang="less">

  * {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
  }

  a {
    text-decoration: none;
    background-color: transparent;
    &:active, &:hover {
      outline-width: 0;
    }
  }

  .visual{
    // background:red;
    // width:100%;
    // text-align:center;
    .test{
      top:250px;
      left:200px;
      font-size:20px;
      // z-inedx:10px;
      position:absolute;
    }
    .test1{
      font-size:20px;
      // z-inedx:10px;
      position:absolute;
      top:300px;
      left:200px;
    }
    img{
      width:100%;
    }
  }


  @media screen and (max-width: 1200px) {
  .content-app {
    margin-top: 160px;
    padding: 0 2%;
  }
}

@media screen and (min-width: 1200px) {
  .content-app {
    margin-top: 80px;
    padding: 0 2%;
  }
}

  .footer {
    margin-top: 20px;
    margin-bottom: 10px;
    text-align: center;
    font-size: small;
  }

  .fadeInUp-enter-active {
    animation: fadeInUp .8s;
  }


</style>

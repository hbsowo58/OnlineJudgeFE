<template>
  <div class="t">
    <div class="visual">
      <span class="visual_title">
      <span>세 가지</span>를 꼭 지켜주세요.
      </span>
  
      <span class="visual_content">
      하나, 모든 소스코드는 처음부터 작성하기
      <br>
      두울, Trace로 끝가지 버그 잡아내기
      <br>
      세엣, 질문은 적극적이고 구체적으로
      </span>
      <img src="../../../../assets/visualimage.jpg"/>
      </div>
  <Row type="flex" justify="space-around">
  <Col :span="22">
  <Announcements class="announcement"></Announcements>
  <br>
  
  <!-- 3차수
  <panel shadow v-if="contests.length" class="contest">
  <div slot="title">
  <Button type="text"  class="contest-title" @click="goContest">{{contests[index].title}}</Button>
  </div>
  <Carousel v-model="index" trigger="hover" autoplay :autoplay-speed="6000" class="contest">
  <CarouselItem v-for="(contest, index) of contests" :key="index">
  <div class="contest-content">
  <div class="contest-content-tags">
  <Button type="info" shape="circle" size="small" icon="calendar">
  {{contest.start_time | localtime('YYYY-M-D HH:mm') }}
  </Button>
  <Button type="success" shape="circle" size="small" icon="android-time">
  {{getDuration(contest.start_time, contest.end_time)}}
  </Button>
  <Button type="warning" shape="circle" size="small" icon="trophy">
  {{contest.rule_type}}
  </Button>
  </div>
  <div class="contest-content-description">
  <blockquote v-html="contest.description"></blockquote>
  </div>
  </div>
  </CarouselItem>
  </Carousel>
  </panel>
  -->
  </Col>
  </Row></div>
</template>

<script>
  import Announcements from './Announcements.vue'
  import api from '@oj/api'
  import time from '@/utils/time'
  import { CONTEST_STATUS } from '@/utils/constants'

  export default {
    name: 'home',
    components: {
      Announcements
    },
    data () {
      return {
        contests: [],
        index: 0
      }
    },
    mounted () {
      let params = {status: CONTEST_STATUS.NOT_START}
      api.getContestList(0, 5, params).then(res => {
        this.contests = res.data.data.results
      })
    },
    methods: {
      getDuration (startTime, endTime) {
        return time.duration(startTime, endTime)
      },
      goContest () {
        this.$router.push({
          name: 'contest-details',
          params: {contestID: this.contests[this.index].id}
        })
      }
    }
  }
</script>
<style lang="less" scoped>
  .contest {
    &-title {
      font-style: italic;
      font-size: 21px;
    }
    &-content {
      padding: 0 70px 40px 70px;
      &-description {
        margin-top: 25px;
      }
    }
  }

  .visual{
    margin-bottom:80px;
    .visual_title{
    top:300px;
    left:200px;
    font-size:20px;
    // z-inedx:10px;
    position:absolute;
    font-family: 'Noto Sans KR', sans-serif;
   }
  .visual_content{
    font-size:20px;
    // z-inedx:10px;
    position:absolute;
    top:350px;
    left:200px;
     font-family: 'Noto Sans KR', sans-serif;
   }
   img{
     width:100%;
   }
  }

  .announcement {
    margin-bottom:30px;
  }
</style>

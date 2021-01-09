<template>
  <div id="header">
    <Menu theme="light" mode="horizontal" @on-select="handleRoute" :active-name="activeMenu" class="oj-menu">

      <img src="../../../assets/logo_01.svg" class="logo"/>
    
      <Menu-item name="/">
        <Icon type="home"></Icon>
        <span class="menu-title">{{$t('m.Home')}}</span>
      </Menu-item>
      
      <Submenu name="/category" v-if="isSuperAdmin">
        <template slot="title">
          <Icon type="ios-list-outline"></Icon>
          <span class="menu-title">{{$t('m.Category')}}</span>
        </template>
        <Menu-item name="/category">
          {{$t('m.G1_name')}}
        </Menu-item>
        <Menu-item name="/category">
          {{$t('m.G2_name')}}
        </Menu-item>
        <Menu-item name="/category">
          {{$t('m.G3_name')}}
        </Menu-item>
        <Menu-item name="/category">
          {{$t('m.G4_name')}}
        </Menu-item>
        <Menu-item name="/category">
          {{$t('m.G5_name')}}
        </Menu-item>
        <Menu-item name="/category">
          {{$t('m.G6_name')}}
        </Menu-item>        
      </Submenu>

      <Menu-item name="/contest">
        <Icon type="person-stalker"></Icon>
        <span class="menu-title">{{$t('m.Contests')}}</span>
      </Menu-item>
      
      <Menu-item name="/problem" v-if="isSuperAdmin">
        <Icon type="ios-keypad"></Icon>
        <span class="menu-title">{{$t('m.NavProblems')}}</span>
      </Menu-item>
      
      <Menu-item name="/status" v-if="isSuperAdmin">
        <Icon type="ios-pulse-strong"></Icon>
        <span class="menu-title">{{$t('m.NavStatus')}}</span>
      </Menu-item>
      
      <Submenu name="rank" v-if="isSuperAdmin">
        <template slot="title">
          <Icon type="podium"></Icon>
          <span class="menu-title">{{$t('m.Rank')}}</span>
        </template>
        <Menu-item name="/acm-rank">
          {{$t('m.ACM_Rank')}}
        </Menu-item>
        <Menu-item name="/oi-rank">
          {{$t('m.OI_Rank')}}
        </Menu-item>
      </Submenu>

      <Submenu name="about">
        <template slot="title">
          <Icon type="android-bulb"></Icon>
          <span class="menu-title">{{$t('m.About')}}</span>
        </template>
        <Menu-item name="/about">
          {{$t('m.Judger')}}
        </Menu-item>
        <Menu-item name="/FAQ">
          {{$t('m.FAQ')}}
        </Menu-item>
      </Submenu>
      <template v-if="!isAuthenticated">
        <div class="btn-menu">
          <Button type="ghost"
                  ref="loginBtn"
                  shape="circle"
                  @click="handleBtnClick('login')">{{$t('m.Login')}}
          </Button>
          <Button v-if="website.allow_register"
                  type="ghost"
                  shape="circle"
                  @click="handleBtnClick('register')"
                  style="margin-left: 5px;">{{$t('m.Register')}}
          </Button>
        </div>
      </template>
      <template v-else>
        <Dropdown class="drop-menu" @on-click="handleRoute" placement="bottom" trigger="click">
          <Button type="text" class="drop-menu-title">{{ user.username }}
            <Icon type="arrow-down-b"></Icon>
          </Button>
          <Dropdown-menu slot="list">
            <Dropdown-item name="/user-home">{{$t('m.MyHome')}}</Dropdown-item>
            <Dropdown-item name="/status?myself=1" v-if="isAdminRole || isSuperAdmin">{{$t('m.MySubmissions')}}</Dropdown-item>
            <Dropdown-item name="/setting/profile">{{$t('m.Settings')}}</Dropdown-item>
            <Dropdown-item v-if="isAdminRole" name="/admin">{{$t('m.Management')}}</Dropdown-item>
            <Dropdown-item divided name="/logout">{{$t('m.Logout')}}</Dropdown-item>
          </Dropdown-menu>
        </Dropdown>
      </template>
    </Menu>
    <Modal v-model="modalVisible" :width="400">
      <div slot="header" class="modal-title">{{$t('m.Welcome_to')}} {{website.website_name_shortcut}}</div>
      <component :is="modalStatus.mode" v-if="modalVisible"></component>
      <div slot="footer" style="display: none"></div>
    </Modal>
  </div>
</template>

<script>
  import { mapGetters, mapActions } from 'vuex'
  import login from '@oj/views/user/Login'
  import register from '@oj/views/user/Register'

  export default {
    components: {
      login,
      register
    },
    mounted () {
      this.getProfile()
    },
    methods: {
      ...mapActions(['getProfile', 'changeModalStatus']),
      handleRoute (route) {
        if (route && route.indexOf('admin') < 0) {
          this.$router.push(route)
        } else {
          window.open('/admin/')
        }
      },
      handleBtnClick (mode) {
        this.changeModalStatus({
          visible: true,
          mode: mode
        })
      }
    },
    computed: {
      ...mapGetters(['website', 'modalStatus', 'user', 'isAuthenticated', 'isAdminRole', 'isSuperAdmin']),
      // 跟随路由变化
      activeMenu () {
        return '/' + this.$route.path.split('/')[1]
      },
      modalVisible: {
        get () {
          return this.modalStatus.visible
        },
        set (value) {
          this.changeModalStatus({visible: value})
        }
      }
    }
  }
</script>

<style lang="less" scoped>
  #header {
    min-width: 300px;
    position: fixed;
    top: 0;
    left: 0;
    height: auto;
    width: 100%;
    z-index: 1000;
    background-color: #fff;
    box-shadow: 0 1px 5px 0 rgba(0, 0, 0, 0.1);

    .oj-menu {
      background: #fdfdfd;
      .menu-title{
        // background:red;
        font-size:16px;
      }
    }

    .logo {
      // margin-left: 2%;
      // margin-right: 2%;
      // font-size: 20px;
      // display:block;
      // height:20px;
      padding:14px 7px;
      float: left;
      margin:0px auto;
      // background:red;
      // line-height: 60px;
    }

    .drop-menu {
      float: right;
      margin-right: 30px;
      position: absolute;
      right: 10px;
      &-title {
        font-size: 18px;
      }
    }
    .btn-menu {
      font-size: 16px;
      float: right;
      margin-right: 10px;
    }
  }

  .modal {
    &-title {
      font-size: 18px;
      font-weight: 600;
    }
  }
</style>

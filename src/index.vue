<template>
        <div class="app-wrapper">
            <router-view class="r-box"></router-view>
            <!-- <tab-bar @tabTo="onTabTo"></tab-bar> -->
        </div>
        <!-- <div class="login-page" v-else>
            <login-page v-on:login="handleMessage" v-on:jumpTo="handleJump"></login-page>
        </div> -->
</template>

<script>
  var modal = weex.requireModule('modal');
  var storage = weex.requireModule('storage');
  var navigator = weex.requireModule('navigator');
  import util from './assets/util';
  import tabBar from './assets/components/tabBar.vue';
  import loginPage from './assets/views/login.vue';
  export default {
      name: 'App',
      data () {
          return {
              login: false
          }
      },
      components: {
          'tab-bar': tabBar,
          'login-page': loginPage
      },
      created () {
          util.initIconFont();
          var _self = this;
          //获取token
          // storage.getItem('token',event => {
          //   var localToken = event.data;
          //   if(localToken == 'undefined'){
          //     this.login = false;
          //     _self.$router.push('/login')              
          //   }else if(localToken != 'undefined'){
          //     this.GET('banners/list', localToken, res => {
          //         let result = res.data;
          //         if(result.code != 200){
          //           this.login = false;
          //           _self.$router.push('/login')
          //         }else if(result.code == 200){
          //           this.login = true;
          //           _self.$router.push('/home')
          //         }
          //     });
          //   }
          // })
      },
      methods: {
          onTabTo(_result){
              let _key = _result.data.key || '';
              this.$router && this.$router.push('/'+_key)
          },
          handleMessage(payload){
              this.login = payload.login;
              this.$router.push('/home')
          },
          handleJump(event){
              console.log(event.url);
              this.$router.push('/register')
          }
      }
  }

</script>

<style scoped>
    .app-wrapper{
        background-color: #ffffff;
    }
  .r-box{
      position: absolute;
      top:0;
      left: 0;
      right: 0;
      bottom: 0;
  }
</style>

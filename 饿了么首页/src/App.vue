<template>
  <div id="app">
      <transition name="tabbar">
          <tabbar v-show="show" style="position:fixed;">
            <tabbar-item selected link="/">
              <img slot="icon" src="./assets/img/index1.png">
              <span slot="label">外卖</span>
            </tabbar-item>
            <tabbar-item link="/find">
              <img slot="icon" src="./assets/img/index2.png">
              <span slot="label">发现</span>
            </tabbar-item>
            <tabbar-item link="/order">
              <img slot="icon" src="./assets/img/index3.png">
              <span slot="label">订单</span>
            </tabbar-item>
            <tabbar-item link="/my">
              <img slot="icon" src="./assets/img/index3.png">
              <span slot="label">我的</span>
            </tabbar-item>
          </tabbar>
        </transition>
          <router-view></router-view>   
  </div>
</template>

<script>
import {Tabbar,TabbarItem} from 'vux' 
export default {
  name: 'app',
  components: {
    Tabbar,TabbarItem
  },
  data(){
    return{
      animateType:'none',
      show: true
    }
  },
  watch: {
    // 监听路由的变化
    $route(to,from){
      // console.log(to);
      // console.log(from);
      var toLevel = to.path.split('/').length;
      var fromLevel = from.path.split('/').length;
      console.log('tolevel='+toLevel);
      // console.log('fromlevel='+fromLevel);

      if (toLevel == fromLevel) {
        // 平级切换
        this.animateType = 'level'
      }else if (toLevel > fromLevel) {
        // 进入下一级界面 1->2, 或者 2->3 
        this.animateType = 'in'
      }else{
        // 返回上一页 2->1  3->2
        this.animateType = 'out'
      }
      //  console.log(this.animateType);
      // 控制tabbar 是否显示
      if (toLevel > 2) {
        this.show = false
      } else {
        this.show = true;
      }
    }
  }
}
</script>

<style lang="less">
@import '~vux/src/styles/reset.less';


</style>

<template>
  <div class="doc">
    <div class="doc-nav">
      <ul class="main-menu">
        <li>
          <router-link :to="{path:'#base'}">基础</router-link>
          <ul class="nav-dropdown">
            <li>
              <router-link :to="{path:'#start'}">开始</router-link>
            </li>
            <li>
              <router-link :to="{path:'#dongtai'}">动态路由</router-link>
            </li>
            <li>
              <router-link :to="{path:'#qiantaoluyou'}">嵌套路由</router-link>
            </li>
          </ul>
        </li>
        <li>
          <router-link :to="{path:'#jinjie'}">进阶</router-link>
          <ul class="nav-dropdown">
            <li>
              <router-link :to="{path:'#jinjie1'}">导航钩子</router-link>
            </li>
            <li>
              <router-link :to="{path:'#jinjie2'}">路由元信息</router-link>
            </li>
            <li>
              <router-link :to="{path:'#jinjie3'}">过渡动效</router-link>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <div class="doc-view">
      <doc-view></doc-view>
    </div>
  </div>
</template>

<script>
  import DocView from '@/views/backend/doc-view'
  import TWEEN from 'tween.js'
  export default {
    components: {
      DocView
    },
    beforeRouteEnter(to,from,next) {
      next((vm) => {
        vm.animate(to)
      })
    },
    beforeRouteUpdate(to,from,next) {
      //console.log(to.hash);
      this.animate(to);
      next();
    },
    methods: {
      animate(to) {

        function animateFunc(time) { //去不断更新下面的函数，不然下面的函数只会执行一次的
          requestAnimationFrame(animateFunc);   //定时器，  animateFunc作为回调函数
          TWEEN.update(time)  //每次更新的时间
        }

        if(to.hash) {
          var el = document.getElementById(to.hash.slice(1));
          var doc = document.getElementsByClassName("doc")[0];

          if(el) {
            animateFunc(); 
            new TWEEN.Tween({
              number: doc.scrollTop   //起始位置
            })
              .to({
                  number: el.offsetTop  //目标位置
                },500)
              .onUpdate(function() {
                doc.scrollTop = this.number.toFixed(0) //当前位置更新页面动画到当前位置
              })
              .start()  //动画开始
          }
        }
      }
    }
  }
</script>
<style>

</style>

<template>
  <div class="left_nav">
      <ul>
          <li>
              <i class="icon iconfont icon-goumai"></i>
              <div>收银</div>
          </li>
          <li>
              <i class="icon iconfont icon-dianpu"></i>
              <div>店铺</div>
          </li>
          <li>
              <i class="icon iconfont icon-hanbao"></i>
              <div>食品</div>
          </li>
          <li>
              <i class="icon iconfont icon-zuanshi"></i>
              <div>会员</div>
          </li>
          <li>
              <i class="icon iconfont icon-tongji"></i>
              <div>统计</div>
          </li>
          <li>
              <i class="icon iconfont icon-shezhi"></i>
              <div>设置</div>
          </li>
      </ul>
  </div>
</template>

<script>
export default {
  name: 'left_nav'
}
</script>

<style>
ul,li {
    margin: 0;
    padding: 0;
}
.left_nav {
    width: 5%;
    height: 100%;
    float: left;
    background: rgb(98, 160, 241);
    color: #FFF;
    font-size: 14px;
}
.left_nav ul {
    width: 100%;
    height: 100%;
}
.left_nav ul li {
    width: 100%;
    height: 8%;
    list-style: none;
    float: left;
    text-align: center;
    padding-top: 80%;
    border-bottom: 1px solid #FFF;
}

</style>


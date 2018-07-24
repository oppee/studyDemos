<template>
  <div class="left-nav">
    <ul>
      <li>
        <a href="#">
          <i class="iconfont icon icon-04"></i>
          <div>收银</div>
        </a>
      </li>
      <li>
        <a href="#">
          <i class="iconfont icon icon-03"></i>
          <div>店铺</div>
        </a>
      </li>
      <li>
        <a href="#">
          <i class="iconfont icon icon-02"></i>
          <div>商品</div>
        </a>
      </li>
      <li>
        <a href="#">
          <i class="iconfont icon icon-01"></i>
          <div>会员</div>
        </a>
      </li>
      <li>
        <a href="#">
          <i class="iconfont icon icon-05"></i>
          <div>统计</div>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'leftNav',
  data(){
    return {

    }
  }
};
</script>

<style scoped>
.left-nav {
  height: 100vh;
  width: 60px;
  background-color: #ff7100;
  text-align: center;
  font-size: 16px;
  color: #fff;
}
.left-nav ul li a{
  display: block;
  text-decoration: none;
  color: #fff;
  padding: 18px 0;
}
</style>
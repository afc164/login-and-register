<template>
  <div>
    <h3>所有注册用户</h3>
    <ul>
      <li v-for="item in list">{{item.username}}</li>
    </ul>
  </div>
</template>
<script type="text/javascript">

</script>
<style>
  ul{
    padding:0;
  }
  ul li{
    list-style: none;
  }
</style>

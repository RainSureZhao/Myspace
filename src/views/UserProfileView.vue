<template>
    <ContentBase>
      <div class="row">
        <div class="col-3">用户信息</div>
        <div class="col-9">帖子列表</div>
      </div> 
    </ContentBase>
  </template>
  
  <script>
  import ContentBase from '../components/ContentBase.vue'
  
  export default {
    name: 'UserProfile',
    components: {
      ContentBase,
  }
  }
  </script>
  
  <style scoped>
  .container {
    margin-top: 20px;
  }
  </style>
  
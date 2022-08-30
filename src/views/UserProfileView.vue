<template>
    <ContentBase>
      <div class="row">
        <div class="col-3">
          <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user">
          </UserProfileInfo>
          <UserProfileWrite @post_a_post="post_a_post"   v-if="is_me">
          </UserProfileWrite>
        </div>
        <div class="col-9">
          <UserProfilePosts :posts="posts" :user="user" @delete_a_post="delete_a_post">
          </UserProfilePosts>
        </div>
      </div> 
    </ContentBase>
  </template>
  
<script>
import ContentBase from '../components/ContentBase.vue'
import UserProfileInfo from '@/components/UserProfileInfo.vue';
import UserProfilePosts from '@/components/UserProfilePosts.vue';
import { reactive } from 'vue';
import UserProfileWrite from '@/components/UserProfileWrite.vue';
import { useRoute } from 'vue-router';
import $ from 'jquery';
import { useStore } from 'vuex';
import { computed } from 'vue';

export default {
  name: 'UserProfile',
  components: {
    ContentBase,
    UserProfileInfo,
    UserProfilePosts,
    UserProfileWrite
},
  setup() {
    const route = useRoute();
    const userId = parseInt(route.params.userId);
    console.log(userId);
    const store = useStore();
    const user = reactive({});
    const posts = reactive({});
    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/getinfo/",
      type: "GET",
      data: {
        user_id: userId,
      },
      headers: {
        'Authorization': "Bearer " + store.state.user.access,
      },
      success(resp) {
        user.id = resp.id;
        user.username = resp.username;
        user.photo = resp.photo;
        user.followerCount = resp.followerCount;
        user.is_followed = resp.is_followed;
      }
    });
    
    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/post/",
      type: "GET",
      data: {
        user_id: userId,
      },
      headers: {
        'Authorization': "Bearer " + store.state.user.access,
      },
      success(resp) {
        posts.count = resp.length;
        posts.posts = resp;
      }
    });

    const follow = () => {
      if(user.is_followed) return;
      user.is_followed = true;
      user.followerCount ++;
    };
    const unfollow = () => {
      if(!user.is_followed) return;
      user.is_followed = false;
      user.followerCount --;
    };

    const is_me = computed(() => userId === store.state.user.id);


    const post_a_post = (content) => {
      posts.count ++;
      posts.posts.unshift({
        id: posts.count,
        userId: 1,
        content: content,
      });
    };
    
    const delete_a_post = post_id => {
      console.log(post_id);
      posts.posts = posts.posts.filter(post => post.id !== post_id);
      posts.count = posts.posts.length;
    }

    return {
      user,
      follow,
      unfollow,
      posts,
      post_a_post,
      delete_a_post,
      is_me
    }
  }
}

  
</script>
  
  <style scoped>
  .container {
    margin-top: 20px;
  }
  </style>
  
<template>
    <ContentBase>
      <div class="row">
        <div class="col-3">
          <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user">
          </UserProfileInfo>
          <UserProfileWrite @post_a_post="post_a_post">
          </UserProfileWrite>
        </div>
        <div class="col-9">
          <UserProfilePosts :posts="posts">
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
    const userId = route.params.userId;
    console.log(userId);

    const user = reactive({
      id: 1,
      username: "Rain Sure",
      lastName: "Zhao",
      firstName: "Runshuo",
      followerCount: 0,
      is_followed: false,
    });

    const posts = reactive({
      count: 3,
      posts: [
        {
          id: 1,
          userId: 1,
          content: "今天上了编译原理，真开心! ",
        },
        {
          id: 2,
          userId: 1,
          content: "今天上了软件工程，更开心了! ",
        },
        {
          id: 3,
          userId: 1,
          content: "今天上了AcWing, 开心极了! ",
        },
      ]
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

    const post_a_post = (content) => {
      posts.count ++;
      posts.posts.unshift({
        id: posts.count,
        userId: 1,
        content: content,
      });
    };

    return {
      user,
      follow,
      unfollow,
      posts,
      post_a_post,
    }
  }
}

  
</script>
  
  <style scoped>
  .container {
    margin-top: 20px;
  }
  </style>
  
<template>
    <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-3 img-field">
                    <img class="img-fluid " :src="user.photo" alt="">
                </div>
                <div class="col-9">
                    <div class="username"> {{user.username}} </div>
                    <div class="fans">粉丝: {{user.followerCount}}</div>
                    <button @click="follow" v-if="!user.is_followed" type="button" class="btn btn-success btn-sm">关注</button>
                    <button @click="unfollow" v-if="user.is_followed"  type="button" class="btn btn-secondary btn-sm">取消关注</button>
                </div>
            </div>
        </div>
    </div>
    
</template>

<script>
import { computed } from '@vue/reactivity';
import $ from 'jquery';
import { useStore } from 'vuex'; 
export default {
    name: "UserProfileInfo",
    props: {
        user: {
            type: Object,
            requierd: true,
        },
    },
    setup(props, context) {
        let fullName = computed(() => props.user.lastName + ' ' + props.user.firstName);
        const store = useStore();
        const follow = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
                type: "POST",
                data: {
                    target_id: props.user.id,
                },
                headers: {
                    'Authorization' : "Bearer " + store.state.user.access,
                },
                success(resp) {
                    if(resp.result === "success") context.emit('follow');
                }
            });
        };
        const unfollow = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
                type: "POST",
                data: {
                    target_id: props.user.id,
                },
                headers: {
                    'Authorization' : "Bearer " + store.state.user.access,
                },
                success(resp) {
                    if(resp.result === "success") context.emit('unfollow');
                }
            });
        };  

        return {
            fullName,
            follow,
            unfollow,
        }
    },
};
    
</script>

<style scoped>
img {
    border-radius: 50%;
}
.username {
    font-weight: bold;
}
.fans {
    font-size: 12px;
    color:gray;
}
button {
    padding: 2px 4px;
    font-size: 12px;
}
.img-field {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
</style>
<template>
    <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-3">
                    <img class="img-fluid" src="https://cdn.acwing.com/media/user/profile/photo/76795_lg_5b565e51a2.jpg" alt="">
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

        const follow = () => {
            context.emit('follow');
        };
        const unfollow = () => {
            context.emit('unfollow');
        };  

        return {
            fullName,
            follow,
            unfollow,
        }
    },
};
    
</script>

<style>
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
</style>
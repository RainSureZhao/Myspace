<template>
    <ContentBase>
		<div class="row  justify-content-md-center">
			<div class="col-3">
				<form @submit.prevent="login">
					<div class="mb-3">
					<label for="username" class="form-label">用户名</label>
					<input v-model="username" type="text" class="form-control" id="username" aria-describedby="emailHelp">
					</div>
					<div class="mb-3">
					<label for="password" class="form-label">密码</label>
					<input v-model="password" type="password" class="form-control" id="password">
					</div>
					<div class="mb-3 form-check">
					<input type="checkbox" class="form-check-input" id="exampleCheck1">
					<label class="form-check-label" for="exampleCheck1">Check me out</label>
					</div>
          <div class="error-message">{{error-message}}</div>
					<button type="submit" class="btn btn-primary">登录</button>
				</form>
			</div>
		</div>
    </ContentBase>
  </template>
  
<script>
import ContentBase from '../components/ContentBase.vue'
import { ref } from 'vue';
import { useStore } from 'vuex';

export default {
  name: 'LoginView',
  components: {
  ContentBase,
},
  setup() {
    const store = useStore();
    let username = ref('');
    let password = ref('');
    let error_message = ref('');

    const login = () => {
      store.dispatch("login", {
        username: username.value,
        password: password.value,
        success() {
          console.log("success");
        },
        error() {
          console.log("failed");
        },
      });
    };

    return {
      username,
      password,
      error_message,
      login
    }
  }
}
</script>
  
<style scoped>
.container {
  margin-top: 20px;
}
button {
  width: 100%;
}
.error-message {
  color: red;
}
</style>
  
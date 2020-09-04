<template>
  <v-container>
    <v-layout row class="text-xs-center">
      <v-flex xs4 class="grey lighten-4">
        <v-container style="position: relative;top: 13%;" class="text-xs-center">
          <v-card flat>
            <v-card-title primary-title>
              <h4>Login</h4>
            </v-card-title>
            <v-form>
            <v-text-field name="username" label="username" v-model="username"></v-text-field>
            <v-text-field name="password" label="password" type="password" v-model="password"></v-text-field>
            <v-card-actions>
              <v-btn @click="userLogin()" primary large block>
                Login
              </v-btn>
            </v-card-actions>
            </v-form>
          </v-card>
        </v-container>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import router from '../router/index';
import axios from 'axios';

export default {
	name: 'Login',
	data: () => ({
		username: '',
		password: '',
		isAuth: false,
	}),
	methods: {
		checkAuth(req, res, next) {
			if (!req.session.user_id) {
				res.send('You are not authorized to view this page');
			} else {
				next();
			}
		},
		userLogin() {
			if (this.username && this.password)
				axios.get('http://localhost:3000')
					.then(response => {
						console.log(response.data);
						if (response.data.username === this.username && response.data.password === this.password) {
							this.isAuth = true
							router.push('about')
						} else {
							console.log('Incorrect login/password');
						}
					})
					.catch((error) => {
						console.log(error);
					});
		}
	}
}
</script>

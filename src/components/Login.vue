<template>
      <div class="d-flex justify-content-center h-100">
		<div class="card">
			<div class="card-header text-left">
				<h3>Log In</h3>
			</div>
			<div class="card-body">
				<form @submit.prevent="login">
					<div class="input-group form-group">
						<div class="input-group-prepend">
							<span class="input-group-text"><i class="fa fa-envelope"></i></span>
						</div>
						<input type="text" class="form-control" placeholder="email" v-model="email" required>						
					</div>
					<div class="input-group form-group">
						<div class="input-group-prepend">
							<span class="input-group-text"><i class="fa fa-key"></i></span>
						</div>
						<input type="password" class="form-control" placeholder="password" v-model="password" required>
					</div>
					<div class="form-group">
						<input type="submit" value="Login" class="btn float-right login_btn">
					</div>
				</form>
			</div>
			<div class="card-footer">
				<div class="d-flex justify-content-center links">
					Don't have an account?<a href="/register">Register</a>
				</div>
				<h3 class="d-flex justify-content-center">{{this.$store.state.status}}</h3>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data(){
			return {
				email : "",
            	password : "",
				mesg:""
            }
		},
		methods: {
		 	login: function () {
		   		let email = this.email 
		   		let password = this.password
		   		this.$store.dispatch('login', { email, password })
		   		.then(() => this.$router.push('/'))
		   		.catch(err => console.log(err))
				
		   	}
		}
	}
</script>
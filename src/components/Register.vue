<template>
      <div class="d-flex justify-content-center h-100">
		<div class="card">
			<div class="card-header text-left">
				<h3>Register</h3>
			</div>
			<div class="card-body">
				<form @submit.prevent="register">
                    <div class="input-group form-group">
						<div class="input-group-prepend">
							<span class="input-group-text"><i class="fa fa-user"></i></span>
						</div>
						<input type="text" class="form-control" placeholder="first name" v-model="firstname" required>			
					</div>
					<div class="input-group form-group">
						<div class="input-group-prepend">
							<span class="input-group-text"><i class="fa fa-user"></i></span>
						</div>
						<input type="text" class="form-control" placeholder="last name" v-model="lastname" required>
					</div>
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
						<input type="submit" value="Register" class="btn float-right login_btn">
					</div>
				</form>
			</div>
		</div>
	</div>
</template>

<script>
    export default {
        data(){
            return {
                firstname : "",
				lastname : "",
                email : "",
                password : "",
            }
        },
        methods: {
            register: function () {
                let data = {
                    firstname: this.firstname,
					lastname: this.lastname,
                    email: this.email,
                    password: this.password
                }
                this.$store.dispatch('register', data)
               .then(() => this.$router.push('/'))
               .catch(err => console.log(err))
            }
        }
    }
</script>
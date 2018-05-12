<template>
	<div id="edit-employee">
		<h3>Edit Employee</h3>
		<div class="row">
			<form @submit.prevent="updateEmployee" class="col s12">
				<div class="row">
					<div class="input-field col s12">
						<input disabled="" type="text" v-model="employee_id" required>

					</div>
				</div>
				<div class="row">
					<div class="input-field col s12">
						<input type="text" v-model="name" required>

					</div>
				</div>
				<div class="row">
					<div class="input-field col s12">
						<input type="text" v-model="dept" required>

					</div>
				</div>
				<div class="row">
					<div class="input-field col s12">
						<input type="text" v-model="position" required>
	
					</div>
				</div>
				<button type="submit" class="btn">Edit</button>
				<router-link to="/" class="btn grey">Cancel</router-link>

				
			</form>
		</div>
	</div>
</template>

<script>
import db from './firebaseInit'
	export default{
		name: 'edit-employee',
		data(){
			return{
				employee_id: null,
				name: null,
				dept: null,
				position: null
			}
		},
		beforeRouteEnter(to, from, next)  {
			db.collection('Employees').where('em_id', '==' , to.params.employee_id).get()
			.then(querySnapshot => {
				querySnapshot.forEach(doc => {
					next(vm => {
						vm.employee_id = doc.data().em_id
						vm.name = doc.data().name
						vm.dept = doc.data().department
						vm.position = doc.data().position
					});
				})
			})
		},

		watch:{
			'$route': 'fetchData'
		},

		methods:{
			fetchData(){
				db.collection('Employees').where('em_id', '==' , this.$route.params.employee_id).get()
				.then(querySnapshot => {
					querySnapshot.forEach(doc => {
						this.employee_id = doc.data().em_id
						this.name = doc.data().name
						this.dept = doc.data().department
						this.position = doc.data().position
					})
				})
			},

			updateEmployee(){
				db.collection('Employees').where('em_id', '==' , this.$route.params.employee_id).get()
				.then(querySnapshot => {
					querySnapshot.forEach(doc => {
						doc.ref.update({
							em_id: this.employee_id,
							name: this.name,
							department: this.dept,
							position: this.position
						})
						.then(() => {
							this.$router.push({name: 'view-employee', params: {employee_id: this.employee_id}})
						})
					})
				})
			}
		}
	}
</script>
<template>
	<div id="view-employee">
		<h5>Name: {{name}}</h5>
		<h5>Department: {{department}}</h5>
		<h5>Position: {{position}}</h5>

		<router-link to="/" class="btn grey">Back</router-link>
				
		<button @click="deleteEmployee" class="btn red">Delete</button>
				
			

		<div class="fixed-action-btn">
			<router-link v-bind:to="{name: 'edit-employee', params:{employee_id: employee_id}}" class="btn-floating btn-large red">
				<i class="fa fa-pencil"></i>
			</router-link>
		</div>
	</div>
</template>

<script>
	import db from './firebaseInit'
	export default{
		name: 'view-employee',
		data(){
			return{
				employee_id: null,
				name: null,
				department: null,
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
						vm.department = doc.data().department
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
						this.department = doc.data().department
						this.position = doc.data().position
					})
				})
			},
			deleteEmployee(){
				if(confirm('Are you sure?')){
					db.collection('Employees').where('em_id', '==', this.$route.params.employee_id)
					.get().then(querySnapshot => {
						querySnapshot.forEach(doc => {
							doc.ref.delete()
							this.$router.push('/')
						})
					})
				}
			}
		}
	}
</script>
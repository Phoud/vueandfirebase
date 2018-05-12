<template>
	<nav>
		<div class="nav-wrapper green">
			<div class="container">
				<router-link to="/" class="brand-logo">Employee Manager</router-link>
				<ul class="right">
				<li><router-link v-bind:to="{path: '/'}">Dashboard</router-link></li>
				<li><router-link v-bind:to="{name: 'login'}">Login</router-link></li>
				<li><router-link v-bind:to="{name: 'register'}">Register</router-link></li>
				</ul>
			</div>
		</div>
	</nav>
</template>
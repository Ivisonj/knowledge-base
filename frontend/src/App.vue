<template>
	<div id="app" :class="{'hide-menu': !isMenuVisible || !user}">
		<HeaderTemplateVue title="Cod3r - Base de Conhecimento" 
			:hideToggle="!user"
			:hideUserDropdown="!user" />
		<MenuTemplateVue v-if="user" />
		<LoadingComponentVue v-if="validatingToken" />
		<ContentTemplateVue v-else />
		<FooterTemplateVue />
	</div>
</template>

<script>
import axios from 'axios'
import { baseApiUrl, userKey } from '@/global'
import { mapState } from 'vuex'
import ContentTemplateVue from './components/template/Content-Template.vue'
import FooterTemplateVue from './components/template/Footer-Template.vue'
import HeaderTemplateVue from './components/template/Header-Template.vue'
import MenuTemplateVue from './components/template/Menu-Template.vue'
import LoadingComponentVue from './components/template/Loading-Component.vue'

export default {
	name: "App",
	components: { ContentTemplateVue, MenuTemplateVue, HeaderTemplateVue, FooterTemplateVue, LoadingComponentVue }, 
	computed: mapState(['isMenuVisible', 'user']),
	data: function() {
		return {
			validatingToken: true
		}
	},
	methods: {
		async validateToken() {
			this.validatingToken = true

			const json = localStorage.getItem(userKey)
			const userData = JSON.parse(json)
			this.$store.commit('setUser', null)

			if(!userData) {
				this.validatingToken = false
				this.$router.push({ name: 'auth' })
				return
			}

			const res = await axios.post(`${baseApiUrl}/validateToken`, userData)

			if (res.data) {
				this.$store.commit('setUser', userData)
				
				if(this.$mq === 'xs' || this.$mq === 'sm') {
					this.$store.commit('toggleMenu', false)
				}
			} else {
				localStorage.removeItem(userKey)
				this.$router.push({ name: 'auth' })
			}

			this.validatingToken = false
		}
	},
	created() {
		this.validateToken()
	}
}
</script>

<style>
	* {
		font-family: "Lato", sans-serif;	
	}

	body {
		margin: 0;
	}

	#app {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;

		height: 100vh;
		display: grid;
		grid-template-rows: 60px 1fr 60px; 
		grid-template-columns: 300px 1fr; 
		grid-template-areas: 
			"header header"
			"menu content"
			"menu footer"	
		;
	}

	#app.hide-menu {
		grid-template-areas: 
			"header header"
			"content content"
			"footer footer"
		;
	}

</style>

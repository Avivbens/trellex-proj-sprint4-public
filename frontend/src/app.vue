<template>
	<div id="app" class="app" :style="background" v-loading="isLoading">
		<black-screen />
		<user-msg />
		<user-profile-menu
			v-if="userProfile"
			@closeUserProfile="userProfile = false"
		/>

		<app-header
			@openUserProfile="userProfile = true"
			:style="appHeaderStyle"
		/>
		<router-view />
	</div>
</template>

<script>
import blackScreen from './cmps/black-screen.vue'
import userMsg from './cmps/user-msg.vue'
import appHeader from './cmps/app-header.vue'
import { eventBusService } from './services/event-bus.service.js';
import userProfileMenu from '@/cmps/user-profile-menu.vue'

export default {
	data() {
		return {
			userProfile: false,
			isLoading: false
		}
	},
	computed: {
		background() {
			if (this.$route.name === 'signup' || this.$route.name === 'login') {
				return {
					backgroundImage: `url('https://res.cloudinary.com/avivcloud/image/upload/v1626896090/login_c2q3at.jpg')`,
					backgroundRepeat: 'no-repeat',
					backgroundSize: 'cover',
					backgroundPosition: 'center'
				}
			}
			// if (this.$route.name === 'home') {
			// 	return {
			// 		backgroundImage: 'url(https://res.cloudinary.com/avivcloud/image/upload/v1626888554/forest.jpg)',
			// 		backgroundRepeat: 'no-repeat',
			// 		backgroundSize: '100% auto'
			// 	}
			// }
			if (this.$store.getters.board && this.$route.path.includes('board')) {				
				const boardStyle = this.$store.getters.board.style
				// url(https://res.cloudinary.com/avivcloud/image/upload/v1626888554/forest.jpg)
				const res = {
					backgroundImage: '',
					backgroundRepeat: 'no-repeat',
					backgroundSize: 'cover',
					backgroundPosition: 'center'
				}

				if (boardStyle.bgUrl) {
					res.backgroundImage = `url('${boardStyle.bgUrl}')`
					return res
				}
				if (boardStyle.bgColor) return { backgroundColor: `${boardStyle.bgColor}` }
			}
		},
		appHeaderStyle() {
			if (this.$route.name === 'home') {
				return {
					backgroundColor: '#026AA7',
				}
			}
		}
	},
	created(){
		eventBusService.$on('loaderOn', ()=> {
			this.isLoading = true
		})
		eventBusService.$on('loaderOff', () => {
			this.isLoading = false
		})
	},
	destroyed(){
		eventBusService.$off('loaderOn')
		eventBusService.$off('loaderOff')
	},
	components: {
		blackScreen,
		appHeader,

		userProfileMenu,
		userMsg
	},
}
</script>
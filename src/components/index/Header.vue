<template>
	<header ref="header">
		<div class="view">
			<img ref="img_bg1" :src="defaultSettings.bg1" style="display: none;">
			<div class="bg1" :style="{backgroundImage:'url('+defaultSettings.bg1+')'}"></div>
		</div>
		<div class="wrapper">
			<i class="ali-iconfont icon-down" @click="scrollToMain"></i>
		</div>
		<div class="wave"></div>
	</header>

</template>

<script>
	import {mapState} from 'vuex'
	import defaultSettings from '@/settings'

	export default {
		name: "Header",
		data() {
			return {
				loaded: false,
				defaultSettings
			}
		},
		computed: {
			...mapState(['clientSize'])
		},
		watch: {
			'clientSize.clientHeight'() {
				this.setHeaderHeight()
			}
		},
		mounted() {
			this.$refs.img_bg1.onload = () => {
				this.loaded = true
			}
			this.setHeaderHeight()
			let startingPoint
			const header = this.$refs.header
			header.addEventListener('mouseenter', (e) => {
				startingPoint = e.clientX
			})
		},
		methods: {
			//根据可视窗口高度，动态改变首图大小
			setHeaderHeight() {
				this.$refs.header.style.height = this.clientSize.clientHeight + 'px'
			},
			//平滑滚动至正文部分
			scrollToMain() {
				window.scrollTo({top: this.clientSize.clientHeight, behavior: 'smooth'})
			}
		},
	}
</script>

<style scoped>
	header {
		--percentage: 0.5;
		user-select: none;
	}

	.view {
		position: absolute;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		display: flex;
		justify-content: center;
	}

	.view div {
		background-position: center center;
		background-size: cover;
		position: absolute;
		width: 110%;
		height: 100%;
	}


	header .view,
	header .bg1 {
		transition: .2s all ease-in;
	}

	header.moving .view,
	header.moving .bg1{
		transition: none;
	}

	@keyframes lineMove {
		9% {
			top: 38px;
		}
		14% {
			top: 8px;
		}
		18% {
			top: 42px;
		}
		22% {
			top: 1px;
		}
		32% {
			top: 32px;
		}
		34% {
			top: 12px;
		}
		40% {
			top: 26px;
		}
		43% {
			top: 7px;
		}
		99% {
			top: 30px;
		}
	}

	@keyframes malfunctionAni {
		10% {
			top: -0.4px;
			left: -1.1px;
		}
		20% {
			top: 0.4px;
			left: -0.2px;
		}
		30% {
			left: .5px;
		}
		40% {
			top: -0.3px;
			left: -0.7px;
		}
		50% {
			left: 0.2px;
		}
		60% {
			top: 1.8px;
			left: -1.2px;
		}
		70% {
			top: -1px;
			left: 0.1px;
		}
		80% {
			top: -0.4px;
			left: -0.9px;
		}
		90% {
			left: 1.2px;
		}
		100% {
			left: -1.2px;
		}
	}

	.wrapper {
		position: absolute;
		width: 100px;
		bottom: 150px;
		left: 0;
		right: 0;
		margin: auto;
		font-size: 26px;
		z-index: 100;
	}

	.wrapper i {
		font-size: 60px;
		cursor: pointer;
		position: absolute;
    color: #ffffffcf;
		top: 60px;
		left: 20px;
		animation: opener .2s ease-in-out alternate infinite;
		transition: opacity .1s ease-in-out, transform .5s ease-in-out .2s;
	}

	.wrapper i:hover {
		opacity: 1;
	}

	@keyframes opener {
		100% {
			top: 65px
		}
	}

	.wave {
    position: absolute;
    bottom: 0;
    z-index: 80;
		background: linear-gradient(360deg, #efefef, #ffffff00);
		height: 300px;
		width: 100%;
	}
</style>

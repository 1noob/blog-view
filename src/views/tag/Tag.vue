<template>
		<BlogList :getBlogList="getBlogList" :blogList="blogList" :totalPage="totalPage"/>
</template>

<script>
	import BlogList from "@/components/blog/BlogList";
	import {getBlogListByTagName} from "@/api/tag";

	export default {
		name: "Tag",
		components: {BlogList},
		data() {
			return {
				blogList: [],
				totalPage: 0
			}
		},
		watch: {
			//在当前组件被重用时，要重新获取博客列表
			'$route.fullPath'() {
				if (this.$route.name === 'tag') {
					this.getBlogList()
				}
			}
		},
		created() {
			this.getBlogList()
		},
		computed: {
			tagName() {
				return this.$route.params.name
			}
		},
		methods: {
			getBlogList(pageNum) {
				getBlogListByTagName(this.tagName, pageNum).then(res => {
					if (res.code === 200) {
						this.blogList = res.data.list
						this.totalPage = res.data.totalPage
						this.$nextTick(() => {
							Prism.highlightAll()
						})
					} else {
						this.msgError(res.msg)
					}
				}).catch(() => {
					this.msgError("请求失败")
				})
			}
		}
	}
</script>

<style scoped>

</style>

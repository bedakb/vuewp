<template>
	<div v-if="item.content">
		<div class="Container">
			<div class="Row">
				<div class="ColumnSeventy">
					<article class="BlogPostSingle">
						<figure class="BlogPostSingle__images" v-if="item.better_featured_image">
							<img :src="item.better_featured_image.source_url">
						</figure>
						<header class="BlogPostSingle__header">
							<h1>{{ item.title.rendered }}</h1>
						</header>
						<aside class="BlogPostSingle__content">
							<div v-html="item.content.rendered"></div>
						</aside>
					</article>
				</div>
				<div class="ColumnQuarter">
					<aside class="SidebarItem">
						<header class="SidebarItem__header">
							<h3>Latest Posts</h3>
						</header>
						<ul>
							<li v-for="post in limitedPosts">
								<router-link :to="{ name: 'post', params: { id: post.id } }">
									{{ post.title.rendered }}
								</router-link>
							</li>
						</ul>
					</aside>
				</div>
			</div>
		</div>
	</div>
</template>

<script>

	import PostService			from 			'../services/PostService'
	import PostsService			from 			'../services/PostsService'

	export default {

		data() {
			return {
				item: [],
				posts: []
			}
		},

		created() {
			this.fetchItem()
			this.fetchPosts()
		},

		watch: {
			'$route': 'fetchItem'
		},

		computed: {
			limitedPosts() {
				return this.posts.splice(0,5)
			}
		},

		methods: {
			fetchItem() {
				return PostService.get(this.$route.params.id)
					.then(result => {
						this.item = result.data
					})
			},
			fetchPosts() {
				return PostsService.get()
					.then(result => {
						this.posts = result.data
					})
			}
		}

	}
</script>

<style lang="stylus">
	.BlogPostSingle
		&__header
			h1 
				font-size: 4em
				padding: 0.3em 0
		&__content
			color: #444
			h1, h2, h3, h4, h5, h6
				padding: 0.3em 0

	.SidebarItem
		margin-bottom: 1em
		&__header 
			background: #fff
			padding: 1em
			text-transform: uppercase
		ul
			list-style: none
			margin: 0
			padding: 0
			li 
				padding: 1em
				background: #fefefe
				border-top: 1px solid #eee


	.fade-enter-active 
		transition: opacity 0.5s

	.fade-leave-active
		@extend .fade-enter-active

	.fade-enter 
		opacity: 0

	.fade-leave  
		@extend .fade-enter

	@media only screen and (max-width: 640px)
		.BlogPostSingle
			margin: 1em 0
			&__header
				h1 
					font-size: 2.5em

</style>
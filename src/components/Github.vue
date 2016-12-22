<script>
    import FileExplore from './FileExplore.vue'
	export default {
		name:'Github',
		data: function(){
			return {
				fullRepoName:'',
				username:'',
				repo:''
			}
		},
		methods:{
			changeRepo:function(){
				
				this.username= this.fullRepoName.split('\/').shift();
				this.repo = this.fullRepoName.split('\/').pop();
				console.group("Github's Data");
				console.log("username",this.username);
				console.log("repo",this.repo);
				console.groupEnd("Github's Data");
			}
		},
		components:{
			FileExplore
		}
	}
</script>
<template>
	<div id="container" class="container">
		<div class="row">
			<legend >
				<h2>
					<span class="mega-octicon octicon-mark-github">
						文件浏览器
					</span>
				</h2>
			</legend>

			<form  id="changeRepoForm" class="form-inline" @submit.prevent="changeRepo()">
				<div class="form-group">
					<label for="fullRepoName">Repo 名称</label>
					<input type="text" class="form-control" id="fullRepoName" v-model="fullRepoName" style="width:300px;">
				</div>
				<button class="btn btn-primary">获&nbsp;取</button>
			</form>
			<hr>
			<file-explore :username="username" :repo="repo"></file-explore>
		</div>
	</div>
</template>
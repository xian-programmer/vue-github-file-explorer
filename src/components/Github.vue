<script>
	export default {
		name:'Github',
		data: function(){
			return {
				fullRepoName:''
			}
		},
		methods:{
			changeRepo:function(){
				console.group("Github's Data");
				console.log(111);
				console.groupEnd("Github's Data");
			}
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
		</div>
	</div>
</template>
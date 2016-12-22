<script>
	export default{
		name: "FileExplore",
		data:function(){
			return {
				path:'/',
				files:[]
			};
		},
		// 和父级绑定
		props: {
			username:{
				type: String,
				required:true
			},
			repo:{
				type:String,
				required:true
			}
		},
		methods:{
			goBack:function(){
				this.path = this.path.split('/').slice(0, -1).join('/');
		        if (this.path === '') this.path = '/';
		        this.getFiles();
			},
			getFiles: function() {
				console.log("fullRepoUrl",'https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.path);
		        this.$http.get('https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.path,
		          function(data) {
		            this.files = data;
		          }
		        );
		      },
		    changePath: function(path) {
		        this.path = '/' + path;
		        this.getFiles();
		     }
		},
		computed:{
			sortedFiles: function() {
		        return this.files.slice(0).sort(function(a, b) {
		          if (a.type !== b.type) {
		            if (a.type === 'dir') {
		              return -1;
		            } else {
		              return 1;
		            }
		          } else {
		            if (a.name < b.name) {
		              return -1;
		            } else {
		              return 1;
		            }
		          }
		        });
		    },
		    fullRepoUrl: function() {
		        return this.username + '/' + this.repo;
		     }
		},
		
		watch: {
	      repo: function(newVal, oldVal) {
	        this.path = '/';
	        this.getFiles();
	      }
	    },
	    created:function(){
	    	console.log(1111);
			if (this.username && this.repo) this.getFiles();
		}
	}
</script>
<template>
	<div class="row">
		<div  class="col-md-12 col-sm-12">
			<table class="table table-hover">
				<thead>
					<tr>
						<th>当前路径{{path}}</th>
						<th class="text-right">
							<button v-if="path!=='/'" @click="goBack()" class="btn btn-danger btn-xs">返&nbsp;回</button>
						</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="file in sortedFiles">
						<td>
							<div class="file" v-if="file.type==='file'">
								<span class="octicon octicon-file-text">
									<a href="#">{{file.name}}</a>
								</span>
							</div>
							<div class="directory" v-if="file.type === 'dir'">
				                <span class="octicon octicon-file-directory"></span>
				                <a href="#" @click="changePath(file.path)"> {{ file.name }}</a>
				              </div>
						</td>
						<td>
							
						</td>
					</tr>
				</tbody>
			</table>

		</div>
	</div>
</template>
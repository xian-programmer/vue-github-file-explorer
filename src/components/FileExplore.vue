<script>
	export default{
		name: "FileExplore",
		data:function(){
			return {
				path:'/',
				files:[],
				file:""
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
		filters: {
		  formBase64: function (value) {
		    if (!value) return '';
		    return window.atob(value);
		  }
		},
		methods:{
			goBack:function(){
				this.path = this.path.split('/').slice(0, -1).join('/');
		        if (this.path === '') this.path = '/';
		        this.getFiles();
		         this.file = "";
			},
			getFiles: function() {
				console.log("fullRepoUrl",'https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.path);
		        this.$http.get('https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.path,
		          function(data) {
		            this.files = data;
		          }
		        );
		     },
		     getFile:function(){
		      	console.log("fullRepoUrl",'https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.path);
		        this.$http.get('https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.path,
		          function(data) {
		          	this.files = [];
		            this.file = data;
		          }
		        );
		     },
		    changePath: function(path,type) {
		    	this.file = "";
		        this.path = '/' + path;
		        type=="dir"?this.getFiles():this.getFile();
		     },
		     
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
					<tr v-for="item in sortedFiles">
						<td>
							<div class="file" v-if="item.type==='file'">
								<span class="octicon octicon-file-text">
									<a href="#" @click="changePath(item.path,item.type)">{{item.name}}</a>
								</span>
							</div>
							<div class="directory" v-if="item.type === 'dir'">
				                <span class="octicon octicon-file-directory"></span>
				                <a href="#" @click="changePath(item.path,item.type)"> {{ item.name }}</a>
				              </div>
						</td>
						<!-- <td>
							
						</td> -->
					</tr>
					<tr v-if="file">
						<td style="background:#fff;width: 100%;">
							<div class="code_source">
							 <h3>
						      <svg　style="margin-top:5px;" aria-hidden="true" class="octicon octicon-book" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M3 5h4v1H3V5zm0 3h4V7H3v1zm0 2h4V9H3v1zm11-5h-4v1h4V5zm0 2h-4v1h4V7zm0 2h-4v1h4V9zm2-6v9c0 .55-.45 1-1 1H9.5l-1 1-1-1H2c-.55 0-1-.45-1-1V3c0-.55.45-1 1-1h5.5l1 1 1-1H15c.55 0 1 .45 1 1zm-8 .5L7.5 3H2v9h6V3.5zm7-.5H9.5l-.5.5V12h6V3z"></path></svg>
						      {{file.name}}
						      <span class="fileSize">大小：{{file.size}}字节</span>
						    </h3>
								
							<div style="word-wrap: break-word; white-space: pre-wrap;padding:16px;background:#f7f7f7" >
								{{file.content | formBase64}}
							</div>
							</div>
						</td>
					</tr>
				</tbody>
			</table>

		</div>
	</div>
</template>
<style>
	.code_source{

		border:1px solid #d8d8d8;
		border-radius: 5px;
	}
	.code_source h3{
		display: block;
	    padding: 9px 10px 10px;
	    margin: 0;
	    font-size: 14px;
	    line-height: 17px;
	    background-color: #f5f5f5;
	    border-bottom: 1px solid #d8d8d8;
	}
	.code_source .fileSize{
		margin-left: 20px;
	}
</style>
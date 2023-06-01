<template>
	<view>
		<view class="">
			<u-cell-group>
				<u-cell-item  title="标题" :arrow="false">
					<input type="text" v-model="title"  />
				</u-cell-item>
				<u-cell-item  title="内容" :arrow="false">
					<textarea maxlength="150" placeholder="最大150字" v-model="content"  @input="change"/>
					<!-- <input type="text" value="" /> -->
					<text>{{commentLen}}/150</text>
				</u-cell-item>
			</u-cell-group>
		</view>
		<u-button @click="save" >提交</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					content:"",
					title:"",
					baseUrl:"",
					commentLen:0,
				}
			},
			onLoad(){
				this.baseUrl = this.$u.http.config.baseUrl
			},
		methods: {
			change(e){
				// console.log(e)
				console.log(e.detail.cursor)
				this.commentLen = e.detail.cursor
			},
			save(){
				if(this.content.length==0) {
					this.$u.toast("请输入标题")
					return
				}
				if(this.title.length==0) {
					this.$u.toast("请输入内容")
					return
				}
				this.$u.post("/prod-api/api/common/feedback",{
					"content": this.content,
					"title": this.title
				}).then(res=>{
					this.$u.toast("提交成功")
					this.content=""
					this.commentLen=0
					this.title=""
				})
			}
		}
	}
</script>

<style lang="scss">
page{
	background-color: #ededed;
}

.camera{
	width: 54px;
	height: 44px;
	
	&:active{
		background-color: #ededed;
	}
}
.user-box{
	background-color: #fff;
}
</style>

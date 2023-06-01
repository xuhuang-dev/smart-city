<template>
	<view>
		<qiun-data-charts type="bar" :chartData="data1" :opts="opts1"></qiun-data-charts>
		<qiun-data-charts type="bar" :chartData="data1" :opts="{max:100,min:0,extra:{bar:{type:'stack'}}}"></qiun-data-charts>
		<qiun-data-charts type="line" :chartData="data2" ></qiun-data-charts>
		<qiun-data-charts type="pie" :chartData="data4" :opts="opts4"></qiun-data-charts>
		<qiun-data-charts type="column" :chartData="data5" :opts="opts5"></qiun-data-charts>
		<qiun-data-charts type="column" :chartData="data3" :opts="opts3"></qiun-data-charts>
		<qiun-data-charts type="mix" :chartData="data2" :opts="opts2"></qiun-data-charts>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				opts1:{},
				data1:{},
				opts2:{},
				data2:{},
				opts3:{},
				data3:{},
				opts4:{},
				data4:{},
				opts5:{},
				data5:{},
			}
		},
		onLoad() {
			this.getData1()
			this.getData2()
			this.getData3()
			this.getData4()
			this.getData5()
		},
		methods: {
			getData1(){
				this.opts1={
					xAxis:{
						max:100,
						min:0,
					}
					// yAxis:{
					// 	disableGrid:true,
					// 	data:[
					// 		{
					// 			min:0,
					// 			max:100
					// 		}
					// 	]
					// },
				}
				this.data1 = {
					categories: ["2016","2017","2018","2019","2020","2021"],
					series: [
						{
							name: "目标值",
							data: [35,36,31,33,13,34]
						},
						{
							name: "完成量",
							data: [18,27,21,24,6,28]
						}
					]
				}
			},
			getData2(){
				this.opts2={
					padding:[15,15,0,15],
					yAxis:{
						disableGrid:true,
						padding: 10,
						data:[
							{
								// title:"完成量",
								position:"left",
								max:39,
								min:0
							},
							{
								// title:"目标值",
								position:"right",
								max:50,
								min:0
							}
						]
					},
					extra:{
						column:{
							width:30,
						},
					}
				}
				this.data2 = {
					categories: ["2016","2017","2018","2019","2020","2021"],
					series: [
						{
							name: "目标值",
							data: [35,36,31,33,13,34],
							type:"line",
							// index:0
						},{
							name: "完成量",
							data: [18,27,21,24,6,28],
							type:"column",
							// index:1
						},
					]
				}
			},
			getData3(){
				this.opts3={
					padding:[20,20,20,20],
					xAxis:{
						disableGrid:true
					},
					yAxis:{
						disableGrid:true,
						data:[
							{
								min:0,
								max:100
							}
						]
					},
					extra:{
						column:{
							type:"stack"
						}
					}
				}
				this.data3 = {
					categories: ["2016","2017","2018","2019","2020","2021"],
					series: [
						{
							name: "目标值",
							data: [35,36,31,33,13,34]
						},
						{
							name: "完成量",
							data: [18,27,21,24,6,28]
						}
					]
				}
			},
			getData4(){
				this.opts4={
					title:"70、80、90违规百分比",
					padding:[20,40,20,10],
					xAxis:{
						// disableGrid:true,
						max:100,
						min:0
					},
				}
				this.data4 = {
					// categories: ["70后","80后","90后"],
					series: [
						{
							data: [{"name":"70后","value":40},{"name":"80后","value":50},{"name":"90后","value":10}]
						}
					]
				}
			},
			getData5(){
				this.opts5={
					title:"70、80、90违规百分比",
					padding:[20,40,20,10],
					xAxis:{
						disableGrid:true,
						categoryGap:3
					},
					extra:{
						column:{
							// type:"group",
							width:14,
						}
					},
					yAxis:{
						disableGrid:true,
						data:[
							{
								min:0,
								max:100
							}
						]
					},
				}
				this.data5 = {
					categories: ["二线1","二线2","二线3","三线1","三线2","三线3","四线1","四线2","四线3"],
					series: [
						{
							name: "2021较2020新增用户百分比",
							data: [18,27,21,18,27,21,18,27,21]
						},
						{
							name: "2020较2019新增用户百分比",
							data: [35,36,31,18,27,21,18,27,21]
						},
						
					]
				}
			},
			
		}
	}
</script>

<style>

</style>

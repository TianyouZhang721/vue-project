<template>
	<div>
		<el-form label-width="80px">
			<el-form-item label="用户名">
				<el-input v-model="formLabelAlign.name"></el-input>
			</el-form-item>
			<el-form-item label="密码">
				<el-input v-model="formLabelAlign.password"></el-input>
			</el-form-item>
			<el-form-item>
				<el-button type="primary" @click="login">登录</el-button>
			</el-form-item>
		</el-form>
		<!-- echarts -->
		<!-- 来个容器盛放图表，有宽高 -->
		<div id="myecharts" ref="echart"></div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			formLabelAlign: {
				name: "",
				password: ""
			},
			list: [],
			arr: []
		};
	},
	methods: {
		login() {
			this.$http
				.post("/api/login", {
					username: this.formLabelAlign.name,
					password: this.formLabelAlign.password
				})
				.then(res => {
					console.log(res);
					if (res.data.status == 1) {
						localStorage.setItem("uname", res.data.data.username);
						this.$router.push("/admin/adminuser");
					}
				});
		}
	},
	created() {
		this.$http.get("/api/goodslist").then(res => {
			console.log(res);
			this.list = res.data.data;
			let arr = [];
			this.list.forEach(item => {
				arr.push(item.price);
			});
			this.arr = arr;
			this.$nextTick(() => {
				let myEchart = this.$echarts.init(this.$refs.echart);
				myEchart.setOption({
					title: {
						text: "vue中的echarts"
					},
					tooltip: {},
					xAxis: {
						data: [1, 2, 3]
					},
					yAxis: {},
					legend: {
						data: ["价格"]
					},
					series: [
						{
							name: "价格",
							type: "bar",
							data: this.arr
						}
					]
				});
			});
			// [{title:zhangsan, id:1}, {title: lisi, id:2}]  [zhangsan, lisi]
		});
	},
	mounted() {}
};
</script>

<style scoped>
#myecharts {
	height: 500px;
	width: 500px;
}
</style>
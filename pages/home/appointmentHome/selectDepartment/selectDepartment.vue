<template>
	<view>
		<department :allData="propsData" ref="department"></department>
	</view>
</template>

<script>
	import department from '../appointment/department.vue'
	import {getDepartmentHospitalAHall} from '@/common/api/department.js'
	
	export default {
		components: {
			department
		},
		data() {
			return {
				propsData: {
					departmentList: [],
					hospitalID: 0,
				},
			}
		},
		methods: {
			// 获取某个医院ID所属的专科
			getDepartmentHospitalAH: function(requestAH) {
				uni.showLoading({
					title: '加载中'
				})
				this.departmentList = [];
				
				getDepartmentHospitalAHall().then(res => {
					if(res.statusCode === 200) {
					 
						const data = res.data.response.body;
						
						this.propsData.departmentList = data;
						//this.$refs.department.changeInit(data[0].ksdm, data[0].ksmc)
						uni.hideLoading();
					}
				}).catch(() => {
					uni.hideLoading();
					error('网络')
				})
			},
		},
		onLoad(e) {
			this.propsData.hospitalID = e.hospitalID
			this.getDepartmentHospitalAH(e.requestAH)
			uni.setStorageSync('hospitalId', e.hospitalID)
		}
	}
</script>

<style>

</style>

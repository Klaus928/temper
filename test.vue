<template>
	<view>
		<view class="cu-form-group margin-top">
			<view class="title">维修描述</view>
			<input placeholder="请输入维修描述,长度50字内" :value="formObject.PlanDescription" name="input" @input="handlePlanDescription"></input>
		</view>
		<view class="cu-form-group">
			<view class="title">维修类型</view>
			<picker @change="PickerRepairType" range-key="label" :value="formObject.RepairMissionType" :range="RepairTypePicker">
				<view class="picker">
					{{formObject.RepairMissionType?formObject.RepairMissionType:'请选择'}}
				</view>
			</picker>
		</view>
		<view class="cu-form-group">
			<view class="title">开工时间</view>
			<picker mode="date" :value="formObject.EstimatedStartTime" :start="$dayjs(new Date()).format('YYYY-MM-DD')" end="2030-09-01" @change="DateChange($event,'EstimatedStartTime')">
				<view class="picker">
					{{formObject.EstimatedStartTime?formObject.EstimatedStartTime:'请选择'}}
				</view>
			</picker>
		</view>
		<view class="cu-form-group">
			<view class="title">预计完工时间</view>
			<picker mode="date" :value="formObject.EstimatedEndTime" :start="$dayjs(new Date()).format('YYYY-MM-DD')" end="2030-09-01" @change="DateChange($event,'EstimatedEndTime')">
				<view class="picker">
					{{formObject.EstimatedEndTime?formObject.EstimatedEndTime:'请选择'}}
				</view>
			</picker>
		</view>
		<view class="cu-form-group">
			<view class="title">包机人</view>
			<view class="right-icon" @tap="selectRelationMans">
				{{formObject.RelationMans?formObject.RelationMans:'请选择'}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				formObject: {
					ID: "",
					PlanDescription: "",  // 维修描述
					RepairMissionType: "",  // 维修类型
					EstimatedStartTime: "",  // 开工时间
					EstimatedEndTime: "",  // 完工时间
					RelationMans: "",  // 关联人员
					RelationManIds: [],  // 关联人员 ID
				},
				RepairTypePicker: []
			}
		},
		onLoad(params) {
			this.formObject.ID = params.formId
		},
		created() {
			this.getRepairType()
		},
		methods: {
			getRepairType() {
				this.$store.dispatch("waitForMe/GetRepairType").then(res => {
					console.log(res)
					this.RepairTypePicker = res
				})
			},
			handlePlanDescription(e) {
				this.formObject.PlanDescription = e.target.value
			},
			PickerRepairType(e) {
				this.formObject.RepairMissionType = this.RepairTypePicker[e.detail.value].label
			},
			DateChange(e, name) {
				this.formObject[name] = e.target.value
			},
			selectRelationMans() {
				uni.navigateTo({
					url: `./selectRelationMans`
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
.right-icon {
	flex: 1;
	padding-right: 21px;
	overflow: hidden;
	position: relative;
	cursor: pointer;
	line-height: 54px;
	height: 54px;
	text-overflow: ellipsis;
	white-space: nowrap;
	overflow: hidden;
	text-align: right;
	&::after {
		font-family: cuIcon;
		display: block;
		content: "\e6a3";
		position: absolute;
		font-size: 18px;
		color: #8799a3;
		line-height: 54px;
		width: 32px;
		text-align: center;
		top: 0;
		bottom: 0;
		right: -10px;
		margin: auto;
	}
}
</style>

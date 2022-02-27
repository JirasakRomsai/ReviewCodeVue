<template>
	<div>
		<!-- Header -->
		<base-page-heading>
			<h2 style="color: #000000">{{ $t('GapAnalysis.Title') }}</h2>
			<!-- <h6>Amet minim mollit non deserunt ullamco est sit aliqua dolor do ametsint. Velit officia consequat duis enim velit mollit.</h6> -->
		</base-page-heading>
		<!-- Page Content -->
		<v-app class="content pt-0">
			<v-container fluid>
				<v-layout row wrap>
					<v-row>
						<v-col cols="12" sm="4" md="4" lg="4" xl="4">
							<v-card tile style="position: sticky; z-index: 1; top: 70px; left: 300px">
								<v-list dense>
									<!-- <v-subheader style="padding-left: 25px">
                    <p style="font-size: 16px; font-weight: bold; color: black">สารบัญ</p>
                  </v-subheader> -->
									<v-list-item-group color="primary" active-class="active-list" v-model="dataSelectList">
										<v-list-item class="box-shadow-card">
											<v-list-item-content @click="selectTab({ data: null, title: 1 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title1') }}
													</div>
													<v-icon color="#151D44">info</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
										<v-list-item class="box-shadow-card" active-class="active-list">
											<v-list-item-content @click="selectTab({ data: $route.params.detail.data.two[0], title: 2 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title2') }}
													</div>
													<v-icon :color="this.dataCheckSuccess[0].color"> mdi-checkbox-marked-circle</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
										<v-list-item class="box-shadow-card" active-class="active-list">
											<v-list-item-content @click="selectTab({ data: $route.params.detail.data.three[0], title: 3 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title3') }}
													</div>
													<v-icon :color="this.dataCheckSuccess[1].color">mdi-checkbox-marked-circle</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
										<v-list-item class="box-shadow-card" active-class="active-list">
											<v-list-item-content @click="selectTab({ data: $route.params.detail.data.four[0], title: 4 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title4') }}
													</div>
													<v-icon :color="this.dataCheckSuccess[2].color">mdi-checkbox-marked-circle</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
										<v-list-item class="box-shadow-card" active-class="active-list">
											<v-list-item-content @click="selectTab({ data: $route.params.detail.data.five[0], title: 5 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title5') }}
													</div>
													<v-icon :color="this.dataCheckSuccess[3].color">mdi-checkbox-marked-circle</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
										<v-list-item class="box-shadow-card" active-class="active-list">
											<v-list-item-content @click="selectTab({ data: $route.params.detail.data.six[0], title: 6 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title6') }}
													</div>
													<v-icon :color="this.dataCheckSuccess[4].color">mdi-checkbox-marked-circle</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
										<v-list-item class="box-shadow-card" active-class="active-list">
											<v-list-item-content @click="selectTab({ data: $route.params.detail.data.seven[0], title: 7 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title7') }}
													</div>
													<v-icon :color="this.dataCheckSuccess[5].color">mdi-checkbox-marked-circle</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
										<v-list-item class="box-shadow-card" active-class="active-list">
											<v-list-item-content @click="selectTab({ data: $route.params.detail.data.eight[0], title: 8 })">
												<div class="d-flex justify-content-between">
													<div class="title-list">
														{{ $t('GapAnalysis.ListAssForm.Title8') }}
													</div>
													<v-icon :color="this.dataCheckSuccess[6].color">mdi-checkbox-marked-circle</v-icon>
												</div>
											</v-list-item-content>
										</v-list-item>
									</v-list-item-group>
								</v-list>
							</v-card>
						</v-col>
						<v-col cols="12" sm="8" md="8" lg="8" xl="8" class="pl-md-3">
							<form-update-progress :data="data"> </form-update-progress>
						</v-col>
					</v-row>
				</v-layout>
			</v-container>
		</v-app>
	</div>
</template>

<script>
import FormUpdateProgress from '@/components/report/FormUpdateProgress.vue'
export default {
	components: {
		FormUpdateProgress: FormUpdateProgress,
	},
	data() {
		return {
			data: { data: null, title: 1, dataAll: [] },
			dataRoute: null,
			dataCheckSuccess: [],
			dataSelectList: 0,
		}
	},
	created() {
		this.dataRoute = this.$route
		if (this.$route.params.title) {
			this.dataSelectList = this.$route.params.title - 1
			let titleDatas = null
			if (this.$route.params.title === 2) titleDatas = this.$route.params.detail.data.two[0]
			else if (this.$route.params.title === 3) titleDatas = this.$route.params.detail.data.three[0]
			else if (this.$route.params.title === 4) titleDatas = this.$route.params.detail.data.four[0]
			else if (this.$route.params.title === 5) titleDatas = this.$route.params.detail.data.five[0]
			else if (this.$route.params.title === 6) titleDatas = this.$route.params.detail.data.six[0]
			else if (this.$route.params.title === 7) titleDatas = this.$route.params.detail.data.seven[0]
			else if (this.$route.params.title === 8) titleDatas = this.$route.params.detail.data.eight[0]

			this.data = { ...this.data, data: titleDatas, title: this.$route.params.title, dataAll: this.$route.params.detail.data }
		} else {
			this.data = { ...this.data, dataAll: this.$route.params.detail.data }
		}
		this.checkSuccess()
	},
	methods: {
		selectTab({ data, title }) {
			this.data = { ...this.data, data: data, title: title, dataAll: this.$route.params.detail.data }
		},
		checkSumSuccessTitle(data) {
			let sum = 0
			for (const k in data) {
				if (`${data[k][0]}` === 'Yes') {
					sum += 1
				}
			}
			return sum
		},
		checkSuccess() {
			let { data } = this.$route.params.detail
			let sum2 = this.checkSumSuccessTitle(data.two[0])
			let sum3 = this.checkSumSuccessTitle(data.three[0])
			let sum4 = this.checkSumSuccessTitle(data.four[0])
			let sum5 = this.checkSumSuccessTitle(data.five[0])
			let sum6 = this.checkSumSuccessTitle(data.six[0])
			let sum7 = this.checkSumSuccessTitle(data.seven[0])
			let sum8 = this.checkSumSuccessTitle(data.eight[0])

			this.dataCheckSuccess[0] = sum2 == 5 ? { color: 'green' } : { color: '#B2B2B2' }
			this.dataCheckSuccess[1] = sum3 == 4 ? { color: 'green' } : { color: '#B2B2B2' }
			this.dataCheckSuccess[2] = sum4 == 3 ? { color: 'green' } : { color: '#B2B2B2' }
			this.dataCheckSuccess[3] = sum5 == 4 ? { color: 'green' } : { color: '#B2B2B2' }
			this.dataCheckSuccess[4] = sum6 == 7 ? { color: 'green' } : { color: '#B2B2B2' }
			this.dataCheckSuccess[5] = sum7 == 5 ? { color: 'green' } : { color: '#B2B2B2' }
			this.dataCheckSuccess[6] = sum8 == 2 ? { color: 'green' } : { color: '#B2B2B2' }
		},
	},
}
</script>

<style scoped>
.box-shadow-card {
	border-left-style: solid;
	border-left-color: #2f4d99;
	border-left-width: 10px;
	margin-top: 1px;
}
.active-list {
	background-color: rgba(47, 77, 153, 0.01);
}
.title-list {
	font-size: 14px !important;
	line-height: 1.6 !important;
}
.active-list {
	background: #f6f6f6;
}
</style>

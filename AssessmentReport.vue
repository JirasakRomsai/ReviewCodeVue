<template>
	<div>
		<!-- Header -->
		<base-page-heading>
			<h2 style="color: #000000">{{ $t('GapAnalysis.Title') }}</h2>
			<!-- <h6>Amet minim mollit non deserunt ullamco est sit aliqua dolor do ametsint. Velit officia consequat duis enim velit mollit.</h6> -->
		</base-page-heading>
		<!-- Page Content -->
		<v-app class="content pt-0">
			<div class="d-flex flex-column">
				<div class="d-flex flex-row justify-content-end">
					<b-button
						variant="primary"
						style="color: #ffffff; font-size: 14px"
						class="d-flex align-center justify-content-end font-s"
						@click="printReport"
						v-if="printer"
						>{{ $t('GapAnalysis.AssReport.DownloadReport') }}</b-button
					>
				</div>
				<span class="header mt-2 mb-1" style="font-size: 16px; font-weight: 700"
					>{{ $t('GapAnalysis.AssReport.OverrallSummary') }} : {{ Math.round(progress) }} %</span
				>
				<v-progress-linear
					v-if="printer"
					:value="progress"
					height="30"
					color="#28A745"
					background-color="#ededed"
					rounded
					class="mt-8 v-progress-ass"
				>
				</v-progress-linear>

				<div v-if="!printer" class="progress mt-8" style="height: 30px">
					<div class="progress-bar bg-success" role="progressbar" :style="{ width: progress + '%' }"></div>
				</div>
				<!-- <p class="header mt-8">ภาพรวม</p>
        <p class="contents">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Ultricies sit aenean at adipiscing elementum sagittis feugiat non. Sem lectus eget
          consectetur dignissim odio nisl bibendum. Aliquam arcu quam lorem in ac non fusce lorem. Quam tortor aliquet gravida nulla nisi, consectetur
          id purus. A ut neque, morbi nulla sagittis lobortis facilisi tortor, massa. Parturient risus ornare lacus tincidunt lobortis id turpis diam
          posuere. Tellus aliquet risus at scelerisque etiam tellus venenatis nulla enim. Tempor felis suspendisse dignissim pulvinar commodo,
          pharetra. Egestas a rutrum ultricies consequat nulla.
        </p> -->
			</div>
			<!-- <p class="header mt-3">การวิเคราะห์และคำแนะนำ</p> -->
			<p class="header mt-8">{{ $t('GapAnalysis.AssReport.RiskAndAdvice') }}</p>
			<div>
				<v-tabs color="#2f4d99">
					<v-tabs-slider color="#2f4d99"></v-tabs-slider>
					<v-tab> {{ $t('GapAnalysis.AssReport.TabAnswer') }} </v-tab>
					<!-- <v-tab v-if="$route.params.risk"> ความเสี่ยงและคำแนะนำ </v-tab> -->
					<v-tab-item>
						<v-card width="99%" class="ml-1 mr-1 mt-5 mb-8">
							<answer-table :data="$route.params.detail.data"></answer-table>
						</v-card>
					</v-tab-item>
					<!-- <v-tab-item v-if="$route.params.risk">
            <v-card width="99%" class="ml-1 mr-1 mt-5">
              <risk-table :data="$route.params.detail.data"></risk-table>
            </v-card>
          </v-tab-item> -->
				</v-tabs>
			</div>
		</v-app>
	</div>
</template>

<script>
import AnswerPdpaTable from '@/views/tables/assessment/AnswerPdpaTable.vue'
// import RiskPdpaTable from '@/views/tables/assessment/RiskPdpaTable.vue'

export default {
	components: {
		answerTable: AnswerPdpaTable,
		// riskTable: RiskPdpaTable,
	},
	data() {
		return {
			printer: true,
		}
	},
	computed: {
		progress() {
			return (this.$route.params.summary[0] / 30) * 100
		},
	},
	methods: {
		printReport() {
			var x = document.getElementById('zsiq_float')
			setTimeout(() => {
				this.printer = !this.printer
				if (x.style.display === 'none') {
					x.style.display = 'block'
				} else {
					x.style.display = 'none'
				}
			}, 1000)

			setTimeout(() => {
				print()
			}, 500)

			this.printer = !this.printer
			if (x.style.display === 'none') {
				x.style.display = 'block'
			} else {
				x.style.display = 'none'
			}
		},
	},
}
</script>

<style scoped>
.font-s {
	font-family: 'Sarabun', sans-serif;
}
div {
	font-family: 'Sarabun', sans-serif;
}
p.header {
	font-size: 16px;
	font-weight: 700;
}
p.contents {
	font-size: 16px;
	font-weight: 400;
}

@media print {
	.progress {
		position: relative;
	}
	.progress:before {
		display: block;
		content: '';
		position: absolute;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		z-index: 0;
		border-bottom: 2rem solid #ededed;
	}
	.progress-bar {
		position: absolute;
		top: 0;
		bottom: 0;
		left: 0;
		z-index: 1;
		border-bottom: 2rem solid #28a745;
	}

	.card-header {
		background-color: #343a40 !important;
		font-size: 16px !important;
		font-weight: 700 !important;
		color: #ffffff !important;
		height: 50px !important;
		width: 100% !important;
	}
}
</style>

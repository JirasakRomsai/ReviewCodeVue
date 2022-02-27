<template>
	<div>
		<v-dialog v-model="trigDialog" scrollable width="800px" style="position: absolute; z-index: 20001">
			<v-card width="500px" style="max-height: 60vh">
				<v-card-title style="height: 17%">
					<span class="text-h5" style="padding-left: 55px; padding-top: 20px"> {{ $t('GapAnalysis.DialogRestart.Title') }} </span>
				</v-card-title>
				<v-card-text>
					<v-container style="padding-left: 68px">
						<v-row style="font-size: 16px">
							<v-col cols="12" md="12" class="mt-3" style="font-weight: 400">
								{{ $t('GapAnalysis.DialogRestart.Main') }}
								<br />
								<br />

								<span style="font-weight: bold"> {{ $t('GapAnalysis.DialogRestart.Sub1.Title') }} : </span>
                {{ $t('GapAnalysis.DialogRestart.Sub1.Detail') }} 
								<br />
								<span style="font-weight: bold"> {{ $t('GapAnalysis.DialogRestart.Sub2.Title') }}  :</span> {{ $t('GapAnalysis.DialogRestart.Sub2.Detail') }} 
							</v-col>
							<v-col cols="12" md="11" style="font-weight: 500; text-align: center">
								{{ $t('GapAnalysis.DialogRestart.CfTxt1') }} <span class="sky"> {{ $t('GapAnalysis.DialogRestart.CfTxt2') }}  </span>{{ $t('GapAnalysis.DialogRestart.CfTxt3') }} 
							</v-col>
							<v-col cols="12" md="12">
								<v-text-field v-model="confirm" placeholder="REASSESSMENT" required style="width: 92%"></v-text-field>
							</v-col>
						</v-row>
					</v-container>
				</v-card-text>
				<v-card-actions style="width: 92%">
					<v-spacer></v-spacer>
					<v-btn style="color: #464d69; font-size: 16px" text @click="close"> {{ $t('GapAnalysis.DialogRestart.Button.Cancel') }}  </v-btn>
					<v-btn style="font-size: 16px" class="sky" :disabled="validateBtn" text @click="saveRestart()" ref="btn_restart"> {{ $t('GapAnalysis.DialogRestart.Button.Save') }}  </v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
	</div>
</template>

<script>
import axios from 'axios'
export default {
	props: ['trigDialog'],
	data() {
		return {
			confirm: '',
			form: {
				two: [
					{
						check1: ['Not Sure'],
						check2: ['Not Sure'],
						check3: ['Not Sure'],
						check4: ['Not Sure'],
						check5: ['Not Sure'],
						text3: '-',
						text1: '-',
						text2: '-',
						text4: '-',
						text5: '-',
					},
				],
				three: [
					{ check1: ['Not Sure'], text1: '-', check2: ['Not Sure'], text2: '-', check3: ['Not Sure'], text3: '-', check4: ['Not Sure'], text4: '-' },
				],
				four: [{ check1: ['Not Sure'], text1: '-', check2: ['Not Sure'], text2: '-', check3: ['Not Sure'], text3: '-' }],
				five: [
					{ check1: ['Not Sure'], check2: ['Not Sure'], check3: ['Not Sure'], check4: ['Not Sure'], text3: '-', text1: '-', text2: '-', text4: '-' },
				],
				six: [
					{
						check1: ['Not Sure'],
						check2: ['Not Sure'],
						check3: ['Not Sure'],
						text3: '-',
						check4: ['Not Sure'],
						text4: '-',
						text1: '-',
						text2: '-',
						check5: ['Not Sure'],
						check6: ['Not Sure'],
						check7: ['Not Sure'],
						text5: '-',
						text6: '-',
						text7: '-',
					},
				],
				seven: [
					{
						check1: ['Not Sure'],
						check2: ['Not Sure'],
						check3: ['Not Sure'],
						text3: '-',
						text1: '-',
						text2: '-',
						check4: ['Not Sure'],
						text4: '-',
						check5: ['Not Sure'],
						text5: '-',
					},
				],
				eight: [{ check1: ['Not Sure'], check2: ['Not Sure'], text1: '-', text2: '-' }],
				statusRestart: true,
			},
		}
	},
	watch: {
		trigDialog() {
			this.confirm = ''
		},
	},
	computed: {
		validateBtn() {
			return this.confirm === 'REASSESSMENT' ? false : true
		},
		validateClass() {
			return this.confirm === 'REASSESSMENT' ? 'sky' : 'btn-dis'
		},
	},
	methods: {
		close() {
			this.confirm = ''
			this.$emit('closeDialog')
		},
		async saveRestart() {
			let formData = {
				data: {
					two: [
						{
							check1: ['Not Sure'],
							check2: ['Not Sure'],
							check3: ['Not Sure'],
							check4: ['Not Sure'],
							check5: ['Not Sure'],
							text1: '-',
							text2: '-',
							text3: '-',
							text4: '-',
							text5: '-',
						},
					],
					three: [
						{
							check1: ['Not Sure'],
							check2: ['Not Sure'],
							check3: ['Not Sure'],
							check4: ['Not Sure'],
							text1: '-',
							text2: '-',
							text3: '-',
							text4: '-',
						},
					],
					four: [
						{
							check1: ['Not Sure'],
							check2: ['Not Sure'],
							check3: ['Not Sure'],
							text1: '-',
							text2: '-',
							text3: '-',
						},
					],
					five: [
						{
							check1: ['Not Sure'],
							check2: ['Not Sure'],
							check3: ['Not Sure'],
							check4: ['Not Sure'],
							text1: '-',
							text2: '-',
							text3: '-',
							text4: '-',
						},
					],
					six: [
						{
							check1: ['Not Sure'],
							check2: ['Not Sure'],
							check3: ['Not Sure'],
							check4: ['Not Sure'],
							check5: ['Not Sure'],
							check6: ['Not Sure'],
							check7: ['Not Sure'],
							text1: '-',
							text2: '-',
							text3: '-',
							text4: '-',
							text5: '-',
							text6: '-',
							text7: '-',
						},
					],
					seven: [
						{
							check1: ['Not Sure'],
							check2: ['Not Sure'],
							check3: ['Not Sure'],
							check4: ['Not Sure'],
							check5: ['Not Sure'],
							text1: '-',
							text2: '-',
							text3: '-',
							text4: '-',
							text5: '-',
						},
					],
					eight: [
						{
							check1: ['Not Sure'],
							check2: ['Not Sure'],
							text1: '-',
							text2: '-',
						},
					],
				},
				statusRestart: true,
				date: new Date().toLocaleDateString(),
			}
			var URL = this.$appurl + '/Report'
			await axios
				.post(URL, formData)
				.then(() => {
					this.confirm = ''
					this.$emit('saveDialog')
				})
				.catch((error) => console.log(error))
		},
	},
}
</script>

<style scoped>
.sky {
	color: #1e88e5;
}

button.v-btn[disabled] {
	opacity: 40%;
	color: #1e88e5 !important;
}
</style>

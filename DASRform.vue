<template>
	<v-app class="saraban">
		<!-- HEADER -->
		<header style="background-color:#171D41; color:white;height:100px;margin-bottom:40px">
			<div class="content py-4">
				<v-row align="center">
					<v-col cols="10">
						<img src="/img/logo/Original-White.png" width="166px" height="53px" />
					</v-col>
					<v-spacer />
					<v-col cols="2" class="d-flex justify-end">
						<v-overlay :value="overlay">
							<v-progress-circular indeterminate size="64" />
						</v-overlay>
						<v-menu offset-y origin="right top" left content-class="language-dropdown" transition="slide-y-transition" nudge-top="-10">
							<template #activator="{ on }">
								<v-btn icon x-large v-on="on" class="ml-2 v-step-3" style="color:white">
									<flag :iso="flag" :squared="false" class="ma-2" />
								</v-btn>
							</template>
							<div class="dropdown-content">
								<v-list class="dropdown-list">
									<v-list-item-group v-model="languageActive" color="primary">
										<v-list-item
											@click="
												changeLocale('en')
												flag = 'us'
												overlay = !overlay
											"
										>
											<flag iso="us" :squared="false" class="ma-2" />
											<span> English </span>
										</v-list-item>
										<v-list-item
											@click="
												changeLocale('th')
												flag = 'th'
												overlay = !overlay
											"
										>
											<flag iso="th" :squared="false" class="ma-2" />
											<span> ภาษาไทย </span>
										</v-list-item>
									</v-list-item-group>
								</v-list>
							</div>
						</v-menu>
					</v-col>
					<!-- <v-spacer></v-spacer> -->
				</v-row>
			</div>
		</header>

		<v-container fluid fill-height style="margin-bottom:40px">
			<!-- Check password -->
			<v-row v-if="!passwordSuccess" justify="center" align="center">
				<div class="d-flex flex-column align-center" style="width: 100%;">
					<v-icon color="#DADADA" size="100">mdi-lock-outline </v-icon>
					<v-text-field
						v-model="password"
						outlined
						type="password"
						:error-messages="verify"
						style="width:25%"
						height="40"
						class="mt-6"
						@keyup.enter="checkPassword()"
					/>
					<p style="font-size:1.8vh">{{ $t('DSAR.DSARForm.PageCheckPassword.Detail1') }}</p>
					<p style="font-size:1.8vh">{{ $t('DSAR.DSARForm.PageCheckPassword.Detail2') }}</p>
					<v-btn class="mt-6" style="background:#28A745; color:white; width:90px;font-size:1.8vh;" @click="checkPassword()">
						{{ $t('DSAR.DSARForm.Button.Confirm') }}
					</v-btn>
				</div>
			</v-row>
			<v-row v-else justify="center">
				<v-card width="20vw" color="#F8F8F8" style="margin-right:15px">
					<v-card-title class="p-head-font-size">{{ $t('DSAR.DSARForm.SubjectDetail') }}</v-card-title>
					<v-card-text style="font-size:1.8vh;color:#000000;" class="mt-n1 ln-text">
						<span class="font-weight-bold">{{ $t('DSAR.DSARForm.RequestOwner') }} :</span> {{ data_req.first_name }} {{ data_req.surname }}<br />
						<span class="font-weight-bold">{{ $t('DSAR.DSARForm.Email') }} :</span> {{ data_req.Email_ID }}<br />
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.Phone') }} :</span> {{ data_req.phone_number }}<br />
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.Category') }} :</span> {{ data_req.data_subject_type }}<br />
					</v-card-text>
					<v-divider style="margin:8px 15px 0px 15px" />
					<v-card-title class="p-head-font-size">{{ $t('DSAR.DSARForm.Detail') }}</v-card-title>
					<v-card-text style="font-size:1.8vh; color:#000000" class="mt-n1 ln-text">
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.StatusMain') }} :</span>
						<span class="font-weight-bold" :style="{ color: stepColor(cmpStatusHistoryLast) }"> {{ cmpCurrentStatus }}</span> <br />
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.RequestType') }} :</span> {{ data_req.data_subject }}<br />
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.Description') }} :</span> {{ data_req.objective }}<br />
						<span class="font-weight-bold">{{ $t('DSAR.DSARForm.StartDate') }} :</span> {{ data_req.history[0].date }}<br />
						<span class="font-weight-bold">{{ $t('DSAR.DSARForm.UpdateDate') }} :</span> {{ data_req.history[data_req.history.length - 1].date }}
					</v-card-text>
					<v-divider style="margin:8px 15px 0px 15px" />
					<v-card-title class="p-head-font-size">{{ $t('DSAR.DSARForm.ComplaintSupervisor') }}</v-card-title>
					<v-card-text style="font-size:1.8vh;color:#000000" class="mt-n1 ln-text">
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.AssignTo') }} :</span> {{ holder.Name }} <br />
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.Position') }} : </span>{{ holder.Position }}<br />
						<span class="font-weight-bold"> {{ $t('DSAR.DSARForm.Email') }} :</span> {{ holder.Email }}<br />
						<span class="font-weight-bold">{{ $t('DSAR.DSARForm.Phone') }} :</span> {{ holder.Tel }}
					</v-card-text>
					<v-divider style="margin:8px 15px 0px 15px" />
				</v-card>
				<v-card width="55vw" color="#F8F8F8">
					<!-- Show when Event Not finished -->
					<div v-if="stpNext !== '3'">
						<div class="mb-4 d-flex justify-center mt-4" style="width: 100%;">
							<div class="d-flex justify-start" style="width:85%">
								<p class="p-head-font-size">{{ $t('DSAR.DSARForm.HandleRequest') }}</p>
							</div>
						</div>
						<div class="d-flex justify-center mt-n10" style="width: 100%;">
							<div class="d-flex justify-start" style="width:85%">
								<v-divider />
							</div>
						</div>

						<div class="mt-n3 d-flex flex-row justify-center" style="width: 100%;">
							<div class="d-flex justify-end" style="width:85%">
								<p style="font-weight:500;font-size:1.8vh" class="mb-1">
									{{ $t('DSAR.DSARForm.RemainingDay') }} <span class="font-weight-bold">{{ cmpTimeLimit }}</span> {{ $t('DSAR.DSARForm.Day') }}
								</p>
							</div>
						</div>
					</div>
					<!-- Show when Event finished -->
					<div v-if="stpNext === '3'" class="mb-7">
						<div class="mb-4 mt-1 d-flex flex-row justify-center" style="width: 100%;">
							<div class="d-flex justify-center" style="width:85%">
								<v-icon color="success" size="40"> mdi-check-circle-outline </v-icon>
								<p class="mb-0 ml-1" style="font-weight:500;font-size:3.5vh;color:#28A745">{{ $t('DSAR.DSARForm.SuccessProcess.Detail1') }}</p>
							</div>
						</div>
						<div class="mb-4 mt-n4  d-flex flex-row justify-center" style="width: 100%;">
							<div class="d-flex justify-center" style="width:85%">
								<p style="font-weight:500;font-size:1.8vh" class="mb-1">
									{{ $t('DSAR.DSARForm.SuccessProcess.Detail2') }}
								</p>
							</div>
						</div>
					</div>
					<div class="mt-5 d-flex justify-center" style="width: 100%; height:580px">
						<v-card elevation="5" :loading="cardloading" class="card-process">
							<div class="content-scroll" style="padding-left:130px">
								<v-timeline dense>
									<v-slide-x-transition group>
										<v-timeline-item
											v-for="(item, idx) in cmpDataHistory"
											:key="idx"
											:color="stepColor(item.event)"
											:icon="stepIcon(item.event)"
											icon-color="white"
											fill-dot
											small
										>
											<v-card :color="stepColor(item.event)" dark style="height: auto; width: 65%; margin-left: -18px;">
												<v-card-title style="padding-top: 0.4vh; height: 4.5vh; font-size: 1.8vh">
													{{ item.event }}
												</v-card-title>
												<v-card-text style="background-color: #ffffff; color: black; padding-top: 0.5vh; font-size:1.7vh;">
													{{ item.date }}<br />
													{{ item.details }}<br />
													<v-row v-if="item.responsible.length > 0" class="mb-n3" justify="end" align="center">
														<div v-for="itm in item.responsible" :key="itm.keyId" class="mt-3">
															<b-avatar size="1.9rem" :variant="itm.color" :text="itm.shotName" style="margin-left:2px" />
														</div>
													</v-row>
												</v-card-text>
											</v-card>
										</v-timeline-item>
									</v-slide-x-transition>
								</v-timeline>
							</div>
							<v-btn :disabled="stpNext !== '1'" color="success" class="btn-add" fab @click="dialog2 = true">
								<v-icon size="50">mdi-plus</v-icon>
							</v-btn>
						</v-card>
					</div>
					<div class="mt-13 mb-7 d-flex flex-row justify-center" style="width: 100%;">
						<div class="d-flex justify-end" style="width:85%">
							<v-btn :disabled="cmpDisableBtnSendMail" @click="openDiSendEmailToDPO()" color="#28A745" style="color:white;">
								{{ $t('DSAR.DSARForm.Button.SuccessSendMail') }}
							</v-btn>
						</div>
					</div>
				</v-card>
			</v-row>
		</v-container>

		<!---- Dialog Add process and edit  ---->
		<v-overlay :value="dialog2" :dark="false">
			<v-card style="background-color:white;height:auto;width:649px;">
				<v-card-title class="headline pl-6">
					<p style="font-size:3vh;color:black;margin-bottom:0px 0px 0px">{{ $t('DSAR.DSARForm.DialogAddEvent.Title') }}</p>
					<v-spacer />
					<v-btn fab color="white" class="mb-2 mr-3 close-btn" @click="close2()">
						<v-icon size="35" color="black" dark> mdi-close-circle-outline </v-icon>
					</v-btn>
				</v-card-title>
				<v-card-text style="background-color: white;color: black;height:auto;" class="pt-3 ">
					<div class="d-flex justify-center">
						<v-card style="width:97%">
							<v-card-text>
								<div class="d-flex flex-row pl-2 pr-2">
									<p style="color:black">{{ $t('DSAR.DSARForm.DialogAddEvent.ActionType') }} :</p>
									<v-select
										:items="cmpStatusSelect"
										@input="selectAction($event)"
										:label="$t('DSAR.DSARForm.DialogAddEvent.ActionTypes')"
										dense
										outlined
										class="ml-1"
										style="width:50%;font-size:1.7vh;color:black;"
									>
										<template #item="{ item }">
											<div>
												<p style="font-size:1.7vh;">
													<v-icon :color="selectColor(item)" small>{{ selectIcon(item) }}</v-icon> {{ item }}
												</p>
											</div>
										</template>
									</v-select>
								</div>
								<v-divider />
								<div class="pl-2 pr-2">
									<p style="font-size:3vh;color:black">{{ $t('DSAR.DSARForm.DialogAddEvent.Detail') }} :</p>
									<v-textarea v-model="newHistory.details" solo height="120" :label="$t('DSAR.DSARForm.DialogAddEvent.PlaceHolderDetailProcess')" />
								</div>
							</v-card-text>
						</v-card>
					</div>
					<div class="d-flex justify-center">
						<div style="width:97%" class="d-flex justify-end mt-4">
							<v-btn
								elevation="2"
								color="#007BFF"
								style="font-size:1.6vh;color:white"
								:loading="buttonEmailloading"
								@click="updateHistory()"
								:disabled="newHistory.event == ''"
							>
								{{ $t('DSAR.DSARForm.Button.updateEvent') }}
							</v-btn>
						</div>
					</div>
				</v-card-text>
			</v-card>
		</v-overlay>

		<!-- Dialog send email To DPO -->
		<v-dialog v-model="dialogSendEmailToDPO" max-width="649" persistent>
			<v-card>
				<v-card-title class="headline pl-5">
					<p style="font-size:2.8vh;color:#000000;margin:0px 0px 0px 10px;font-weight:600">{{ $t('DSAR.DSARForm.DialogConfirmEmail.Title') }}</p>
					<v-spacer />
					<v-btn fab color="white" class="close-btn mt-3" @click="closeDiSendEmailToDPO()">
						<v-icon size="35" color="black" dark> mdi-close-circle-outline </v-icon>
					</v-btn>
				</v-card-title>
				<v-card-text>
					<div class="pl-2 pt-4 saraban">
						<p style="font-size:1.8vh;color:#000000;font-weight:500;">
							{{ $t('DSAR.DSARForm.DialogConfirmEmail.Detail1') }}
						</p>
						<p style="font-size:1.8vh;color:#000000;font-weight:600;">
							<span style="font-weight:700;"> {{ $t('DSAR.DSARForm.DialogConfirmEmail.Note') }} :</span>
							{{ $t('DSAR.DSARForm.DialogConfirmEmail.Detail2') }}
						</p>
					</div>
				</v-card-text>
				<v-card-actions class="justify-end mr-3">
					<v-btn
						style="background:#28A745; color:white; width:100px;font-weight:400;font-size:1.9vh;"
						@click="nextHistory()"
						:loading="buttonEmailloading"
						class="mb-2"
					>
						{{ $t('DSAR.DSARForm.Button.Confirm') }}
					</v-btn>
					<v-btn
						text
						style="width:100px;color:white;font-weight:400;background-color:#6C757D;font-size:1.9vh;"
						class="mb-2"
						@click="closeDiSendEmailToDPO()"
					>
						{{ $t('DSAR.DSARForm.Button.Cancel') }}
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>

		<!-- Footers -->
		<v-footer style="background-color:#171D41; color:white;height:100px;">
			<div class="content py-2">
				<b-row class="font-size-sm">
					<b-col sm="6" order-sm="2" class="text-center text-sm-right" style="font-size:1.8vh">
						Powered by <img class="img-avatar img-avatar32" src="/img/favicons/logo_32-32.png" />
						<a class="font-w600" href="https://www.ragnar.co.th" style="color:white" target="_blank">Ragnar</a>
					</b-col>
					<b-col sm="6" order-sm="1" class="text-center text-sm-left pt-4" style="font-size:1.8vh">
						<a class="font-w600" href="https://t-reg.co" style="color:white" target="_blank">{{
							$store.getters.appName + ' ' + $store.getters.appVersion
						}}</a>
						&copy;
						{{ $store.getters.appCopyright }}
					</b-col>
				</b-row>
			</div>
		</v-footer>
	</v-app>
</template>

<script>
import axios from 'axios'
// import i18n from '@/plugins/i18n'
import i18n from '../../plugins/i18n';

export default {
	data() {
		return {
			statusSelect: ['กำลังดำเนินการ', 'เกิดเหตุขัดข้อง', 'ดำเนินการล้มเหลว', 'ดำเนินการเสร็จสิ้น'],
			cardloading: false,
			passwordSuccess: false,
			dialog2: false,
			verify: null,
			password: '',
			data_req: {
				CustomerName: '',
				data_subject: [],
				history: [
					{
						date: '',
						details: '',
						event: '',
					},
				],
			},
			data_history: [
				{
					date: '',
					details: '',
					event: '',
				},
			],
			time: '',
			newHistory: {
				date: '',
				details: '',
				event: '',
			},
			customerName: '',
			buttonEmailloading: false,
			holder: {},
			stpNext: '',
			dtRequestNext: false,
			historyLast: [],
			dialogSendEmailToDPO: false,
			// *TODO: data language
			toggleLanguage: undefined,
			flag: 'th',
			languageActive: 1,
			overlay: false,
			updateLanguage: false,
			sendedMail: false,
		}
	},
	created() {
		this.checkLanguage()
		// this.password = 'nqGPdR4o'
		// this.password = 'WEWQz6UT'
		// this.password = '3rcegORD'
		this.checkPassword()
	},
	watch: {
		overlay(val) {
			if (val) {
				setTimeout(() => {
					this.overlay = false
				}, 500)
			}
		},
	},
	computed: {
		cmpHistoryLast() {
			const reverseArr = (input) => {
				const ret = []
				for (let i = input.length - 1; i >= 0; i--) {
					ret.push(input[i])
				}
				return ret
			}
			return reverseArr(this.historyLast)
		},
		cmpStatusHistoryLast() {
			// return this.cmpHistoryLast[0].event
			if (this.cmpHistoryLast[0].event === 'คำร้องเข้าสู่ระบบ') return this.$t('DSAR.DSARForm.Status.RequestToSystem')
			if (this.cmpHistoryLast[0].event === 'กำลังดำเนินการ') return this.$t('DSAR.DSARForm.Status.InProgress')
			if (this.cmpHistoryLast[0].event === 'เกิดเหตุขัดข้อง') return this.$t('DSAR.DSARForm.Status.HasProblem')
			if (this.cmpHistoryLast[0].event === 'ดำเนินการเสร็จสิ้น') return this.$t('DSAR.DSARForm.Status.Success')
			if (this.cmpHistoryLast[0].event === 'ดำเนินการล้มเหลว') return this.$t('DSAR.DSARForm.Status.Failed')
			return this.cmpStatusHistoryLast
		},
		cmpTimeLimit() {
			const days = 30 - Math.floor((Date.now() - Date.parse(this.time)) * 0.000000011574074)
			return days
		},
		cmpDisableBtnSendMail() {
			if (this.stpNext === '1' || this.stpNext === '3' || this.sendedMail === true) {
				return true
			}
			return false
		},
		// !Change Language i18n
		cmpCurrentStatus() {
			if (this.cmpStatusHistoryLast === 'คำร้องเข้าสู่ระบบ') return this.$t('DSAR.DSARForm.Status.RequestToSystem')
			if (this.cmpStatusHistoryLast === 'กำลังดำเนินการ') return this.$t('DSAR.DSARForm.Status.InProgress')
			if (this.cmpStatusHistoryLast === 'เกิดเหตุขัดข้อง') return this.$t('DSAR.DSARForm.Status.HasProblem')
			if (this.cmpStatusHistoryLast === 'ดำเนินการเสร็จสิ้น') return this.$t('DSAR.DSARForm.Status.Success')
			if (this.cmpStatusHistoryLast === 'ดำเนินการล้มเหลว') return this.$t('DSAR.DSARForm.Status.Failed')
			return this.cmpStatusHistoryLast
		},
		cmpStatusSelect() {
			// const statusSelect = ['กำลังดำเนินการ', 'เกิดเหตุขัดข้อง', 'ดำเนินการล้มเหลว', 'ดำเนินการเสร็จสิ้น']
			const statusSelect = [
				this.$t('DSAR.DSARForm.Status.InProgress'),
				this.$t('DSAR.DSARForm.Status.HasProblem'),
				this.$t('DSAR.DSARForm.Status.Failed'),
				this.$t('DSAR.DSARForm.Status.Success'),
			]

			return statusSelect
		},
		cmpDataHistory() {
			const newHis = JSON.parse(JSON.stringify(this.cmpHistoryLast))
			newHis.forEach((item) => {
				if (item.event === 'คำร้องเข้าสู่ระบบ') {
					item.event = this.$t('DSAR.DSARForm.Status.RequestToSystem')
					item.details = `${this.$t('DSAR.DSARForm.RequestToSystem.Detial1')} ${this.$t('DSAR.DSARForm.RequestToSystem.Detail2')}`
				}
				if (item.event === 'กำลังดำเนินการ') {
					item.event = this.$t('DSAR.DSARForm.Status.InProgress')
					if (item.details.includes('คำร้องถูกมอบหมายให้แก่')) {
						let allName = []
						item.responsible.forEach((item2) => {
							allName.push(item2.Name)
						})

						item.details = ` ${this.$t('DSAR.DSARForm.InProgress.Detail1')} ${allName.join(', ')} ${this.$t(
							'DSAR.DSARForm.InProgress.Detail2'
						)}`
					}
				}
				if (item.event === 'เกิดเหตุขัดข้อง') item.event = this.$t('DSAR.DSARForm.Status.HasProblem')
				if (item.event === 'ดำเนินการเสร็จสิ้น') item.event = this.$t('DSAR.DSARForm.Status.Success')
				if (item.event === 'ดำเนินการล้มเหลว') item.event = this.$t('DSAR.DSARForm.Status.Failed')
				return item
			})
			return newHis
		},
	},
	methods: {
		close2() {
			this.dialog2 = false
			this.newHistory = { date: '', details: '', event: '' }
		},
		async checkPassword() {
			const URL = `${this.$appurl}/DataSubjectPublic/${this.$route.params.id}/${this.$route.params.reqid}/${this.$route.params.memberId}/${this.$route.params.holderID}`
			await axios
				.get(URL, { headers: { Authorization: this.password } })
				.then(async (res) => {
					if (res.data === 'The conditional request failed') {
						this.verify = 'คุณกรอกรหัสไม่ถูกต้องเกิน 5 ครั้ง โปรดรอเวลา 5 นาทีเพื่อกรอกรหัสผ่านใหม่'
					} else if (res.data === 1 || res.data === 2 || res.data === 3 || res.data === 4) {
						this.verify = `รหัสไม่ถูกต้อง คุณสามารถกรอกรหัสผ่านได้อีก ${res.data} ครั้ง`
					} else if (res.data[1]) {
						this.data_req = res.data[0]
						this.customerName = this.$route.params.memberId
						this.data_req.data_subject = res.data[0].data_subject.toString()
						this.data_history = res.data[0].history
						this.time = res.data[0].date

						this.holder = res.data[0].holders.find((item) => item.keyId === this.$route.params.holderID)
						this.filterHistoryLast()
						this.nextStep()
						this.passwordSuccess = true
					}
				})
				.catch((error) => {
					console.log(error)
				})
		},
		async nextHistory() {
			let idxHis
			let itemHis
			let formData
			let URL
			this.data_history.forEach((item, idx) => {
				if ((item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') && item.responsible[0].keyId === this.$route.params.holderID) {
					idxHis = idx
					itemHis = item
				}
			})
			this.buttonEmailloading = true
			formData = {
				appurl: this.$appurl,
				CusName: `${this.data_req.first_name} ${this.data_req.surname}`,
				data_subject: this.data_req.data_subject,
				Email_ID: this.data_req.Email_ID,
				HoldName: this.holder.Name,
				phone_number: this.data_req.phone_number,
			}
			URL = `${this.$appurl}/DataSubjectPublic/${this.$route.params.memberId}/emailtoDPO`
			await axios
				.post(URL, formData)
				.then(() => {})
				.catch((error) => console.log(error))
			// !replace history[idx] 1
			formData = {
				index: idxHis,
				date: itemHis.date,
				event: itemHis.event,
				details: itemHis.details,
				responsible: itemHis.responsible,
				userSystem: itemHis.userSystem,
				sendedMail: true,
			}
			URL = `${this.$appurl}/DataSubjectPublic/${this.$route.params.id}/${this.customerName}`
			await axios
				.put(URL, formData, { headers: { Authorization: this.password } })
				.then(() => {})
				.catch((error) => console.log(error))

			this.sendedMail = true
			this.buttonEmailloading = false
			this.closeDiSendEmailToDPO()
		},
		async updateHistory() {
			this.buttonEmailloading = true
			let formData
			formData = {
				date: new Date().toLocaleString(),
				event: this.newHistory.event,
				details: this.newHistory.details,
				responsible: [this.holder],
				userSystem: false,
			}
			const URL = `${this.$appurl}/DataSubjectPublic/${this.$route.params.id}/${this.customerName}`
			this.cardloading = true
			await axios
				.put(URL, formData, { headers: { Authorization: this.password } })
				.then(async () => {
					if (this.newHistory.event === 'ดำเนินการล้มเหลว' || this.newHistory.event === 'ดำเนินการเสร็จสิ้น') {
						const formDataBf = {
							date: new Date().toLocaleString(),
							event: this.newHistory.event,
							details: this.newHistory.details,
							responsible: [this.holder],
							userSystem: false,
						}
						this.newHistory.details = `ผู้มอบหมาย ${this.holder.Name} ได้${this.newHistory.event}  ${this.newHistory.details} ขณะนี้อยู่ระหว่างรอผู้ดูแลระบบตรวจสอบ`
						this.newHistory.event = 'รอการตรวจสอบ'

						formData = {
							date: new Date().toLocaleString(),
							event: this.newHistory.event,
							details: this.newHistory.details,
							responsible: [this.holder],
							userSystem: false,
						}
						await axios
							.put(URL, formData, { headers: { Authorization: this.password } })
							.then(async () => {
								this.buttonEmailloading = false
								this.data_history.push(formDataBf)
								this.filterHistoryLast()
								this.nextStep()
								this.cardloading = false
								this.close2()
							})
							.catch((error) => console.log(error))
					} else {
						this.buttonEmailloading = false
						this.data_history.push(formData)
						this.filterHistoryLast()
						this.nextStep()
						this.cardloading = false
						this.close2()
					}
				})
				.catch((error) => console.log(error))
		},
		filterHistoryLast() {
			let idxLast
			const newHis = []
			let newHisCp = []
			const obj = {
				front: false,
				back: false,
			}
			// *find idx last update this user keyId
			// for (let i in this.data_history) {
			// 	if (this.data_history[i].userSystem === false && this.data_history[i].responsible[0].keyId === this.$route.params.holderID) {
			// 		idxLast = i
			// 	}
			// 	//*Keep sended mail
			// 	if (
			// 		(this.data_history[i].event === 'ดำเนินการเสร็จสิ้น' || this.data_history[i].event === 'ดำเนินการล้มเหลว') &&
			// 		this.data_history[i].responsible[0].keyId === this.$route.params.holderID
			// 	) {
			// 		this.sendedMail = this.data_history[i].sendedMail === undefined ? false : true
			// 	}
			// }
			// *find idx last update this user keyId
			this.data_history.forEach((item, idx) => {
				if (item.userSystem === false && item.responsible[0].keyId === this.$route.params.holderID) {
					idxLast = idx
				}
				// *Keep sended mail
				if ((item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') && item.responsible[0].keyId === this.$route.params.holderID) {
					// this.sendedMail = item.sendedMail === undefined ? false : true
					this.sendedMail = item.sendedMail !== undefined
				}
			})
			// *ถ้าเคยดำเนินการแล้ว
			if (idxLast !== undefined) {
				// !หาพาร์ทหน้า
				for (let i = 0; i <= idxLast; i++) {
					const { event: ev } = this.data_history[i]
					if (ev !== 'รอการตรวจสอบ' && ev !== 'ตอบกลับโดย EMAIL' && ev !== 'ตอบกลับโดย SMS') {
						newHis.push(this.data_history[i])
					}
				}
				const hasSucOrFaild1 = newHis.filter((item) => {
					if ((item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') && item.responsible[0].keyId === this.$route.params.holderID) {
						return true
					}
					return false
				})
				// !หาพาร์ทหลัง
				newHisCp = JSON.parse(JSON.stringify(newHis))
				for (let i = parseInt(idxLast, 10) + 1; i <= this.data_history.length - 1; i++) {
					const ev = this.data_history[i].event
					if (ev !== 'รอการตรวจสอบ' && ev !== 'ตอบกลับโดย EMAIL' && ev !== 'ตอบกลับโดย SMS') {
						newHisCp.splice(i, 0, this.data_history[i])
					}
				}
				const hasSucOrFaild2 = newHisCp.filter((item) => {
					if ((item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') && item.responsible[0].keyId === this.$route.params.holderID) {
						return true
					}
					return false
				})

				if (hasSucOrFaild1.length > 0) obj.front = true
				if (hasSucOrFaild2.length > 0) obj.back = true

				if (obj.front === false && obj.back === false) {
					this.historyLast = newHisCp
				} else if (obj.front === true) {
					this.historyLast = newHis
				}
			} else {
				// *ถ้ายังไม่เคยดำเนินการแล้ว
				const newHis2 = []
				this.data_history.forEach((item) => {
					if (item.event !== 'รอการตรวจสอบ' && item.event !== 'ตอบกลับโดย EMAIL' && item.event !== 'ตอบกลับโดย SMS') {
						newHis2.push(item)
					}
				})
				this.historyLast = JSON.parse(JSON.stringify(newHis2))
			}
		},
		nextStep() {
			this.stpNext = '1'
			const { event } = this.historyLast[this.historyLast.length - 1]
			const { keyId } = this.historyLast[this.historyLast.length - 1].responsible[0]
			const { endRequest } = this.data_req
			if ((event === 'ดำเนินการเสร็จสิ้น' || event === 'ดำเนินการล้มเหลว') && keyId === this.$route.params.holderID) {
				this.stpNext = '2'
			}
			if (endRequest === true) this.stpNext = '3'
		},
		stepColor(text) {
			let color
			if (text === this.$t('DSAR.DSARForm.Status.RequestToSystem')) {
				color = '#212121'
			}
			if (text === this.$t('DSAR.DSARForm.Status.InProgress')) {
				color = '#007BFF'
			}
			if (text === this.$t('DSAR.DSARForm.Status.HasProblem')) {
				color = '#FFB51E'
			}
			if (text === this.$t('DSAR.DSARForm.Status.Failed')) {
				color = 'grey'
			}
			if (text === this.$t('DSAR.DSARForm.Status.Success')) {
				color = '#009834'
			}
			return color
		},
		stepIcon(text) {
			let icon
			if (text === this.$t('DSAR.DSARForm.Status.RequestToSystem')) {
				icon = 'mdi-email-outline'
			}
			if (text === this.$t('DSAR.DSARForm.Status.InProgress')) {
				icon = 'mdi-circle-slice-6'
			}
			if (text === this.$t('DSAR.DSARForm.Status.HasProblem')) {
				icon = 'mdi-alert-box'
			}
			if (text === this.$t('DSAR.DSARForm.Status.Failed')) {
				icon = 'mdi-close-box'
			}
			if (text === this.$t('DSAR.DSARForm.Status.Success')) {
				icon = 'mdi-checkbox-marked'
			}
			return icon
		},
		selectColor(item) {
			let color
			if (item === 'กำลังดำเนินการ') color = '#1F6DF9'
			if (item === 'เกิดเหตุขัดข้อง') color = '#FFB51E '
			if (item === 'ดำเนินการล้มเหลว') color = 'red'
			if (item === 'ดำเนินการเสร็จสิ้น') color = '#009834'
			return color
		},
		selectIcon(item) {
			let icon
			if (item === 'กำลังดำเนินการ') icon = 'mdi-circle-slice-6'
			if (item === 'เกิดเหตุขัดข้อง') icon = 'mdi-alert-box'
			if (item === 'ดำเนินการล้มเหลว') icon = 'mdi-close-box'
			if (item === 'ดำเนินการเสร็จสิ้น') icon = ' mdi-checkbox-marked'
			return icon
		},
		checkLanguage() {
			// this.flag =
			// sessionStorage.getItem('language') && sessionStorage.getItem('language') === 'en'
			// ? 'us'
			// : sessionStorage.getItem('language') !== 'null'
			// ? sessionStorage.getItem('language')
			// : 'th'
			if (sessionStorage.getItem('language') && sessionStorage.getItem('language') === 'en') {
				this.flag = 'us'
			}
			if (sessionStorage.getItem('language') !== 'null') {
				this.flag = sessionStorage.getItem('language')
			} else {
				this.flag = 'th'
			}

			this.changeLocale(this.flag)
		},
		changeLocale(locale) {
			i18n.locale = locale
			this.updateLanguage = !this.updateLanguage
		},
		openDiSendEmailToDPO() {
			this.dialogSendEmailToDPO = true
		},
		closeDiSendEmailToDPO() {
			this.dialogSendEmailToDPO = false
		},
		selectAction(e) {
			if (e === this.$t('DSAR.DSARForm.Status.InProgress')) {
				this.newHistory.event = 'กำลังดำเนินการ'
			} else if (e === this.$t('DSAR.DSARForm.Status.HasProblem')) {
				this.newHistory.event = 'เกิดเหตุขัดข้อง'
			} else if (e === this.$t('DSAR.DSARForm.Status.Failed')) {
				this.newHistory.event = 'ดำเนินการล้มเหลว'
			} else if (e === this.$t('DSAR.DSARForm.Status.Success')) {
				this.newHistory.event = 'ดำเนินการเสร็จสิ้น'
			}
		}
	},
}
</script>

<style scoped>
@import 'https://fonts.googleapis.com/css?family=Kanit|Prompt';
@import 'https://fonts.googleapis.com/css?family=Sarabun|Prompt';

.saraban {
	font-family: 'Sarabun', sans-serif;
}
.p-head-font-size {
	font-size: 2vh;
	font-weight: bold;
	width: 95%;
}
.card-process {
	background-color: #ffffff;
	position: relative !important;
	width: 85%;
	height: 600px;
}
.content-scroll {
	overflow: auto;
	width: 100%;
	height: 100%;
}
.btn-add {
	position: absolute !important;
	bottom: 0.8rem !important;
	right: 1.9rem !important;
}
.ln-text {
	line-height: 2;
}
.close-btn {
	width: 26px;
	height: 26px;
}
</style>

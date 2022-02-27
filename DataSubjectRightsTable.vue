<template>
	<v-main app>
		<div v-if="!onlyRequestNotReceived" class="form row col-md-12" style="margin-bottom: 0px; padding-bottom: 0px">
			<v-chip color="#F3F8FC" style="width: 300px; height: 30px; padding-right: 20px; padding-left: 20px">
				<v-text-field
					v-model="search"
					append-icon="mdi-magnify"
					label="Search Here..."
					single-line
					hide-details
					style="margin-top: 0px; padding-top: 0px; width: 500px; height: 30px"
				/>
			</v-chip>
			<v-spacer />
			<p>{{ $t('DSAR.Filter.Title') }} :</p>
			<v-chip color="#F3F8FC" style="width: 200px; height: 30px">
				<v-select :items="dataSubjectTypeChoice" label="ประเภทเจ้าของข้อมูล" v-model="dataSubjectTypeFillter" dense />
			</v-chip>
			<v-chip color="#F3F8FC" style="width: 200px; height: 30px">
				<v-select :items="costumerChoice" label="ประเภทคำร้อง" v-model="costumerFillter" dense />
			</v-chip>
			<v-chip color="#F3F8FC" style="width: 168px; height: 30px">
				<v-select :items="statusChoice" label="สถานะ" v-model="statusFillter" dense />
			</v-chip>
			<v-chip color="#F3F8FC" style="width: 165px; height: 30px">
				<v-select :items="durationChoice" label="ระยะเวลาที่เหลือ" v-model="durationFillter" dense />
			</v-chip>
			<v-btn text color="primary" @click="refresh()" style="margin-top: -5px; margin-left: 15px">
				<v-icon color="primary"> mdi-refresh </v-icon>
				{{ $t('DSAR.Button.Refresh') }}
			</v-btn>
		</div>
		<br />
		<br />

		<v-data-table
			:headers="headers"
			:items="databaseNewTable"
			:item-class="rowClass"
			:search="search"
			:footer-props="{ 'items-per-page-options': [5, 10, 15, -1] }"
			:footer="{ class: 'rowClass' }"
			:loading="tableloading"
			sort-by="timeStamp"
			:sort-desc="true"
			loading-text="กำลังโหลดข้อมูล รอสักครู่..."
			style="font-family: 'Sarabun', sans-serif"
		>
			<template #[`item.timeStamp`]="{ item }">
				{{ item.date }}
			</template>
			<template #[`item.working`]="{ item }">
				<v-badge
					class="red_small_dot"
					color="error"
					offset-x="11"
					offset-y="11"
					overlap
					value="0"
					v-if="item.working == 'คำร้องเข้าสู่ระบบ' || item.working == 'รอการตรวจสอบ'"
				>
					<v-chip :color="color(item.working, item.lim)" text-color="white">
						{{ item.working }}
					</v-chip>
				</v-badge>
				<v-chip
					:color="color(item.working, item.lim)"
					text-color="white"
					v-if="item.working != 'คำร้องเข้าสู่ระบบ' && item.working != 'รอการตรวจสอบ'"
				>
					{{ item.working }}
				</v-chip>
			</template>
			<template #[`item.lim`]="{ item }">
				<p :style="style(item.working, item.lim)">{{ item.limit }}</p>
			</template>
			<template #[`item.objective`]="{ item }"> {{ item.objective }}<br />{{ $t('DSAR.RefDSAR') }} : {{ item.referrerName }} </template>
			<template #[`item.num`]="{ item }">
				<v-menu bottom offset-x>
					<template #activator="{ on, attrs }">
						<v-btn icon v-bind="attrs" v-on="on">
							<v-icon>mdi-dots-horizontal</v-icon>
						</v-btn>
					</template>
					<v-list>
						<v-list-item v-if="$can('read', 'DSARTransaction') || $can('update', 'DSARTransaction')" @click="complete(item.num)">
							<v-list-item-title>{{ $t('DSAR.Button.HandleRequest') }}</v-list-item-title>
						</v-list-item>
						<v-list-item v-if="$can('delete', 'DSARTransaction')" @click="remove(item)">
							<v-list-item-title>{{ $t('DSAR.Button.Delete') }}</v-list-item-title>
						</v-list-item>
					</v-list>
				</v-menu>
			</template>
		</v-data-table>

		<b-modal ref="my-modal1" id="modal-center" centered title="T-Reg">
			<p class="my-4">{{ $t('DSAR.DialogDSAR.DeleteText1') }} "{{ name }}" {{ $t('DSAR.DialogDSAR.DeleteText2') }}</p>
			<template #modal-footer>
				<b-button variant="danger" style="color: white; padding-right: 22px; padding-left: 22px" @click="deleteButton(i)">{{
					$t('DSAR.DialogDSAR.Button.Delete')
				}}</b-button>
				<b-button variant="light" @click="cancelButton()">{{ $t('DSAR.DialogDSAR.Button.Cancel') }}</b-button>
			</template>
		</b-modal>
		<b-modal ref="my-modal2" id="modal-center" centered title="T-Reg">
			<p class="my-4">คุณดำเนินการตามคำร้องของ "{{ name }}" เสร็จสมบูรณ์แล้ว ใช่หรือไม่?</p>
			<template #modal-footer>
				<b-button variant="success" style="color: white; padding-right: 22px; padding-left: 22px" @click="completeButton(i)">
					ดำเนินการเสร็จแล้ว
				</b-button>
				<b-button variant="light" @click="cancelButton()">ยกเลิก</b-button>
			</template>
		</b-modal>
		<b-modal ref="my-modal3" id="modal-center" centered title="T-Reg">
			<p class="my-4">คุณดำเนินการตามคำร้องของ "{{ name }}" เสร็จสมบูรณ์แล้ว ใช่หรือไม่?</p>
			<template #modal-footer>
				<b-button variant="light" @click="cancelButton()">ยกเลิก</b-button>
			</template>
		</b-modal>

		<!-- Dialog process and edit -->
		<v-dialog v-model="dialog1" persistent width="90vw" max-width="1000px" style="position: absolute; z-index: 20001">
			<v-card width="100vw" min-width="700px" style="background-color: #ffffff; overflow-x: hidden; overflow-y: hidden">
				<!---- Dialog Add process and edit  ---->
				<v-overlay :value="dialog2" :dark="false">
					<v-card style="background-color:white;height:auto;width:600px;margin:30px 0px 0px 40px;">
						<v-card-title class="headline pl-5">
							<p style="font-size:3vh;color:black;margin-bottom:0px">{{ $t('DSAR.DialogDSAR.SubCardTitle') }}</p>
							<v-spacer />
							<v-btn fab x-small color="white" @click="close2()">
								<v-icon size="37" color="black" dark> mdi-close-circle-outline </v-icon>
							</v-btn>
						</v-card-title>
						<v-card-text style="background-color: white;color: black;height:auto;" class="pt-3 ">
							<div class="d-flex justify-center">
								<v-card style="width:90%">
									<v-card-text>
										<div class="d-flex flex-row pl-2 pr-2">
											<p style="color:black">{{ $t('DSAR.DialogDSAR.ActionType') }} :</p>
											<!-- <v-select
												:items="cmpStatusSelect"
												v-model="newHistory.event"
												:label="$t('DSAR.DialogDSAR.ActionTypes')"
												dense
												outlined
												class="ml-1"
												style="width:50%;font-size:1.7vh;color:black;"
											> -->
											<v-select
												:items="cmpStatusSelect"
												@input="mtSelectAction($event)"
												:label="$t('DSAR.DialogDSAR.ActionTypes')"
												dense
												outlined
												class="ml-1"
												style="width:50%;font-size:1.7vh;color:black;"
											>
												<template #item="{ item }">
													<div style="height:50px">
														<p style="font-size:1.7vh;">
															<v-icon :color="selectColor(item)" small>{{ selectIcon(item) }}</v-icon> {{ item }}
														</p>
													</div>
												</template>
											</v-select>
										</div>
										<v-divider />
										<div class="pl-2 pr-2">
											<p style="font-size:3vh;color:black">{{ $t('DSAR.DialogDSAR.DetailsPro') }} :</p>
											<p
												class="mt-n5"
												style="font-size:1.9vh;color:black"
												v-if="newHistory.event === 'ดำเนินการเสร็จสิ้น' || newHistory.event === 'ดำเนินการล้มเหลว'"
											>
												{{ $t('DSAR.DialogDSAR.Note') }}: {{ $t('DSAR.DialogDSAR.DetailSucFailed') }}
											</p>
											<v-textarea v-model="newHistory.details" solo height="120" :label="$t('DSAR.DialogDSAR.PlaceHolderDetailProcess')" />
										</div>
										<v-btn
											v-if="$can('update','DSARTransaction')"
											elevation="2"
											style="font-size:1.6vh;font-weight: bold;left: 65%;background:#3E66FB;color:white"
											:loading="buttonEmailloading"
											@click="updateHistory()"
											:disabled="newHistory.event == ''"
										>
											{{ $t('DSAR.DialogDSAR.Button.AddProcess') }}
										</v-btn>
									</v-card-text>
								</v-card>
							</div>
						</v-card-text>
					</v-card>
				</v-overlay>

				<!--- Dialog Process and Edit  --->
				<v-card flat class="mb-5" style="background-color: #ffffff">
					<v-card-title class="pl-8 pt-4">
						<p style="font-size: 4vh; color: black">{{ $t('DSAR.DialogDSAR.CardTitle') }}</p>
						<v-spacer />
						<v-btn fab small color="white" @click="close()">
							<v-icon size="45" color="black" dark> mdi-close-circle-outline </v-icon>
						</v-btn>
					</v-card-title>
					<v-row>
						<!-- Show All step -->
						<v-col cols="6">
							<v-card elevation="5" class="d-flex flex-column ml-8 pl-2 pr-2" style="background-color: #ffffff; height: 100%">
								<p class="m-0 pt-2 p-head-font-size">
									{{ $t('DSAR.DialogDSAR.RequestOwner') }}
								</p>
								<p class="m-0 mt-2 p-font-size">
									<b> {{ $t('DSAR.DialogDSAR.Fullname') }} :</b> {{ data_name }}
								</p>
								<p class="m-0 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.Email') }} :</b> {{ data_email }}
								</p>
								<p class="m-0 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.Phone') }} :</b> {{ data_number }}
								</p>
								<p class="m-0 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.Category') }} :</b> {{ data_category }}
								</p>
								<v-divider />
								<p class="m-0 p-head-font-size">
									{{ $t('DSAR.DialogDSAR.RequestDetail') }}
								</p>
								<p class="m-0 mt-2 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.Status') }} :</b> {{ cmpDataStatus }}
								</p>
								<p class="m-0 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.RequestType') }} :</b> {{ data_type.toString() }}
								</p>
								<p class="m-0 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.Description') }} :</b> {{ data_objective }}
								</p>
								<p class="m-0 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.StartDate') }} :</b> {{ data_startdate }}
								</p>
								<p class="m-0 p-font-size">
									<b>{{ $t('DSAR.DialogDSAR.UpdateDate') }} :</b> {{ data_recentdate }}
								</p>
								<v-row align-content="end" class="ml-1 mb-2">
									<v-btn
										elevation="2"
										color="#3E66FB"
										style="font-size: 1.6vh; height: 50px; color: white"
										v-if="$can('update', 'DSARTransaction') || $can('update', 'ConsentTransaction')"
										@click="allConsentCheck()"
									>
										{{ $t('DSAR.DialogDSAR.Button.CheckConsent') }}
									</v-btn>
								</v-row>
							</v-card>
						</v-col>
						<v-col cols="6">
							<v-card elevation="5" class="mr-8 pl-2 pr-2" style="background-color: #ffffff; height: 100%">
								<!-- Show Step1 -->
								<div v-if="stpProcess === '1'">
									<div class="pl-2 pr-2">
										<p class="m-0 pt-2 p-head-font-size ">{{ $t('DSAR.DialogDSAR.AddResponsibleSystem') }}</p>
										<div class="d-flex flex-row mt-0 mt-sm-0 mt-md-3 mt-sm-1">
											<div class="rounded-lg" style="min-height: 50%; border: 1px solid black; width: 100%">
												<div v-for="(item, index) in datasRSPS" :key="index" class="d-inline">
													<div v-if="datasRSPS.length <= 6" class="d-inline">
														<b-avatar size="5.5vh" :variant="item.color" :text="item.shotName" class="mt-1 mb-1 ml-1" />
													</div>
													<div v-else class="d-inline">
														<b-avatar
															v-if="index === 0"
															size="5.5vh"
															:variant="item.color"
															:text="item.shotName"
															class="mt-1 mb-1 ml-md-1 ml-1"
														/>
														<b-avatar
															v-else
															size="5.5vh"
															:variant="item.color"
															:text="item.shotName"
															class="mt-1 mb-1 ml-xs-n1 ml-1 ml-sm-1 ml-lg-n4"
														/>
													</div>
												</div>
											</div>
											<v-btn
												:disabled="!$can('update', 'DSARTransaction')"
												color="#2E4CBC"
												style="height: 50px; "
												class="ml-3"
												@click="trigDialogSearchResponsible()"กร
											>
												<v-icon color="white"> mdi-account-multiple-plus-outline </v-icon>
											</v-btn>
										</div>
										<p class="ml-1 mt-0" style="font-size: 12px">{{ datasRSPS.length }}/10</p>
										<v-divider class="mt-0 mt-xs-0 mt-sm-0 mt-md-0 mt-lg-0" />
										<div class="d-flex justify-space-between">
											<p class="p-head-font-size">{{ $t('DSAR.DialogDSAR.AddResponsibleManual') }}</p>
											<v-btn
												:disabled="cmpBtnAdvance"
												@click="clickBtnAdvance"
												:outlined="btnAdvance"
												:color="btnAdvance ? 'black' : '#3e66fb'"
												:style="{ color: btnAdvance ? 'black' : 'white' }"
												style="width: 110px; height: 27px; color;font-size:1.9vh"
											>
												{{ $t('DSAR.DialogDSAR.Button.Advance') }}
											</v-btn>
										</div>
										<v-form ref="form">
											<v-text-field
												v-model="holder.name"
												:rules="nameRules"
												:label="$t('DSAR.DialogDSAR.Label.AssignName')"
												outlined
												:disabled="!btnAdvance"
												style="height: 50px;font-size:2vh"
											/>
											<v-text-field
												v-model="holder.email"
												:rules="emailRules"
												:label="$t('DSAR.DialogDSAR.Label.Email')"
												outlined
												:disabled="!btnAdvance"
												style="height: 50px;font-size:2vh "
												class="mt-5"
											/>
											<v-text-field
												v-model="holder.tel"
												:rules="telRules"
												:label="$t('DSAR.DialogDSAR.Label.Phone')"
												outlined
												:disabled="!btnAdvance"
												style="height: 50px;font-size:2vh"
												class="mt-5"
											/>
											<v-text-field
												v-model="holder.position"
												:rules="nameRules"
												:label="$t('DSAR.DialogDSAR.Label.Position')"
												outlined
												:disabled="!btnAdvance"
												style="height: 50px;font-size:2vh"
												class="mt-5"
											/>
										</v-form>
										<v-row justify="end" class="mb-2 mt-4 mr-2">
											<v-btn
												v-if="btnAdvance"
												class="mr-3"
												style="color: white;font-size:1.9vh"
												color="#2E4CBC"
												min-width="20"
												@click="mtAddManualRSPS()"
											>
												{{ $t('DSAR.DialogDSAR.Button.Save') }}
											</v-btn>
											<v-btn :disabled="cmpDisableAcceptReq" elevation="2" style="width: 150px;font-size:1.9vh" @click="mtAcceptRequest()">
												{{ $t('DSAR.DialogDSAR.Button.Submit') }}
											</v-btn>
										</v-row>
									</div>
								</div>
								<!-- Show Step2 & Step3  -->
								<div v-if="cmpShowSt2St3" class="pl-2 pr-2">
									<p class="m-0 pt-2 p-head-font-size">{{ $t('DSAR.DialogDSAR.Responsible') }}</p>
									<div class="d-flex flex-row mt-0 mt-sm-0 mt-md-4 mt-sm-2">
										<div class="rounded-lg" style="min-height: 50px; border: 1px solid black; width: 100%">
											<div v-for="(item, index) in datasRSPS" :key="index" class="d-inline">
												<div v-if="datasRSPS.length <= 8" class="d-inline">
													<!-- <v-tooltip top v-if="item.userSystem && index === 0" color="black">
														<template v-slot:activator="{ on, attrs }">
															<span v-bind="attrs" v-on="on">
																<b-avatar size="5.5vh" :variant="item.color" :text="item.shotName" class="mt-1 mb-1 ml-md-2 ml-2"></b-avatar>
															</span>
														</template>
														<span>{{ item.Email }}</span>
													</v-tooltip> -->
													<b-avatar
														v-if="index === 0"
														size="5.5vh"
														:variant="item.color"
														:text="item.shotName"
														class="mt-1 mb-1 ml-md-2 ml-2"
													/>
													<b-avatar
														v-else-if="index === 1"
														size="5.5vh"
														:variant="item.color"
														:text="item.shotName"
														class="mt-1 mb-1 ml-1"
													/>
													<b-avatar v-else size="5.5vh" :variant="item.color" :text="item.shotName" class="mt-1 mb-1 ml-1" />
												</div>
												<div v-else class="d-inline">
													<!-- <v-tooltip top v-if="item.userSystem && index === 0" color="black">
														<template v-slot:activator="{ on, attrs }">
															<span v-bind="attrs" v-on="on">
																<b-avatar size="5.5vh" :variant="item.color" :text="item.shotName" class="mt-1 mb-1 ml-md-2 ml-2"></b-avatar>
															</span>
														</template>
														<span>{{ item.Email }}</span>
													</v-tooltip> -->
													<b-avatar v-if="index === 0" size="5.5vh" :variant="item.color" :text="item.shotName" class="mt-1 mb-1 ml-md-2 ml-2" />
													<b-avatar
														v-else-if="index === 1"
														size="5.5vh"
														:variant="item.color"
														:text="item.shotName"
														class="mt-1 mb-1 ml-2 ml-sm-2 ml-md-2 ml-lg-n1"
													/>
													<b-avatar
														v-else
														size="5.5vh"
														:variant="item.color"
														:text="item.shotName"
														class="mt-1 mb-1 ml-xs-2 ml-2 ml-sm-2 ml-lg-n2 "
													/>
												</div>
											</div>
										</div>
									</div>
									<p class="ml-1 mt-0" style="font-size: 12px">{{ datasRSPS.length }}/10</p>
									<v-divider class="mt-5 mt-xs-5 mt-sm-5 mt-md-5 mt-lg-5" />
									<div class="d-flex justify-space-between">
										<p class="p-head-font-size">{{ $t('DSAR.DialogDSAR.RequestAction') }}</p>
										<v-btn
											class="text-capitalize bg-white mt-1"
											style="width: 120px; height: 22px; border: 1px solid #5383b2;font-size:1.85vh"
											@click="openDialogFullProgress()"
										>
											<v-icon left class="pl-2"> mdi-fullscreen </v-icon>
											{{ $t('DSAR.DialogDSAR.FullScreen') }}
										</v-btn>
									</div>
									<v-card
										elevation="5"
										:loading="cardloading2"
										style="height: 40vh; width: 100%; overflow-y: scroll"
										class="actv-timeline-progress-ddsr"
									>
										<v-timeline dense>
											<v-slide-x-transition group>
												<!-- <v-timeline-item
													v-for="(item, idx) in data_history"
													:key="idx"
													:color="stepColor(item.event)"
													:icon="stepIcon(item.event)"
													icon-color="white"
													fill-dot
													small
												> -->
												<v-timeline-item
													v-for="(item, idx) in cmpDataHistory"
													:key="idx"
													:color="stepColor(item.event)"
													:icon="stepIcon(item.event)"
													icon-color="white"
													fill-dot
													small
												>
													<v-card :color="stepColor(item.event)" dark style="height: auto; width: 100%; margin-left: -18px;">
														<v-card-title style="padding-top: 0.1vh; height: 4.5vh; font-size: 2.5vh">
															{{ item.event }}
														</v-card-title>
														<v-card-text style="background-color: #ffffff; color: black; padding-top: 0.5vh; font-size:1.7vh;">
															{{ item.date }}<br />
															{{ item.details }}<br />
															<!-- <p v-if="item.event === 'คำร้องเข้าสู่ระบบ'">{{ item.details2 }}</p> -->
															<v-row v-if="item.responsible.length > 0" justify="end" align="center">
																<!-- {{item.responsible}} -->
																<div v-for="itm in item.responsible" :key="itm.keyId" class="mt-3">
																	<!-- <b-avatar size="1.6rem" variant="primary" :text="itm.shotName"></b-avatar> -->
																	<!-- <v-tooltip top v-if="itm.userSystem" color="black">
																		<template v-slot:activator="{ on, attrs }">
																			<span v-bind="attrs" v-on="on">
																				<b-avatar size="1.4rem" :variant="itm.color" :text="itm.shotName" style="margin-right:4px"></b-avatar>
																			</span>
																		</template>
																		<span>{{ itm.Email }}</span>
																	</v-tooltip> -->
																	<b-avatar size="1.5rem" :variant="itm.color" :text="itm.shotName" style="margin-left:3px" />
																</div>
															</v-row>
														</v-card-text>
													</v-card>
												</v-timeline-item>
											</v-slide-x-transition>
										</v-timeline>
									</v-card>
									<v-row justify="end" class="mb-2 mt-4 mr-2" v-if="stpProcess === '2'">
										<v-btn :disabled="!$can('update','DSARTransaction')" class="mr-3" style="color: white;font-size: 1.6vh;background:#3E66FB" min-width="20" @click="addTimeline()">
											{{ $t('DSAR.DialogDSAR.Button.AddProcess') }}
										</v-btn>

										<v-btn
											:disabled="stpNext === '3' ? false : true"
											elevation="2"
											style="width: 100px;font-size: 1.6vh;"
											@click="openCfRequstNext()"
										>
											{{ $t('DSAR.DialogDSAR.Button.Next') }}
										</v-btn>
									</v-row>
									<v-row justify="end" class="mb-2 mt-4 mr-2" v-if="stpProcess === '3'">
										<v-btn class="mr-3 btn-sms-email" v-if="$can('update', 'DSARTransaction')" @click="openDialogSendSMS()">
											{{ $t('DSAR.DialogDSAR.Button.SendSMS') }}
										</v-btn>
										<v-btn class="btn-sms-email" v-if="$can('update', 'DSARTransaction')" @click="openDialogSendEmailToRQ()">
											{{ $t('DSAR.DialogDSAR.Button.SendEmail') }}
										</v-btn>
									</v-row>
								</div>
							</v-card>
						</v-col>
					</v-row>
				</v-card>
			</v-card>
		</v-dialog>

		<!-- Dialog Show example Send Email To Team All -->
		<v-dialog v-model="dialog3" persistent width="557px" :z-index="2">
			<v-alert
				v-if="successSendEmail"
				type="success"
				class="animate__animated animate__fadeInDown"
				style="position: fixed; z-index: 1; top: 25%; right: 43%; width: 220px; font-family: 'Sarabun', sans-serif"
			>
				{{ $t('DSAR.DialogDSAR.DSARDone') }}
			</v-alert>
			<v-card width="557px" height="625px" style="margin-top: 0px; overflow-x: hidden">
				<v-card-title class="headline" style="padding-left: 20px">
					<v-spacer />
					<v-btn fab x-small color="white" @click="close3()">
						<v-icon size="37" color="black" dark> mdi-close-circle-outline </v-icon>
					</v-btn>
				</v-card-title>
				<v-row style="display: flex; justify-content: center; align-items: center">
					<v-card outlined width="357px" height="476px" style="margin-bottom: 20px; margin-top: 0px">
						<v-img contain max-height="476px" max-width="357px" src="\img\photos\DSAR_imgSer.png">
							<template #placeholder>
								<v-skeleton-loader height="476px" width="357px" type="card-avatar, article" />
							</template>
						</v-img>
					</v-card>
				</v-row>
				<v-btn
					elevation="2"
					color="primary"
					:loading="buttonEmailloading"
					class="font-weight-bold mt-3"
					style="position: absolute;right: 100px;"
					@click="mtSendEmailToTeamAll()"
				>
					{{ $t('DSAR.DialogDSAR.Button.SendEmailConfirm') }}
				</v-btn>
			</v-card>
		</v-dialog>

		<!-- Dialog Show example Send Email To Requester-->
		<v-dialog v-model="dialogSendEmailToRQ" persistent width="557px" :z-index="2">
			<v-card width="557px" height="625px" style="margin-top: 0px; overflow-x: hidden">
				<v-card-title class="headline" style="padding-left: 20px">
					<v-spacer />
					<v-btn fab dark x-small color="white" @click="close4()">
						<v-icon size="37" color="black" dark> mdi-close-circle-outline </v-icon>
					</v-btn>
				</v-card-title>
				<v-row style="display: flex; justify-content: center; align-items: center">
					<v-card outlined style="margin-bottom: 20px; margin-top: 0px">
						<v-img v-if="cmpShowExEmail" contain max-height="476px" max-width="357px" src="\img\photos\DSAR_imgSuc.png">
							<template #placeholder>
								<v-skeleton-loader height="476px" width="357px" type="card-avatar, article" />
							</template>
						</v-img>
						<v-img v-if="!cmpShowExEmail" contain max-height="476px" max-width="357px" src="\img\photos\DSAR_imgErr.png" />
					</v-card>
				</v-row>
				<!-- <v-row style="display: flex; justify-content: center; align-items: center">
					<div class="d-flex" height="476px" style="width:357px;">
					<v-text-field v-model="dtSendMoreDetail" color="primary" label="รายละเอียดเพิ่มเติม"></v-text-field>
					<v-btn elevation="2" color="primary" :loading="buttonEmailloading" class="font-weight-bold mt-3 ml-1" @click="sendEmailToCustomer()">{{
						$t('DSAR.DialogDSAR.Button.SendEmailConfirm')
					}}</v-btn>
					</div>
				</v-row> -->
				<v-btn
					elevation="2"
					color="primary"
					:loading="buttonEmailloading"
					class="font-weight-bold mt-3"
					style="position: absolute;right: 100px;"
					@click="sendEmailToCustomer()"
				>
					{{ $t('DSAR.DialogDSAR.Button.SendEmailConfirm') }}
				</v-btn>
			</v-card>
		</v-dialog>

		<!-- Dialog Show example Send SMS To Requester-->
		<!-- <v-dialog v-model="dialogSendSMSToRQ" persistent width="557px" :z-index="2">
			<v-card width="557px" height="625px" style="margin-top: 0px; overflow-x: hidden">
				<v-card-title class="headline" style="padding-left: 20px">
					<v-spacer></v-spacer>
					<v-btn fab dark small color="white" @click="close5()">
						<v-icon color="#1c38e4" dark> mdi-close </v-icon>
					</v-btn>
				</v-card-title>
				<v-row style="display: flex; justify-content: center; align-items: center">
					<v-card outlined style="margin-bottom: 20px; margin-top: 0px">
						<v-img v-if="data_status == 'ดำเนินการเสร็จสิ้น'" max-height="476px" max-width="357px" contain src="\img\photos\DSAR_imgSuc.png">
							<template v-slot:placeholder>
								<v-skeleton-loader height="476px" width="357px" type="card-avatar, article"></v-skeleton-loader>
							</template>
						</v-img>
						<v-img v-if="data_status == 'ดำเนินการล้มเหลว'" contain max-height="476px" max-width="357px" src="\img\photos\DSAR_imgErr.png"></v-img>
					</v-card>
				</v-row>
				<v-btn
					elevation="2"
					color="primary"
					:loading="buttonEmailloading"
					style="position: absolute;right: 100px;"
					class="font-weight-bold mt-3 ml-1"
					@click="openCfSendSMS()"
					>{{ $t('DSAR.DialogDSAR.Button.SendSMS') }}</v-btn
				>
			</v-card>
		</v-dialog> -->

		<!-- Dialog Show example Send SMS To Requester (New)-->
		<v-dialog v-model="dialogSendSMSToRQ" width="350">
			<v-card flat>
				<v-card-text style="font-size:14px; font-weight:700;background:#367BF5; color:white; height:38px">
					<p class="pt-2 ml-n2">{{ $t('DSAR.DialogDSAR.DialogConfirmSendSMS.Title') }}</p>
				</v-card-text>
				<v-card-text class="mt-3 " style="font-size:14px;color:#000000; ">
					<p class="ml-n2" style="font-weight:700;">{{ $t('DSAR.DialogDSAR.DialogConfirmSendSMS.SubTitle') }}</p>
					<p class="ml-n2">
						{{ $t('DSAR.DialogDSAR.DialogConfirmSendSMS.Detail1') }} <span style="font-weight:700"> "{{ data_number }}" </span>
						{{ $t('DSAR.DialogDSAR.DialogConfirmSendSMS.Detail2') }}
					</p>
				</v-card-text>
				<v-card-actions style="font-weight:700">
					<v-spacer />
					<v-btn text style="width:100px;color:#000000;font-weight:700" @click="closeDialogSendSMS()">
						{{ $t('DSAR.DialogDSAR.Button.Cancel') }}
					</v-btn>
					<v-btn style="background:#367BF5; color:white; width:100px;font-weight:700" @click="sendSMSToCustomer()" :loading="buttonEmailloading">
						{{ $t('DSAR.DialogDSAR.Button.Confirm') }}
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>

		<!-- Dialog All Consent -->
		<v-dialog v-model="dialogAllConsent" width="1300px" scrollable style="position: absolute; z-index: 20001">
			<v-card class="dialogCard">
				<v-card-title class="indigo darken-4" style="height: 15%">
					<p class="tb-text" style="font-size: 22px; color: #ffffff">
						{{ $t('DSAR.DialogDSAR.ConsentDetail') }}
					</p>
				</v-card-title>
				<v-card-text>
					<list-consent-table ref="consents" :fullname="data_name" :email="data_email" :dsar="true" @invalid="checkInvalidConsent" class="pt-10" />
				</v-card-text>
				<v-card-actions>
					<v-btn
						elevation="2"
						:disabled="disableActivityBtn"
						color="primary"
						style="font-family: 'Sarabun', sans-serif; font-size: 1.6vh; font-weight: bold"
						v-if="$can('update', 'DSARTransaction')"
						@click="
							allAssetCheck()
							consentLoading = true
							allActivity = []
						"
					>
						{{ $t('DSAR.DialogDSAR.Button.AssetActivity') }}
					</v-btn>
					<v-spacer />
					<v-btn
						color="primary"
						text
						@click="
							dialogAllConsent = false
							disableActivityBtn = true
						"
					>
						{{ $t('DSAR.DialogDSAR.Button.Close') }}
					</v-btn>
				</v-card-actions>
			</v-card>
			<v-overlay :value="consentLoading">
				<v-progress-circular indeterminate size="64" />
			</v-overlay>
		</v-dialog>

		<!-- Dialog dialogAllAsset -->
		<v-dialog v-model="dialogAllAsset" scrollable width="80%">
			<v-card class="dialogCard">
				<v-card-title class="indigo darken-4" style="height: 15%">
					<p class="tb-text" style="font-size: 22px; color: #ffffff">
						{{ $t('DSAR.DialogDSAR.Activity') }}
					</p>
				</v-card-title>
				<v-card-text>
					<process-activity
						ref="activity"
						:activityitem="allActivity"
						:previewOnly="true"
						:headersRoP="headersRoP"
						:reload="updateActivityTable"
						:dsar="true"
					/>
				</v-card-text>
				<v-card-actions>
					<v-spacer />
					<v-btn color="primary" text @click="dialogAllAsset = false">
						{{ $t('DSAR.DialogDSAR.Button.Close') }}
					</v-btn>
				</v-card-actions>
			</v-card>
			<v-overlay :value="consentLoading">
				<v-progress-circular indeterminate size="64" />
			</v-overlay>
		</v-dialog>
		<v-snackbar v-model="snackbarSuccess" timeout="3000" color="success" style="z-index: 40001">
			{{ $t('DSAR.DialogDSAR.Alert.ActionSuccess') }}
		</v-snackbar>
		<v-snackbar v-model="snackbarFailed" timeout="3000" color="red" style="z-index: 40001">
			{{ $t('DSAR.DialogDSAR.Alert.ActionFailed') }}
		</v-snackbar>

		<!--Dialog confirm RQNext -->
		<v-dialog v-model="dialogRequestNext" width="350">
			<v-card flat>
				<v-card-text style="font-size:14px; font-weight:700;background:#F3A72E; color:white; height:38px">
					<p class="pt-2 ml-n2">{{ $t('DSAR.DialogDSAR.DialogConfirmEndRequest.Title') }}</p>
				</v-card-text>
				<v-card-text class="mt-3 " style="font-size:14px;color:#000000;font-weight:700; ">
					<p class="ml-n2">{{ $t('DSAR.DialogDSAR.DialogConfirmEndRequest.SubTitle') }}</p>
					<p class="ml-n2">
						{{ $t('DSAR.DialogDSAR.DialogConfirmEndRequest.Detail') }}
					</p>
				</v-card-text>
				<v-card-actions style="font-weight:700">
					<v-spacer />
					<v-btn text style="width:100px;color:#000000;font-weight:700" @click="closeCfRequstNext()">
						{{ $t('DSAR.DialogDSAR.Button.Cancel') }}
					</v-btn>
					<v-btn style="background:#F3A72E; color:white; width:100px;font-weight:700" @click="cfRequstNext()" :loading="buttonEmailloading">
						{{ $t('DSAR.DialogDSAR.Button.Confirm') }}
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>

		<!-- Search Responsible -->
		<dialog-search-reponsible
			v-if="diSearchRSPS.trigger"
			:trigger="diSearchRSPS.trigger"
			:propsResponsible="diSearchRSPS.propsRSPS"
			:numberResponsible="datasRSPS.length"
			:optionUsers="diSearchRSPS.allUserRSPS"
			@diSaveResponsible="mtDiSearchRSPSSaveRSPS($event)"
			@diEditManual="mtDiSearchEditRSPSManual($event)"
			@close="mtDiSearchRSPSClose()"
		/>

		<!-- Full Timeline  progress -->
		<dialog-full-progress
			v-if="dataDialogFullProgress.trigger"
			:trigger="dataDialogFullProgress.trigger"
			:dataHistory="cmpDataHistory"
			@close="mtCloseDialogFullProgress()"
		/>
	</v-main>
</template>

<script>
import axios from 'axios'
import ListConsentTable from '../../components/consent/ListConsentTable.vue'
import ProcessActivity from '../../components/rop/process_activity.vue'
import DialogSearchReponsible from '../../components/DataSubjectRight/DialogSearchReponsible.vue'
import DialogFullProgress from '../../components/DataSubjectRight/DialogFullProgress.vue'
import { v4 as uuidv4 } from 'uuid'

export default {
	components: {
		ListConsentTable,
		ProcessActivity,
		DialogSearchReponsible,
		DialogFullProgress,
	},
	data() {
		return {
			userid: '',
			pageIndex: 0,
			updateConsentTable: 0,
			updateActivityTable: 0,
			databaseGet: [],
			contactGet: [],
			profileGet: [],
			dialog1: false,
			dialog2: false,
			dialog3: false,
			dialogSendEmailToRQ: false,
			dialogSendSMSToRQ: false,
			dialogResponsible: false,
			snackbarSuccess: false,
			snackbarFailed: false,
			dialogAllConsent: false,
			dialogAllAsset: false,
			buttonEmailloading: false,
			i: 0,
			p: true,
			resorce: {},
			search: '',
			loading: true,
			name: '',
			index: 0,
			re: false,
			colorCard: '#e4e7f6',
			cardloading2: false,
			statusFillter: '',
			durationFillter: '',
			costumerFillter: '',
			dataSubjectTypeFillter: '',
			statusChoice: ['ทั้งหมด', 'ดำเนินการเสร็จสิ้น', 'ดำเนินการล้มเหลว', 'คำร้องเข้าสู่ระบบ', 'รอการตรวจสอบ', 'กำลังดำเนินการ', 'ดำเนินการล้มเหลว'],
			durationChoice: ['ทั้งหมด', 'ภายใน 7 วัน', 'ภายใน 14 วัน', 'ภายใน 21 วัน', 'ภายใน 30 วัน', 'เกินกำหนด'],
			costumerChoice: [
				'ทั้งหมด',
				'ถอนการยินยอมการให้ใช้ข้อมูล',
				'ขอเข้าถึงข้อมูล / ขอสำเนาข้อมูลส่วนบุคคลของข้าพเจ้า',
				'แก้ไขข้อมูลส่วนบุคคลของข้าพเจ้า',
				'ระงับ / ลบข้อมูลส่วนบุคคลของข้าพเจ้า',
				'จำกัดการประมวลผลข้อมูลส่วนบุคคล',
				'ขอโอนย้ายข้อมูลส่วนบุคคล',
				'คัดค้านการประมวลผลของข้อมูลส่วนบุคคล',
				'เปิดเผยแหล่งที่มาของข้อมูลส่วนบุคคลของข้าพเจ้า',
				'ถ่ายโอนข้อมูลส่วนบุคคลของข้าพเจ้า ไปยังผู้ควบคุมข้อมูลส่วนบุคคลอื่น',
				'คำขอเกี่ยวกับการตัดสินใจแทนโดยอัตโนมัติ (automated decision making and profiling)',
			],
			dataSubjectTypeChoice: ['ทั้งหมด', 'ลูกค้า', 'พนักงาน', 'พันธมิตรหรือคู่ค้าทางธุรกิจ', 'ผู้ถือหุ้น', 'กรรมการ', 'บุคคลภายนอกอื่น ๆ', 'ไม่ระบุ'],
			tableloading: true,
			cardloading: false,
			holder: {
				name: '',
				tel: '',
				email: '',
				position: '',
			},
			newHistory: {
				date: '',
				details: '',
				event: '',
			},
			data_name: '',
			data_email: '',
			data_number: '',
			data_working: '',
			data_type: '',
			data_objective: '',
			data_startdate: '',
			data_recentdate: '',
			data_lastdate: '',
			data_history: [],
			data_status: '',
			data_id: '',
			data_category: '',
			nameRules: [(v) => !!v || ''],
			emailRules: [(v) => !!v || '', (v) => /.+@.+\..+/.test(v) || ''],
			telRules: [(v) => !!v || '', (v) => /[0-9]/.test(v)],
			// Integrate Consent Management Center
			confirmSuspend: false,
			fullnameConsent: '',
			allConsentDetail: [],
			activity_rebase: [],
			consentLoading: false,
			// Integrate RoP
			allActivity: [],
			disableActivityBtn: true,
			headersRoP: [
				{
					text: 'ชื่อกิจกรรม',
					align: 'start',
					value: 'activity_name',
					width: '15%',
				},
				{ text: 'แผนกที่เกี่ยวข้อง', value: 'responsible_person', width: '10%' },

				{
					text: 'Asset ที่เกี่ยวข้อง',
					value: 'data_lineage',
					width: '15%',
					sortable: false,
				},
				{
					text: 'ตัวเลือก',
					value: 'actions',
					width: '16%',
					sortable: false,
					align: 'center',
				},
			],
			btnAdvance: false,
			/* for dialogSearchResponsible */
			diSearchRSPS: {
				trigger: false,
				propsRSPS: [],
				allUserRSPS: [],
			},
			datasRSPS: [],
			/* for dataDialogFullProgress */
			dataDialogFullProgress: {
				trigger: false,
			},
			selectAccessLevel: [
				{ name: 'Owner', val: 'Owner' },
				{ name: 'Editor', val: 'Editor' },
				{ name: 'Viewer', val: 'Viewer' },
			],
			disableAcceptRqBtn: false,
			stpProcess: '',
			stpNext: '',
			colorAvatar: ['secondary', 'dark', 'primary', 'success', 'danger', 'warning', 'info'],
			successSendEmail: false,
			userSystem: {},
			dialogRequestNext: false,
			dtDiCfSendSMS: false,
			endRequest: false,
			dataHistoryAll: []
		}
	},
	props: {
		onlyRequestNotReceived: Boolean,
	},
	created() {
		this.mtQueryData()
	},
	methods: {
		async mtQueryData() {
			let URL = ''
			URL = `${this.$appurl}/DataSubjectRequest`
			let DSARItem = []
			await axios
				.get(URL)
				.then(async (res) => {
					this.databaseGet = res.data.Items.map((item) => {
						const id = { CustomerName: item.CustomerName }
						const timeStamp = { timeStamp: item.timeStamp }
						DSARItem = { ...id, ...timeStamp, ...item.DataSubjectRequest[0] }
						return DSARItem
					})
					this.tableloading = false
				})
				.catch((error) => console.log(error))

			URL = `${this.$appurl}/Contact`
			await axios
				.get(URL)
				.then(async (res) => {
					this.contactGet = res.data.Items[0].Contact
				})
				.catch((error) => console.log(error))

			URL = `${this.$appurl}/Profile`
			await axios
				.get(URL)
				.then(async (res) => {
					this.profileGet = res.data.Items[0].CompanyProfile
				})
				.catch((error) => console.log(error))

			URL = `${this.$appurl}/Documents/UserID`
			await axios
				.get(URL)
				.then(async (res) => {
					this.userid = res.data.Items[0].id
				})
				.catch((error) => console.log(error))
			this.mtQueryUsersRSPS()
			this.mtQueryUserSystem()
		},
		set(i, j) {
			this.i = i
			this.name = j
		},
		async refresh() {
			const URL = `${this.$appurl}/DataSubjectRequest`
			let DSARItem = []
			this.tableloading = true
			await axios
				.get(URL)
				.then((res) => {
					this.databaseGet = res.data.Items.map((item) => {
						const id = { CustomerName: item.CustomerName }
						const timeStamp = { timeStamp: item.timeStamp }
						DSARItem = { ...id, ...timeStamp, ...item.DataSubjectRequest[0] }
						return DSARItem
					})
					this.tableloading = false
				})
				.catch((error) => console.log(error))
			this.statusFillter = ''
			this.durationFillter = ''
			this.costumerFillter = ''
			this.dataSubjectTypeFillter = ''
			this.search = ''
			this.re = true
		},
		color(text) {
			let i = 0
			if (text === 'ดำเนินการเสร็จสิ้น') i = 'green'
			if (text === 'ดำเนินการล้มเหลว') i = 'grey'
			if (text === 'รอการตรวจสอบ') i = 'red'
			if (text === 'เกินกำหนดเวลา') i = 'grey'
			if (text === 'กำลังดำเนินการ') i = 'primary'
			if (text === 'คำร้องเข้าสู่ระบบ') i = '#2F4D99'
			if (text === 'เกิดเหตุขัดข้อง')	i = 'warning'
			return i
		},
		status(text, time) {
			let i = 0
			if ((text === 'ดำเนินการล้มเหลว') || (text === 'ดำเนินการเสร็จสิ้น')) i = 'ดำเนินการแล้ว'
			else {
				if (time >= 0) {
					i = 'กำลังดำเนินการ'
				}
				if (time < 0) {
					i = 'เกินกำหนดเวลา'
				}
			}
			return i
		},
		style(text, time) {
			let i = 'margin-top: 15px;'
			let color = 'orange'
			if (time < 0) color = 'red'
			if (text === 'ดำเนินการเสร็จสิ้น') color = 'green'
			if (text === 'ดำเนินการล้มเหลว') color = 'grey'
			i = `${i}color:${color}`
			return i
		},
		selectColor(item) {
			let color
			if (item === this.$t('DSAR.TimelineProcess.Status.InProgress')) color = '#1F6DF9'
			if (item === this.$t('DSAR.TimelineProcess.Status.HasProblem')) color = '#FFB51E'
			if (item === this.$t('DSAR.TimelineProcess.Status.Failed')) color = 'red'
			if (item === this.$t('DSAR.TimelineProcess.Status.Success')) color = '#009834'
			return color
		},
		selectIcon(item) {
			let icon
			if (item === this.$t('DSAR.TimelineProcess.Status.InProgress')) icon = 'mdi-circle-slice-6'
			if (item === this.$t('DSAR.TimelineProcess.Status.HasProblem')) icon = 'mdi-alert-box'
			if (item === this.$t('DSAR.TimelineProcess.Status.Failed')) icon = 'mdi-close-box'
			if (item === this.$t('DSAR.TimelineProcess.Status.Success')) icon = ' mdi-checkbox-marked'
			return icon
		},
		async complete(index) {
			// *Push old data to new obj in DB
			await this.updateOldDataToDB(index)

			// !start prepare for dialog search
			if (this.databaseGet[index].holders) {
				this.datasRSPS = this.databaseGet[index].holders
			} else {
				this.datasRSPS = []
			}
			this.dialog1 = true

			function reverseArr(input) {
				const ret = []
				for (let i = input.length - 1; i >= 0; i--) {
					ret.push(input[i])
				}
				return ret
			}
			// *clear data
			if (!this.$refs.form !== true) this.$refs.form.reset()

			this.holder = {
				name: '',
				tel: '',
				email: '',
				position: '',
			}
			this.i = this.databaseNew.length - index - 1
			const j = this.i
			this.index = index
			this.data_name = this.databaseNew[j].name
			this.data_email = this.databaseNew[j].email
			this.data_number = this.databaseNew[j].number
			this.data_type = this.databaseNew[j].type.toString()
			this.data_objective = this.databaseNew[j].objective
			this.data_startdate = this.databaseNew[this.databaseNew.length - index - 1].history[0].date
			this.data_recentdate = this.databaseNew[this.databaseNew.length - index - 1].history[
				this.databaseNew[this.databaseNew.length - index - 1].history.length - 1
			].date
			this.data_history = reverseArr(this.databaseNew[this.databaseNew.length - index - 1].history)
			this.data_status = this.databaseNew[this.databaseNew.length - index - 1].history[
				this.databaseNew[this.databaseNew.length - index - 1].history.length - 1
			].event
			this.data_lastdate = this.databaseNew[this.databaseNew.length - index - 1].history[
				this.databaseNew[this.databaseNew.length - index - 1].history.length - 1
			].date
			this.data_id = this.databaseNew[j].reqid
			this.data_category = this.databaseNew[j].data_subject_type !== '-' ? this.databaseNew[j].data_subject_type : 'ไม่ระบุ'

			this.filterHistory()
			this.nextStep()
		},
		completeButton(index) {
			const URL = `${this.$appurl}/DataSubjectRequest${index}`
			axios
				.put(URL)
				.then(() => {})
				.catch((error) => console.log(error))
		},
		remove(index) {
			this.$refs['my-modal1'].show()
			this.name = this.databaseNew[this.databaseNew.length - index.num - 1].name
			this.i = index.CustomerName
		},
		deleteButton(index) {
			const URL = `${this.$appurl}/DataSubjectRequest/${index}`
			this.tableloading = true
			axios
				.delete(URL)
				.then(() => {
					this.$refs['my-modal1'].hide()
					this.refresh()
					this.tableloading = false
				})
				.catch((error) => console.log(error))
		},
		cancelButton() {
			this.$refs['my-modal1'].hide()
			this.$refs['my-modal2'].hide()
		},
		openDialogSendEmailToRQ() {
			this.dialogSendEmailToRQ = true
		},
		openDialogSendSMS() {
			this.dialogSendSMSToRQ = true
		},
		closeDialogSendSMS() {
			this.dialogSendSMSToRQ = false
		},
		close() {
			this.dialog1 = false
			this.btnAdvance = false
			this.dialogAllConsent = false
			this.dialogAllAsset = false
			if (!this.$refs.form !== true) this.$refs.form.reset()

			this.refresh()
		},
		close2() {
			this.dialog2 = false
			this.newHistory = { date: '', details: '', event: '' }
		},
		close3() {
			this.dialog3 = false
		},
		close4() {
			this.dialogSendEmailToRQ = false
		},
		close5() {
			this.dialogSendSMSToRQ = false
		},
		rowClass() {
			const rowClass = 'bodyTable'
			return rowClass
		},
		async sendSMSToCustomer() {
			let formData
			let URL
			this.cardloading2 = true
			this.buttonEmailloading = true
			// *reverse history
			function reverseArr(input) {
				const ret = []
				for (let i = input.length - 1; i >= 0; i--) {
					ret.push(input[i])
				}
				return ret
			}
			// *send SMS
			const allEmail = []
			const allSMS = []
			this.datasRSPS.forEach((item) => {
				allEmail.push(item.Email)
				allSMS.push(item.Tel)
			})
			formData = {
				CusTel: this.databaseNew[this.i].number.replace(/-/g, ''),
				ReqType: this.databaseNew[this.i].type.toString(),
				WorkEmail: allEmail.join(', '),
				WorkTel: allSMS.join(', '),
				ComName: this.profileGet.companyName,
			}
			// *Find suc or failed at last
			let sucOrFailed = ''
			const dtHisEvent = reverseArr(this.data_history)
			dtHisEvent.forEach((item) => {
				if ((item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') && item.userSystem === true) {
					sucOrFailed = item.event
				}
			})

			if (sucOrFailed === 'ดำเนินการเสร็จสิ้น') formData.Type = 'Success'
			else if (sucOrFailed === 'ดำเนินการล้มเหลว') formData.Type = 'Error'

			URL = `${this.$appurl}/DataSubjectRequest/sendSMS`
			await axios
				.post(URL, formData)
				.then(() => {})
				.catch((error) => console.log(error))

			// !save History
			let numberS = 1
			this.data_history.forEach((item) => {
				if (item.event === 'ตอบกลับโดย SMS') {
					numberS += 1
				}
			})
			formData = {
				date: new Date().toLocaleString(),
				event: 'ตอบกลับโดย SMS',
				details: `ส่ง SMS ยืนยันคำร้อง ครั้งที่ ${numberS} เสร็จสิ้น`,
				responsible: [],
				userSystem: true,
			}
			URL = `${this.$appurl}/DataSubjectRequest/history/${this.databaseNew[this.i].CustomerName}`
			await axios
				.put(URL, formData)
				.then(async () => {
					// !get DSSR & history
					let DSARItem = []
					URL = `${this.$appurl}/DataSubjectRequest`
					await axios
						.get(URL)
						.then((res) => {
							this.databaseGet = res.data.Items.map((item) => {
								const id = { CustomerName: item.CustomerName }
								const timeStamp = { timeStamp: item.timeStamp }
								DSARItem = { ...id, ...timeStamp, ...item.DataSubjectRequest[0] }
								return DSARItem
							})
							this.data_history = reverseArr(this.databaseNew[this.databaseNew.length - this.index - 1].history)
							this.nextStep()
							this.buttonEmailloading = false
							this.cardloading2 = false
							this.close5()
							this.snackbarSuccess = true
						})
						.catch((error) => console.log(error))
				})
				.catch((error) => console.log(error))
		},
		async sendEmailToCustomer() {
			const evSucFailed = this.data_history.filter((item) => item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว')
			let formData
			let URL
			this.buttonEmailloading = true
			this.cardloading2 = true

			function reverseArr(input) {
				const ret = []
				for (let i = input.length - 1; i >= 0; i--) {
					ret.push(input[i])
				}
				return ret
			}

			const allName = []
			const allEmail = []
			const allTel = []
			this.datasRSPS.forEach((item) => {
				allEmail.push(item.Email)
				allTel.push(item.Tel)
			})

			formData = {
				CusName: this.databaseNew[this.i].name,
				CusEmail: this.databaseNew[this.i].email,
				CusTel: this.databaseNew[this.i].number,
				ReqType: this.databaseNew[this.i].type.toString(),
				ReqDate: this.databaseNew[this.i].date,
				ReqData: evSucFailed[0].details,
				WorkAds: allName.join(', '),
				WorkEmail: allEmail.join(', '),
				WorkTel: allTel.join(', '),
				DPOname: this.profileGet.DPOname,
				DPOemail: this.profileGet.companyEmail,
				DPOtel: this.profileGet.companyTel,
				ComDate: this.databaseNew[this.databaseNew.length - this.index - 1].history[
					this.databaseNew[this.databaseNew.length - this.index - 1].history.length - 1
				].date,
				ComName: this.profileGet.companyName,
				ComHome: this.profileGet.companyPlace,
				Type: '',
			}
			// *Find suc or failed at last
			let sucOrFailed = ''
			const dtHisEvent = reverseArr(this.data_history)
			dtHisEvent.forEach((item) => {
				if ((item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') && item.userSystem === true) {
					sucOrFailed = item.event
				}
			})

			if (sucOrFailed === 'ดำเนินการเสร็จสิ้น') formData.Type = 'Success'
			else if (sucOrFailed === 'ดำเนินการล้มเหลว') formData.Type = 'Error'

			// !send email
			URL = `${this.$appurl}/DataSubjectRequest/sendEmail`
			await axios
				.post(URL, formData)
				.then(() => {})
				.catch((error) => console.log(error))

			// *find number send email
			let numberE = 1
			this.data_history.forEach((item) => {
				if (item.event === 'ตอบกลับโดย EMAIL') {
					numberE += 1
				}
			})
			formData = {
				date: new Date().toLocaleString(),
				event: 'ตอบกลับโดย EMAIL',
				details: `ส่ง EMAIL ยืนยันคำร้อง ครั้งที่ ${numberE} เสร็จสิ้น`,
				responsible: [],
				userSystem: true,
			}
			// !save History
			URL = `${this.$appurl}/DataSubjectRequest/history/${this.databaseNew[this.i].CustomerName}`
			await axios
				.put(URL, formData)
				.then(async () => {
					// !get DSSR & history
					let DSARItem = []
					URL = `${this.$appurl}/DataSubjectRequest`
					await axios
						.get(URL)
						.then((res) => {
							this.databaseGet = res.data.Items.map((item) => {
								const id = { CustomerName: item.CustomerName }
								const timeStamp = { timeStamp: item.timeStamp }
								DSARItem = { ...id, ...timeStamp, ...item.DataSubjectRequest[0] }
								return DSARItem
							})
							this.data_history = reverseArr(this.databaseNew[this.databaseNew.length - this.index - 1].history)
							this.nextStep()
							this.buttonEmailloading = false
							this.cardloading2 = false
							this.close4()
							this.snackbarSuccess = true
						})
						.catch((error) => console.log(error))
				})
				.catch((error) => console.log(error))

			this.buttonEmailloading = false
		},
		mtAcceptRequest() {
			this.dialog3 = true
		},
		async updateHistory() {
			this.buttonEmailloading = true
			let URL

			function reverseArr(input) {
				const ret = []
				for (let i = input.length - 1; i >= 0; i--) {
					ret.push(input[i])
				}
				return ret
			}
			// *Add this.userSystem
			const formData = {
				date: new Date().toLocaleString(),
				event: this.newHistory.event,
				details: this.newHistory.details,
				responsible: [this.userSystem],
				userSystem: true,
			}

			URL = `${this.$appurl}/DataSubjectRequest/history/${this.databaseNew[this.i].CustomerName}`
			this.cardloading2 = true
			try {
				await axios.put(URL, formData)
				let DSARItem = []
				URL = `${this.$appurl}/DataSubjectRequest`
				const res2 = await axios.get(URL)
				this.databaseGet = res2.data.Items.map((item) => {
					const id = { CustomerName: item.CustomerName }
					const timeStamp = { timeStamp: item.timeStamp }
					DSARItem = { ...id, ...timeStamp, ...item.DataSubjectRequest[0] }
					return DSARItem
				})
				this.data_history = reverseArr(this.databaseNew[this.databaseNew.length - this.index - 1].history)
				this.data_status = this.data_history[0].event
				this.filterHistory()
				this.nextStep()
				this.buttonEmailloading = false
				this.snackbarSuccess = true
			} catch (error) {
				this.snackbarFailed = true
				console.log(`Err : ${error}`)
			}

			this.close2()
			this.cardloading2 = false
		},
		allConsentCheck() {
			this.dialogAllConsent = true
			this.updateConsentTable += 1
			this.consentLoading = true
			setTimeout(() => {
				this.$refs.consents.searchConsent()
			}, 500)
		},
		checkInvalidConsent(value) {
			if (value) {
				this.disableActivityBtn = true
			} else {
				this.disableActivityBtn = false
			}
			this.consentLoading = false
		},
		allAssetCheck() {
			setTimeout(() => {
				this.getActivity()
			}, 200)
			this.dialogAllAsset = true
		},
		addTimeline() {
			this.dialog2 = true
		},
		stepColor(text) {
			let color
			if (text === this.$t('DSAR.TimelineProcess.Status.RequestToSystem')) {
				color = '#151D44'
			}
			if (text === this.$t('DSAR.TimelineProcess.Status.InProgress')) {
				color = '#3E66FB'
			}
			if (text === this.$t('DSAR.TimelineProcess.Status.HasProblem')) {
				color = '#FFB51E'
			}
			if (text === this.$t('DSAR.TimelineProcess.Status.WaitCheck')) {
				color = 'red'
			}
			if (text === this.$t('DSAR.TimelineProcess.Status.Failed')) {
				color = 'grey'
			}
			if (text === this.$t('DSAR.TimelineProcess.Status.Success')) {
				color = '#009834'
			}
			if (text === this.$t('DSAR.TimelineProcess.Status.ReplyEmail') || text === this.$t('DSAR.TimelineProcess.Status.ReplySMS')) {
				color = '#1F337D'
			}
			return color
		},
		stepIcon(text) {
			let icon
			if (text === this.$t('DSAR.TimelineProcess.Status.RequestToSystem')) icon = 'mdi-email-outline'
			if (text === this.$t('DSAR.TimelineProcess.Status.InProgress')) icon = 'mdi-circle-slice-6'
			if (text === this.$t('DSAR.TimelineProcess.Status.HasProblem')) icon = 'mdi-alert-box'
			if (text === this.$t('DSAR.TimelineProcess.Status.WaitCheck')) icon = 'mdi-clock'
			if (text === this.$t('DSAR.TimelineProcess.Status.Failed')) icon = 'mdi-close-box'
			if (text === this.$t('DSAR.TimelineProcess.Status.Success')) icon = 'mdi-checkbox-marked'
			if (text === this.$t('DSAR.TimelineProcess.Status.ReplyEmail') || text === this.$t('DSAR.TimelineProcess.Status.ReplySMS')) icon = 'mdi-email-outline'
			return icon
		},
		nextStep() {
			this.stpProcess = '1'
			const { event } = this.databaseNew[this.i].history[this.databaseNew[this.i].history.length - 1]
			if (event === 'คำร้องเข้าสู่ระบบ' && this.databaseNew[this.i].holders === undefined) this.stpProcess = '1'
			else if (event === 'กำลังดำเนินการ' || event === 'เกิดเหตุขัดข้อง') {
				this.stpProcess = '2'
				this.stpNext = '2'
			} else if (event === 'รอการตรวจสอบ') {
				this.stpProcess = '2'
				this.stpNext = '2'
			} else if (event === 'ดำเนินการล้มเหลว' || event === 'ดำเนินการเสร็จสิ้น') {
				const { endRequest } = this.databaseNew[this.i]
				this.data_status = event
				if (endRequest === true) this.stpProcess = '3'
				else {
					this.stpProcess = '2'
					this.stpNext = '3'
				}
			} else if (event === 'ตอบกลับโดย SMS' || event === 'ตอบกลับโดย EMAIL') {
				const ev = this.data_history.find((item) => {
					// *return at first found
					if (item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') {
						return item
					}

					return []
				})
				this.data_status = ev.event
				this.stpProcess = '3'
			}
		},
		getActivity() {
			this.allActivity = []
			this.headersRoP = [
				{
					text: 'ชื่อกิจกรรม',
					align: 'start',
					value: 'activity_name',
					width: '15%',
				},
				{ text: 'แผนกที่เกี่ยวข้อง', value: 'responsible_person', width: '10%' },

				{
					text: 'Asset ที่เกี่ยวข้อง',
					value: 'data_lineage',
					width: '15%',
					sortable: false,
				},
				{
					text: 'ตัวเลือก',
					value: 'actions',
					width: '16%',
					sortable: false,
					align: 'center',
				},
			]
			axios
				.get(`${this.$appurl}/RoP`, {})
				.then((response) => {
					response.data.message.Items.forEach((element) => {
						if (element.RoPs.Activity) {
							element.RoPs.Activity.data_lineage.forEach((items) => {
								this.$refs.consents.transactions.forEach((transaction) => {
									if (element.RoPs.Activity.activity_name === transaction.FormName) {
										if (this.allActivity.indexOf(element.RoPs.Activity) === -1) {
											this.allActivity.push(element.RoPs.Activity)
										}
									}
									if (items === transaction.FormName) {
										if (this.allActivity.indexOf(element.RoPs.Activity) === -1) {
											this.allActivity.push(element.RoPs.Activity)
										}
									}
								})
							})
						}
					})
					if (this.$refs.consents) {
						this.consentLoading = false
					}
				})
				.catch((e) => {
					this.errors.push(e)
				})
		},
		clickBtnAdvance() {
			this.btnAdvance = !this.btnAdvance
			this.$refs.form.reset()
		},
		trigDialogSearchResponsible() {
			this.diSearchRSPS.trigger = true
			this.btnAdvance = false
			this.diSearchRSPS.propsRSPS = JSON.parse(JSON.stringify(this.datasRSPS))
			this.$refs.form.reset()
		},
		mtDiSearchRSPSClose() {
			this.diSearchRSPS.trigger = false
			this.disableAcceptRqBtn = false
			this.diSearchRSPS.propsRSPS = JSON.parse(JSON.stringify(this.datasRSPS))
		},
		mtDiSearchRSPSSaveRSPS(data) {
			this.diSearchRSPS.trigger = false
			this.datasRSPS = data
		},
		mtDiSearchEditRSPSManual(data) {
			this.btnAdvance = true
			this.diSearchRSPS.trigger = false

			this.holder.name = data.Name
			this.holder.tel = data.Tel
			this.holder.email = data.Email
			this.holder.position = data.Position
			const idx = this.datasRSPS.findIndex((item) => item.keyId === data.keyId)
			this.datasRSPS.splice(idx, 1)
		},
		openDialogFullProgress() {
			this.dataDialogFullProgress.trigger = true
		},
		mtCloseDialogFullProgress() {
			this.dataDialogFullProgress.trigger = false
			this.disableAcceptRqBtn = true
		},
		changeStatusBtnAddDLSR() {
			this.diSearchRSPS.trigger = false
			this.disableAcceptRqBtn = true
		},
		mtAddManualRSPS() {
			if (this.$refs.form.validate()) {
				this.$refs.form.resetValidation()
				this.mtCutShotName(this.holder.name)
				this.holder.name = ''
				this.holder.tel = ''
				this.holder.email = ''
				this.holder.position = ''
				this.btnAdvance = false
			}
		},
		mtCutShotName(data) {
			let name = ''
			let shotN = ''
			if (data.includes('-')) {
				name = data.replace(/-/g, ',').split(',')
				if (name[0].match(/[ก-ฮ]/g) && name[1].match(/[ก-ฮ]/g)) {
					shotN = name[0].match(/[ก-ฮ]/g)[0] + name[1].match(/[ก-ฮ]/g)[0]
				} else if (name[0].toUpperCase().match(/[A-Z]/g) && name[1].toUpperCase().match(/[A-Z]/g)) {
					shotN = name[0].toUpperCase().match(/[A-Z]/g)[0] + name[1].toUpperCase().match(/[A-Z]/g)[0]
				}
			} else if (data.includes(' ')) {
				name = data.replace(/ /g, ',').split(',')
				if (name[0].match(/[ก-ฮ]/g) && name[1].match(/[ก-ฮ]/g)) {
					shotN = name[0].match(/[ก-ฮ]/g)[0] + name[1].match(/[ก-ฮ]/g)[0]
				} else if (name[0].toUpperCase().match(/[A-Z]/g) && name[1].toUpperCase().match(/[A-Z]/g)) {
					shotN = name[0].toUpperCase().match(/[A-Z]/g)[0] + name[1].toUpperCase().match(/[A-Z]/g)[0]
				}
			} else {
				if (data.match(/[ก-ฮ]/g)) {
					shotN = data.match(/[ก-ฮ]/g)[0]
				} else if (data.toUpperCase().match(/[A-Z]/g)) {
					shotN = data.toUpperCase().match(/[A-Z]/g)[0]
				}
			}

			const color = this.colorAvatar[Math.floor(Math.random() * this.colorAvatar.length)]
			const obj = {
				keyId: uuidv4(),
				Name: this.holder.name,
				Tel: this.holder.tel,
				Email: this.holder.email,
				Position: this.holder.position,
				shotName: shotN,
				color,
			}
			this.datasRSPS.push(obj)
		},
		async mtSendEmailToTeamAll() {
			this.buttonEmailloading = true
			let cntRes1 = 0
			let formData
			let URL
			// !send mail
			// for (let i in this.datasRSPS) {
			// 	let id = this.databaseNew[this.i].CustomerName
			// 	formData = {
			// 		CusName: this.databaseNew[this.i].name,
			// 		CusEmail: this.databaseNew[this.i].email,
			// 		CusTel: this.databaseNew[this.i].number,
			// 		ReqType: this.databaseNew[this.i].type.toString(),
			// 		ReqDate: this.databaseNew[this.i].date,
			// 		ReqData: this.databaseNew[this.databaseNew.length - this.index - 1].history[
			// 			this.databaseNew[this.databaseNew.length - this.index - 1].history.length - 1
			// 		].details,
			// 		WorkAds: this.datasRSPS[i].Name,
			// 		WorkEmail: this.datasRSPS[i].Email,
			// 		WorkTel: this.datasRSPS[i].Tel,
			// 		DPOname: this.profileGet.DPOname,
			// 		DPOemail: this.profileGet.companyEmail,
			// 		DPOtel: this.profileGet.companyTel,
			// 		ComDate: this.databaseNew[this.databaseNew.length - this.index - 1].history[
			// 			this.databaseNew[this.databaseNew.length - this.index - 1].history.length - 1
			// 		].date,
			// 		ComName: this.profileGet.companyName,
			// 		ComHome: this.profileGet.companyPlace,
			// 		URLwork:
			// 			document.domain + '/datasubjectForm/' + id + '/' + this.databaseNew[this.i].reqid + '/' + this.userid + '/' + this.datasRSPS[i].keyId,
			// 		Password: this.databaseNew[this.i].password,
			// 		Description: this.databaseNew[this.i].objective,
			// 		// WorkDetails: this.dtSendMoreDetail, //!เพิ่งเพิ่ม รายละเอียดเพิ่มเติม
			// 		Type: 'ToTeam',
			// 	}
			// 	URL = this.$appurl + '/DataSubjectRequest/sendEmail'
			// 	await axios
			// 		.post(URL, formData)
			// 		.then((res) => {
			// 			if (res.status === 200 || res.statusText === 'OK') {
			// 				cntRes1 = cntRes1 + 1
			// 			}
			// 		})
			// 		.catch((error) => console.log(error))
			// }

			this.datasRSPS.forEach(async (item) => {
				const id = this.databaseNew[this.i].CustomerName
				formData = {
					CusName: this.databaseNew[this.i].name,
					CusEmail: this.databaseNew[this.i].email,
					CusTel: this.databaseNew[this.i].number,
					ReqType: this.databaseNew[this.i].type.toString(),
					ReqDate: this.databaseNew[this.i].date,
					ReqData: this.databaseNew[this.databaseNew.length - this.index - 1].history[
						this.databaseNew[this.databaseNew.length - this.index - 1].history.length - 1
					].details,
					WorkAds: item.Name,
					WorkEmail: item.Email,
					WorkTel: item.Tel,
					DPOname: this.profileGet.DPOname,
					DPOemail: this.profileGet.companyEmail,
					DPOtel: this.profileGet.companyTel,
					ComDate: this.databaseNew[this.databaseNew.length - this.index - 1].history[
						this.databaseNew[this.databaseNew.length - this.index - 1].history.length - 1
					].date,
					ComName: this.profileGet.companyName,
					ComHome: this.profileGet.companyPlace,
					URLwork: `${document.domain}/datasubjectForm/${id}/${this.databaseNew[this.i].reqid}/${this.userid}/${item.keyId}`,
					Password: this.databaseNew[this.i].password,
					Description: this.databaseNew[this.i].objective,
					Type: 'ToTeam',
				}

				URL = `${this.$appurl}/DataSubjectRequest/sendEmail`
				await axios
					.post(URL, formData)
					.then((res) => {
						if (res.status === 200 || res.statusText === 'OK') {
							cntRes1 += 1
						}
					})
					.catch((error) => console.log(error))
			})

			// !save holders
			URL = `${this.$appurl}/DataSubjectRequest/holders/${this.databaseNew[this.i].CustomerName}`
			await axios
				.put(URL, this.datasRSPS)
				.then(() => {})
				.catch((error) => console.log(error))
			// !replace history[0] 1
			formData = {
				index: 0,
				date: this.data_history[0].date,
				event: this.data_history[0].event,
				details: `${this.data_history[0].event} อยู่ระหว่างรอการยืนยัน`,
				// details2: 'เพิ่มผู้รับผิดชอบเข้าสู่คำร้อง',
				responsible: [],
				userSystem: true,
			}
			URL = `${this.$appurl}/DataSubjectRequest/history/${this.databaseNew[this.i].CustomerName}`
			await axios
				.put(URL, formData)
				.then(() => {})
				.catch((error) => console.log(error))

			// !Add history 2 and all responsible
			function reverseArr(input) {
				const ret = []
				for (let i = input.length - 1; i >= 0; i--) {
					ret.push(input[i])
				}
				return ret
			}

			const allName = []
			this.datasRSPS.forEach((item) => {
				allName.push(item.Name)
			})
			const allNameRSPS = allName.join(', ')
			formData = {
				date: new Date().toLocaleString(),
				event: 'กำลังดำเนินการ',
				details: `คำร้องถูกมอบหมายให้แก่ ${allNameRSPS} รับผิดชอบแล้ว`,
				responsible: this.datasRSPS,
				userSystem: true,
			}
			URL = `${this.$appurl}/DataSubjectRequest/history/${this.databaseNew[this.i].CustomerName}`
			await axios
				.put(URL, formData)
				.then()
				.catch((error) => {
					console.log(error)
				})

			// !fect all DDSR
			URL = `${this.$appurl}/DataSubjectRequest`
			await axios
				.get(URL)
				.then((res) => {
					const { index } = this
					this.databaseGet = res.data.Items.map((item) => {
						const id = { CustomerName: item.CustomerName }
						const DSARItem = { ...id, ...item.DataSubjectRequest[0] }
						return DSARItem
					})

					this.data_name = this.databaseNew[this.databaseNew.length - index - 1].name
					this.data_email = this.databaseNew[this.databaseNew.length - index - 1].email
					this.data_number = this.databaseNew[this.databaseNew.length - index - 1].number
					this.data_type = this.databaseNew[this.databaseNew.length - index - 1].type.toString()
					this.data_startdate = this.databaseNew[this.databaseNew.length - index - 1].history[0].date
					this.data_recentdate = this.databaseNew[this.databaseNew.length - index - 1].history[
						this.databaseNew[this.databaseNew.length - index - 1].history.length - 1
					].date
					this.data_history = reverseArr(this.databaseNew[this.databaseNew.length - index - 1].history)
					this.data_status = this.databaseNew[this.databaseNew.length - index - 1].history[
						this.databaseNew[this.databaseNew.length - index - 1].history.length - 1
					].event
					this.data_lastdate = this.databaseNew[this.databaseNew.length - index - 1].history[
						this.databaseNew[this.databaseNew.length - index - 1].history.length - 1
					].date
					this.datasRSPS = this.databaseGet[index].holders
				})
				.catch((error) => console.log(error))
			// !response
			if (cntRes1 === this.datasRSPS.length) {
				this.buttonEmailloading = false
				this.successSendEmail = true
				this.nextStep()

				setTimeout(() => {
					this.close3()
					this.successSendEmail = false
				}, 1000)
			}
		},

		async mtQueryUsersRSPS() {
			const URL2 = `${this.$appurl}/Contact`
			await axios
				.get(URL2)
				.then((res) => {
					this.diSearchRSPS.allUserRSPS = res.data.Items[0].Contact
				})
				.catch((error) => console.log(error))
		},
		async mtQueryUserSystem() {
			const userSys = sessionStorage.getItem('username')
			let us
			const URL = `${this.$appurl}/member`
			await axios
				.get(URL)
				.then((res) => {
					us = res.data.Users.filter((item) => item.Username === userSys)
				})
				.catch((error) => console.log(error))
			this.userSystem = {
				keyId: uuidv4(),
				Name: `${us[0].Attributes[5].Value} ${us[0].Attributes[0].Value}`,
				Tel: us[0].Attributes[1].Value,
				Email: us[0].Username,
				Postion: us[0].Attributes[4].Value,
				shotName: '',
				userSystem: true,
				color: 'danger',
			}
			const em = us[0].Username.substring(0, 2)
			// !cut shot name
			if (em[0].toUpperCase().match(/[A-Z]/g) && em[1].toUpperCase().match(/[A-Z]/g)) {
				this.userSystem.shotName = em[0].toUpperCase().match(/[A-Z]/g)[0] + em[1].toUpperCase().match(/[A-Z]/g)[0]
			}
		},
		openCfRequstNext() {
			this.dialogRequestNext = true
		},
		closeCfRequstNext() {
			this.dialogRequestNext = false
		},
		async cfRequstNext() {
			let URL
			this.buttonEmailloading = true

			URL = `${this.$appurl}/DataSubjectRequest/endRequest/${this.databaseNew[this.i].CustomerName}`
			const formData = {
				endRequest: true,
			}
			await axios
				.put(URL, formData)
				.then(async () => {
					let DSARItem = []
					URL = `${this.$appurl}/DataSubjectRequest`
					await axios.get(URL).then((res) => {
						this.databaseGet = res.data.Items.map((item) => {
							const id = { CustomerName: item.CustomerName }
							const timeStamp = { timeStamp: item.timeStamp }
							DSARItem = { ...id, ...timeStamp, ...item.DataSubjectRequest[0] }
							return DSARItem
						})

						this.nextStep()
						this.buttonEmailloading = false
						this.closeCfRequstNext()
						this.snackbarSuccess = true
					})
				})
				.catch((error) => console.log(error))
		},
		openCfSendSMS() {
			this.dtDiCfSendSMS = true
		},
		// !filter history not success or failed
		filterHistory() {
			this.dataHistoryAll = JSON.parse(JSON.stringify(this.data_history))
			const hisNew = this.data_history.filter((item) => {
				if ((item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') && item.userSystem === false) return false
				return true
			})
			this.data_history = hisNew
		},
		mtSelectAction(e) {
			if (e === this.$t('DSAR.TimelineProcess.Status.InProgress')) {
				this.newHistory.event = 'กำลังดำเนินการ'
			} else if (e === this.$t('DSAR.TimelineProcess.Status.HasProblem')) {
				this.newHistory.event = 'เกิดเหตุขัดข้อง'
			} else if (e === this.$t('DSAR.TimelineProcess.Status.Failed')) {
				this.newHistory.event = 'ดำเนินการล้มเหลว'
			} else if (e === this.$t('DSAR.TimelineProcess.Status.Success')) {
				this.newHistory.event = 'ดำเนินการเสร็จสิ้น'
			}
		},
		async updateOldDataToDB(index) {
			let updated = false
			let holder
			let arr = []
			let URL
			let formData
			let hasSucFailed = false

			const mtsAddHolder = () => {
				const data = this.databaseGet[index].holder.name
				let name = ''
				let shotN = ''
				if (data.includes('-')) {
					name = data.replace(/-/g, ',').split(',')
					if (name[0].match(/[ก-ฮ]/g) && name[1].match(/[ก-ฮ]/g)) {
						shotN = name[0].match(/[ก-ฮ]/g)[0] + name[1].match(/[ก-ฮ]/g)[0]
					} else if (name[0].toUpperCase().match(/[A-Z]/g) && name[1].toUpperCase().match(/[A-Z]/g)) {
						shotN = name[0].toUpperCase().match(/[A-Z]/g)[0] + name[1].toUpperCase().match(/[A-Z]/g)[0]
					}
				} else if (data.includes(' ')) {
					name = data.replace(/ /g, ',').split(',')
					if (name[0].match(/[ก-ฮ]/g) && name[1].match(/[ก-ฮ]/g)) {
						shotN = name[0].match(/[ก-ฮ]/g)[0] + name[1].match(/[ก-ฮ]/g)[0]
					} else if (name[0].toUpperCase().match(/[A-Z]/g) && name[1].toUpperCase().match(/[A-Z]/g)) {
						shotN = name[0].toUpperCase().match(/[A-Z]/g)[0] + name[1].toUpperCase().match(/[A-Z]/g)[0]
					}
				} else {
					if (data.match(/[ก-ฮ]/g)) {
						shotN = data.match(/[ก-ฮ]/g)[0]
					} else if (data.toUpperCase().match(/[A-Z]/g)) {
						shotN = data.toUpperCase().match(/[A-Z]/g)[0]
					}
				}

				const color = this.colorAvatar[Math.floor(Math.random() * this.colorAvatar.length)]
				holder = {
					keyId: uuidv4(),
					Name: this.databaseGet[index].holder.name,
					Tel: this.databaseGet[index].holder.tel,
					Email: this.databaseGet[index].holder.email,
					Position: this.databaseGet[index].holder.department,
					shotName: shotN,
					color,
				}
			}

			const mtsPrepareHistory = (hld) => {
				const newHis = []
				const newHisSucFailedSys = []
				const newHisWait = []

				this.databaseGet[index].history.forEach((item) => {
					if (item.event !== 'รอการตรวจสอบ' && item.event !== 'ดำเนินการเสร็จสิ้น' && item.event !== 'ดำเนินการล้มเหลว') {
						newHis.push(item)
					}
					if (item.event === 'ดำเนินการเสร็จสิ้น' || item.event === 'ดำเนินการล้มเหลว') {
						let stH = false
						if (!item.password) stH = true
						if (item.userSystem !== undefined) {
							if (item.userSystem) stH = true
							else stH = false
						}
						if (stH) newHisSucFailedSys.push(item)
					}

					if (item.event === 'รอการตรวจสอบ') newHisWait.push(item)
				})

				newHis.forEach((item, idx) => {
					let obj = {}
					if (idx === 0) {
						obj = {
							date: item.date,
							event: item.event,
							details: item.details,
							responsible: [],
							userSystem: true,
						}
					} else if (idx === 1) {
						obj = {
							date: item.date,
							event: item.event,
							details: item.details,
							responsible: [hld],
							userSystem: true,
						}
					} else {
						obj = {
							date: item.date,
							event: item.event,
							details: item.details,
						}
						// *รอบแรกที่อัพเดต อาจจะมี password
						if (item.password === undefined) {
							obj.responsible = []
							obj.userSystem = true
						} else {
							obj.responsible = [hld]
							obj.userSystem = false
						}

						// *รอบถัดไปที่อัพเดต อาจจะมี userSystem
						if (item.userSystem !== undefined) {
							if (item.userSystem) {
								obj.responsible = []
								obj.userSystem = true
							} else {
								obj.responsible = [hld]
								obj.userSystem = false
							}
						}
					}

					arr.push(obj)
				})

				if (newHisWait.length > 0) {
					const idxLast = newHisWait.length - 1
					const waitStatusSuc = !!newHisWait[idxLast].details.includes('ดำเนินการเสร็จสิ้น')
					const str = newHisWait[idxLast].details

					if (waitStatusSuc) {
						const str1 = str.replace('ผู้มอบหมายได้ดำเนินการเสร็จสิ้น ', '').replace(' ขณะนี้อยู่ระหว่างรอผู้ดูแลระบบตรวจสอบ', '')
						const arrH = [
							{
								date: newHisWait[idxLast].date,
								event: 'ดำเนินการเสร็จสิ้น',
								details: str1,
								responsible: [hld],
								userSystem: false,
							},
							{
								date: newHisWait[idxLast].date,
								event: 'รอการตรวจสอบ',
								details: `ผู้มอบหมาย ${hld.Name} ได้ดำเนินการเสร็จสิ้น ${str1} ขณะนี้อยู่ระหว่างรอผู้ดูแลระบบตรวจสอบ`,
								responsible: [hld],
								userSystem: false,
							},
						]
						if (newHisWait[idxLast].userSystem !== undefined) {
							arrH[1].details = str
						}

						arr = [...arr, ...arrH]
					} else {
						let str1 = str.replace('ผู้มอบหมายได้ดำเนินการล้มเหลว ', '').replace(' ขณะนี้อยู่ระหว่างรอผู้ดูแลระบบตรวจสอบ', '')
						if (newHisWait[idxLast].userSystem !== undefined) str1 = str
						const arrH = [
							{
								date: newHisWait[idxLast].date,
								event: 'ดำเนินการล้มเหลว',
								details: str1,
								responsible: [hld],
								userSystem: false,
							},
							{
								date: newHisWait[idxLast].date,
								event: 'รอการตรวจสอบ',
								details: `ผู้มอบหมาย ${hld.Name} ได้ดำเนินการล้มเหลว ${str1} ขณะนี้อยู่ระหว่างรอผู้ดูแลระบบตรวจสอบ`,
								responsible: [hld],
								userSystem: false,
							},
						]

						if (newHisWait[idxLast].userSystem !== undefined) {
							arrH[1].details = str
						}

						arr = [...arr, ...arrH]
					}
				}

				if (newHisSucFailedSys.length > 0) {
					const idxLast = newHisSucFailedSys.length - 1
					arr.push({
						date: newHisSucFailedSys[idxLast].date,
						event: newHisSucFailedSys[idxLast].event,
						details: newHisSucFailedSys[idxLast].details,
						responsible: [],
						userSystem: true,
					})

					hasSucFailed = true
				}
			}

			if (this.databaseGet[index].holders === undefined && this.databaseGet[index].holder.name !== '') {
				mtsAddHolder()
				mtsPrepareHistory(holder)

				// !ADD holders
				URL = `${this.$appurl}/DataSubjectRequest/holders/${this.databaseGet[index].CustomerName}`
				await axios
					.put(URL, [holder])
					.then(() => {})
					.catch((error) => console.log(error))

				// !Replace history
				formData = {
					replace: true,
					history: arr,
				}
				URL = `${this.$appurl}/DataSubjectRequest/history/${this.databaseGet[index].CustomerName}`
				this.cardloading2 = true
				await axios
					.put(URL, formData)
					.then(() => {})
					.catch((error) => console.log(error))

				// !Add endRequest
				if (hasSucFailed) {
					URL = `${this.$appurl}/DataSubjectRequest/endRequest/${this.databaseGet[index].CustomerName}`
					formData = {
						endRequest: true,
					}
					await axios
						.put(URL, formData)
						.then(() => {})
						.catch((error) => console.log(error))
				}
				updated = true
			} else if (this.databaseGet[index].holders !== undefined && this.databaseGet[index].holder.name !== '') {
				if (this.databaseGet[index].holders.length === 1) {
					let hasChange = true
					this.databaseGet[index].history.forEach((item, idx)=>{
						if (item.userSystem === undefined) {
							hasChange = false
						}
					})

					if (!hasChange) {
						const hld = this.databaseGet[index].holders[0]
						mtsPrepareHistory(hld)

						// !Replace history
						formData = {
							replace: true,
							history: arr,
						}
						URL = `${this.$appurl}/DataSubjectRequest/history/${this.databaseGet[index].CustomerName}`
						this.cardloading2 = true
						await axios
							.put(URL, formData)
							.then(() => {})
							.catch((error) => console.log(error))

						// !Add endRequest
						if (hasSucFailed) {
							URL = `${this.$appurl}/DataSubjectRequest/endRequest/${this.databaseGet[index].CustomerName}`
							formData = {
								endRequest: true,
							}
							await axios
								.put(URL, formData)
								.then(() => {})
								.catch((error) => console.log(error))
						}

						updated = true
					}
				}
			}

			if (updated) {
				URL = `${this.$appurl}/DataSubjectRequest`
				let DSARItem = []
				await axios
					.get(URL)
					.then(async (res) => {
						this.databaseGet = res.data.Items.map((item) => {
							const id = { CustomerName: item.CustomerName }
							const timeStamp = { timeStamp: item.timeStamp }
							DSARItem = { ...id, ...timeStamp, ...item.DataSubjectRequest[0] }
							return DSARItem
						})
						this.tableloading = false
					})
					.catch((error) => console.log(error))
			}

			this.cardloading2 = false
		},
	},
	computed: {
		databaseNew() {
			function TimeLimit(time, status) {
				const days = 30 - Math.floor((Date.now() - Date.parse(time)) * 0.000000011574074)
				if (status === 'ดำเนินการเสร็จสิ้น') return 'เสร็จสมบูรณ์'
				if (status === 'ดำเนินการล้มเหลว') return 'ดำเนินการไม่สำเร็จ'
				if (days >= 0) return `เหลือเวลาอีก ${days} วัน`
				return `เกินกำหนด ${days * -1} วัน `
			}
			function TimeLim(time) {
				const days = 30 - Math.floor((Date.now() - Date.parse(time)) * 0.000000011574074)
				return days
			}
			const newObj = this.databaseGet.map((obj, index) => {
				const Obj = {}
				Obj.CustomerName = obj.CustomerName
				Obj.timeStamp = obj.timeStamp
				Obj.date = obj.date
				Obj.name = `${obj.first_name} ${obj.surname}`
				Obj.type = obj.data_subject
				Obj.working = obj.history[obj.history.length - 1].event
				Obj.lim = TimeLim(obj.date)
				Obj.limit = TimeLimit(obj.date, Obj.working)
				Obj.holder = obj.holder
				Obj.objective = obj.objective
				Obj.referrerName = obj.referrerName
				Obj.password = obj.password
				Obj.number = obj.phone_number
				Obj.email = obj.email
				Obj.num = index
				Obj.history = obj.history
				Obj.reqid = obj.id
				Obj.data_subject_type = obj.data_subject_type ? obj.data_subject_type : '-'
				Obj.endRequest = obj.endRequest ? obj.endRequest : false
				return Obj
			})
			return newObj.reverse()
		},
		databaseNewTable() {
			const db = JSON.parse(JSON.stringify(this.databaseNew))
			this.databaseNew.forEach((itm1, idx1) => {
				let ev = ''
				if (itm1.working === 'ตอบกลับโดย EMAIL' || itm1.working === 'ตอบกลับโดย SMS') {
					this.databaseNew[idx1].history.forEach((itm2) => {
						if (itm2.event === 'ดำเนินการเสร็จสิ้น' || itm2.event === 'ดำเนินการล้มเหลว') {
							ev = itm2.event
						}
					})
					db[idx1].working = ev
				}
			})

			return db
		},
		navigationcolor() {
			switch (this.pageIndex) {
			case 0:
				return 'blue-grey'
			case 1:
				return 'teal'
			case 2:
				return 'brown'
			case 3:
				return 'indigo'
			default:
				return 'blue-grey'
			}
		},
		headers() {
			return [
				{
					text: this.$t('DSAR.TableHeader.Date'),
					align: 'start',
					value: 'date',
					width: '10%',
					class: 'headerTable',
				},
				{
					text: this.$t('DSAR.TableHeader.Category'),
					align: 'center',
					value: 'data_subject_type',
					width: '12%',
					class: 'headerTable',
					filter: (value) => {
						if (!this.dataSubjectTypeFillter) return true
						if (this.dataSubjectTypeFillter === 'ไม่ระบุ') {
							return !value || value === ''
						}
						return value === this.dataSubjectTypeFillter
					},
				},
				{
					text: this.$t('DSAR.TableHeader.Fullname'),
					align: 'center',
					value: 'name',
					width: '13%',
					class: 'headerTable',
				},
				{
					text: this.$t('DSAR.TableHeader.DSARType'),
					align: 'center',
					value: 'type',
					width: '15%',
					class: 'headerTable',
					filter: (value) => {
						if (!this.costumerFillter) return true
						return value.includes(this.costumerFillter)
					},
				},
				{
					text: this.$t('DSAR.TableHeader.Status'),
					align: 'center',
					value: 'working',
					width: '10%',
					class: 'headerTable',
					filter: (value) => {
						if ((!this.statusFillter || this.statusFillter === 'ทั้งหมด') && !this.onlyRequestNotReceived) {
							return true
						}
						if (this.onlyRequestNotReceived) {
							return value === 'คำร้องเข้าสู่ระบบ'
						}
						return value == this.statusFillter
					},
				},
				{
					text: this.$t('DSAR.TableHeader.RemainingDay'),
					align: 'center',
					value: 'lim',
					width: '15%',
					class: 'headerTable',
					filter: (value) => {
						if (!this.durationFillter) return true
						if (this.durationFillter === 'ภายใน 7 วัน') {
							return (value <= 7) && (value > 0)
						}

						if (this.durationFillter === 'ภายใน 14 วัน') {
							return (value <= 14) && (value > 7)
						}

						if (this.durationFillter === 'ภายใน 21 วัน') {
							return (value <= 21) && (value > 14)
						}

						if (this.durationFillter === 'ภายใน 30 วัน') {
							return (value <= 30) && (value > 21)
						}

						if (this.durationFillter === 'เกินกำหนด') {
							return value <= 0
						}
					},
				},
				{
					text: this.$t('DSAR.TableHeader.Description'),
					value: 'objective',
					sortable: false,
					class: 'headerTable',
					width: '10%',
				},
				{ text: this.$t('DSAR.TableHeader.Action'), align: 'center', value: 'num', sortable: false, width: '10%', class: 'headerTable' },
			]
		},
		cmpBtnAdvance() {
			let re = false
			if (!this.$can('update', 'DSARTransaction') || this.datasRSPS.length >= 10) {
				re = true
			}
			return re
		},
		cmpDisableAcceptReq() {
			let re = false
			if (this.datasRSPS.length < 1) {
				re = true
			}
			if (this.datasRSPS.length >= 1 && this.btnAdvance) {
				re = true
			}

			if (this.$can('update', 'DSARTransaction')) {
				re = true
			}

			return re
		},
		cmpShowSt2St3() {
			let re = false
			if (this.stpProcess === '2' || this.stpProcess === '3') {
				re = true
			}
			return re
		},
		// !change language i18n
		cmpDataHistory() {
			const newHis = JSON.parse(JSON.stringify(this.data_history))
			const datAll = this.dataHistoryAll.filter((item2) => {
				if ((item2.event === 'ดำเนินการล้มเหลว' || item2.event === 'ดำเนินการเสร็จสิ้น') && item2.userSystem === false) {
					return true
				}
				return false
			})
			let datIdx = 0

			newHis.map((item, idx) => {
				if (item.event === 'คำร้องเข้าสู่ระบบ') {
					item.event = this.$t('DSAR.TimelineProcess.Status.RequestToSystem')
					item.details = `${this.$t('DSAR.TimelineProcess.RequestToSystem.Detial1')} ${this.$t('DSAR.TimelineProcess.RequestToSystem.Detail2')}`
				}
				if (item.event === 'กำลังดำเนินการ') {
					item.event = this.$t('DSAR.TimelineProcess.Status.InProgress')
					if (item.details.includes('คำร้องถูกมอบหมายให้แก่')) {
						const allName = []
						item.responsible.forEach((item2) => {
							allName.push(item2.Name)
						})

						item.details = ` ${this.$t('DSAR.TimelineProcess.InProgress.Detail1')} ${allName.join(', ')} ${this.$t(
							'DSAR.TimelineProcess.InProgress.Detail2'
						)}`
					}
				}
				if (item.event === 'เกิดเหตุขัดข้อง') item.event = this.$t('DSAR.TimelineProcess.Status.HasProblem')
				if (item.event === 'ดำเนินการเสร็จสิ้น') item.event = this.$t('DSAR.TimelineProcess.Status.Success')
				if (item.event === 'ดำเนินการล้มเหลว') item.event = this.$t('DSAR.TimelineProcess.Status.Failed')
				if (item.event === 'รอการตรวจสอบ') {
					item.event = this.$t('DSAR.TimelineProcess.Status.WaitCheck')
					if (item.details.includes('ได้ดำเนินการเสร็จสิ้น')) {
						item.details = `${this.$t('DSAR.TimelineProcess.WaitCheck.Detail1')} ${item.responsible[0].Name} ${datAll[datIdx].details} ${this.$t(
							'DSAR.TimelineProcess.WaitCheck.Detail2Success'
						)} ${this.$t('DSAR.TimelineProcess.WaitCheck.Detail3')}`
					} else if (item.details.includes('ได้ดำเนินการล้มเหลว')) {
						item.details = `${this.$t('DSAR.TimelineProcess.WaitCheck.Detail1')} ${item.responsible[0].Name} ${datAll[datIdx].details} ${this.$t(
							'DSAR.TimelineProcess.WaitCheck.Detail2Failed'
						)} ${this.$t('DSAR.TimelineProcess.WaitCheck.Detail3')}`
					}

					datIdx += 1
				}
				if (item.event === 'ตอบกลับโดย EMAIL') {
					item.event = this.$t('DSAR.TimelineProcess.Status.ReplyEmail')
					let num = item.details.match(/\d+/g)
					item.details = `${this.$t('DSAR.TimelineProcess.Email.Detail1')} ${num[0]} ${this.$t('DSAR.TimelineProcess.Email.Detail2')}`
				}
				if (item.event === 'ตอบกลับโดย SMS') {
					item.event = this.$t('DSAR.TimelineProcess.Status.ReplySMS')
					let num = item.details.match(/\d+/g)
					item.details = `${this.$t('DSAR.TimelineProcess.SMS.Detail1')} ${num[0]} ${this.$t('DSAR.TimelineProcess.SMS.Detail2')}`
				}

				return item
			})

			return newHis
		},
		cmpDataStatus() {
			if (this.data_status === 'คำร้องเข้าสู่ระบบ') return this.$t('DSAR.TimelineProcess.Status.RequestToSystem')
			if (this.data_status === 'กำลังดำเนินการ') return this.$t('DSAR.TimelineProcess.Status.InProgress')
			if (this.data_status === 'เกิดเหตุขัดข้อง') return this.$t('DSAR.TimelineProcess.Status.HasProblem')
			if (this.data_status === 'ดำเนินการเสร็จสิ้น') return this.$t('DSAR.TimelineProcess.Status.Success')
			if (this.data_status === 'ดำเนินการล้มเหลว') return this.$t('DSAR.TimelineProcess.Status.Failed')
			if (this.data_status === 'รอการตรวจสอบ') return this.$t('DSAR.TimelineProcess.Status.WaitCheck')

			return this.data_status
		},
		cmpStatusSelect() {
			// const statusSelect = ['กำลังดำเนินการ', 'เกิดเหตุขัดข้อง', 'ดำเนินการล้มเหลว', 'ดำเนินการเสร็จสิ้น']
			const statusSelect = [
				this.$t('DSAR.TimelineProcess.Status.InProgress'),
				this.$t('DSAR.TimelineProcess.Status.HasProblem'),
				this.$t('DSAR.TimelineProcess.Status.Failed'),
				this.$t('DSAR.TimelineProcess.Status.Success'),
			]

			return statusSelect
		},
		cmpShowExEmail() {
			const db = this.databaseNewTable
			if (this.i) {
				const { working } = db[this.i]
				if (working === 'ดำเนินการเสร็จสิ้น') return true
				return false
			}
			return false
		}
	},
	watch: {
		durationFillter() {
			if (this.re) {
				this.statusFillter = ''
			} else {
				this.statusFillter = 'กำลังดำเนินการ'
			}
			this.re = false
		},
	},
}
</script>

<style>
.bodyTable,
.bodyTable a {
	color: rgb(34, 8, 8) !important;
	background-color: #ffffff !important;
	border-color: #c3d6f0 !important;
}
.bodyTable:active,
.bodyTable.active,
.bodyTable:focus,
.bodyTable.focus,
.bodyTable:hover,
.bodyTable a:hover {
	background-color: #f9f9f9 !important;
	border-color: #ffffff !important;
}

.headerTable,
.headerTable a {
	color: #464d69 !important;
	background-color: #f9f9f9 !important;
	border-color: #c3d6f0 !important;
}

.red_small_dot .v-badge__badge {
	height: 14px;
	min-width: 14px;
}
</style>

<style scoped>
@import 'https://fonts.googleapis.com/css?family=Kanit|Prompt';
@import 'https://fonts.googleapis.com/css?family=Sarabun|Prompt';

body {
	font-family: 'Sarabun', sans-serif;
}
h1 {
	font-family: 'Sarabun', sans-serif;
	font-size: 32px;
}
h4 {
	font-family: 'Sarabun', sans-serif;
	font-size: 16px;
}
p {
	font-family: 'Sarabun', sans-serif;
}
.dialogCard {
	max-height: 80vh !important;
}
div {
	font-family: 'Sarabun', sans-serif;
}
.p-font-size {
	color: #000000;
	font-size: 1.8vh;
}
.p-head-font-size {
	color: #000000;
	font-size: 2.5vh;
	font-weight: bold;
	width: 95%;
}
.btn-sms-email {
	color: white;
	background-color: #2e4cbc !important;
	font-size: 1.6vh;
}
</style>

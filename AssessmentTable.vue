<template>
	<div>
		<table class="table table-hover">
			<thead>
				<tr>
					<th scope="col" style="text-align: center">รายละเอียด</th>
					<th style="text-align: center" scope="col">สถานะ</th>
				</tr>
			</thead>
			<tbody>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">1. บทนำ</p></td>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px; width: 140px"></p></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							1.1 พระราชบัญญัติคุ้มครองข้อมูลส่วนบุคคล (Thailand Personal Data Protection Act)
						</p>
					</td>
					<td></td>
				</tr>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">2. ความถูกต้องตามกฎหมายในการเก็บรวบรวม ใช้ หรือเปิดเผย ข้อมูลส่วนบุคคล</p></td>
					<td></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							2.1 ทางองค์กรมีการจัดทำบันทึกการประมวลผลข้อมูลส่วนบุคคล (Record of Processing Activity) โดยมีการบันทึกประเภทข้อมูลส่วนบุคคลที่จัดเก็บ
							วัตถุประสงค์ของการเก็บรวบรวม ฐานกฎหมายที่อ้างอิง ระยะเวลาในการจัดเก็บ แหล่งที่มา การส่งต่อหรือเปิดเผย
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check1[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check1[0] != null">
							<b-icon v-bind:icon="icon(form.two[0].check1[0])" v-bind:variant="variant(form.two[0].check1[0])"></b-icon>&nbsp;{{
								status(form.two[0].check1[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							2.2 ในกรณีที่ไม่สามารถอ้างอิงฐานกฎหมายใด ๆ ได้บริษัทได้มีการจัดทำคำยินยอม (Consent) โดยเจ้าของข้อมูลมีสิทธิที่จะยินยอมหรือไม่ยินยอม
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check2[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check2[0] != null">
							<b-icon v-bind:icon="icon(form.two[0].check2[0])" v-bind:variant="variant(form.two[0].check2[0])"></b-icon>&nbsp;{{
								status(form.two[0].check2[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							2.3 ทางองค์กรมีระบบจัดเก็บคำยินยอมของเจ้าของข้อมูลส่วนบุคคล (Consent Management)
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check3[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check3[0] != null">
							<b-icon v-bind:icon="icon(form.two[0].check3[0])" v-bind:variant="variant(form.two[0].check3[0])"></b-icon>&nbsp;{{
								status(form.two[0].check3[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							2.4 ทางองค์กรมีการแจ้งให้เจ้าของข้อมูลรับทราบถึงการเก็บรวบรวม ใช้
							หรือเปิดเผยข้อมูลที่เป็นข้อมูลอ่อนไหวอย่างชัดเจนในประกาศความเป็นส่วนตัวหรือคำยินยอม
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check4[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check4[0] != null">
							<b-icon v-bind:icon="icon(form.two[0].check4[0])" v-bind:variant="variant(form.two[0].check4[0])"></b-icon>&nbsp;{{
								status(form.two[0].check4[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							2.5 กรณีที่มีการขอคำยินยอมในการประมวลผลข้อมูลส่วนบุคคล บริษัทแยกการประมวลผลข้อมูลส่วนบุคคลระหว่างผู้ที่ให้คำยินยอม
							และผู้ที่ไม่ยินยอมออกจากกันอย่างชัดเจนตามแต่วัตถุประสงค์ที่เกี่ยวข้องกับคำยินยอม
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check5[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.two[0].check5[0] != null">
							<b-icon v-bind:icon="icon(form.two[0].check5[0])" v-bind:variant="variant(form.two[0].check5[0])"></b-icon>&nbsp;{{
								status(form.two[0].check5[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">3. ความโปร่งใสในการใช้งานข้อมูลส่วนบุคคล</p></td>
					<td></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							3.1 ทางองค์กรมีการประกาศนโยบายความเป็นส่วนตัว (Privacy Notice) โดยมีข้อมูล ประเภทข้อมูลส่วนบุคคลที่จัดเก็บ วัตถุประสงค์การจัดเก็บ
							ระยะเวลาการจัดเก็บ แหล่งที่มา การส่งต่อหรือเปิดเผยข้อมูล สิทธิของเจ้าของข้อมูล ช่องทางการติดต่อ อย่างชัดเจนทั้งสำหรับบุคคลภายนอก
							และบุคคลภายใน
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check1[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check1[0] != null">
							<b-icon v-bind:icon="icon(form.three[0].check1[0])" v-bind:variant="variant(form.three[0].check1[0])"></b-icon>&nbsp;{{
								status(form.three[0].check1[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							3.2 ทางองค์กรมีการแจ้งให้เจ้าของข้อมูลทราบถึง ประเภทข้อมูลส่วนบุคคล แหล่งที่มาของข้อมูล วัตถุประสงค์ในการประมวลผล ระยะเวลาการเก็บรักษา
							การเปิดเผยและส่งต่อ ครบถ้วนและสอดคล้องกับกิจกรรมการประมวลผลภายในบริษัท
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check2[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check2[0] != null">
							<b-icon v-bind:icon="icon(form.three[0].check2[0])" v-bind:variant="variant(form.three[0].check2[0])"></b-icon>&nbsp;{{
								status(form.three[0].check2[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							3.3 ทางองค์กรมีกระบวนการในการปรับปรุงหรือทบทวนนโยบายความเป็นส่วนตัวให้ตรงกับกิจกรรมการประมวผลข้อมูลส่วนบุุคคลของบริษัท อย่างสม่ำเสมอ
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check3[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check3[0] != null">
							<b-icon v-bind:icon="icon(form.three[0].check3[0])" v-bind:variant="variant(form.three[0].check3[0])"></b-icon>&nbsp;{{
								status(form.three[0].check3[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							3.4 ในกรณีที่ต้องมีการจัดทำคำยินยอม (Consent) คำยินยอมดังกล่าวมีความชัดเจน และอยู่ในรูปแบบที่เจ้าของข้อมูลต้องเลือกที่จะยินยอม (Opt-in)
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check4[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.three[0].check4[0] != null">
							<b-icon v-bind:icon="icon(form.three[0].check4[0])" v-bind:variant="variant(form.three[0].check4[0])"></b-icon>&nbsp;{{
								status(form.three[0].check4[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">4. การจัดเก็บข้อมูลเฉพาะที่ใช้งาน และการทำลายข้อมูลที่ไม่มีความจำเป็น</p></td>
					<td></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							4.1 ทางองค์กรมีข้อมูลส่วนบุคคลที่มีการประมวลผลทั้งหมดเป็นข้อมูลที่มีความจำเป็นต่อการใช้งานและมีวัตถุประสงค์รองรับทุกประเภท
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.four[0].check1[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.four[0].check1[0] != null">
							<b-icon v-bind:icon="icon(form.four[0].check1[0])" v-bind:variant="variant(form.four[0].check1[0])"></b-icon>&nbsp;{{
								status(form.four[0].check1[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							4.2
							ทางองค์กรมีกระบวนการในการตรวจสอบว่าข้อมูลส่วนบุคคลที่ถูกจัดเก็บนั้นจะถูกลบและทำลายหลังจากไม่ได้ใช้งานตามระยะเวลาที่ระบุไว้ในนโยบายความเป็นส่วนตัว
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.four[0].check2[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.four[0].check2[0] != null">
							<b-icon v-bind:icon="icon(form.four[0].check2[0])" v-bind:variant="variant(form.four[0].check2[0])"></b-icon>&nbsp;{{
								status(form.four[0].check2[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							4.3 ทางองค์กรมีมาตรการในการทบทวนความจำเป็นของข้อมูลส่วนบุคคลที่จัดเก็บอย่างสม่ำเสมอ
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.four[0].check3[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.four[0].check3[0] != null">
							<b-icon v-bind:icon="icon(form.four[0].check3[0])" v-bind:variant="variant(form.four[0].check3[0])"></b-icon>&nbsp;{{
								status(form.four[0].check3[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">5. มาตรฐานความปลอดภัยและการสร้างความตระหนักรู้</p></td>
					<td></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							5.1 ทางองค์กรมีการประกาศนโยบายคุ้มครองข้อมูลส่วนบุคคลและบังคับใช้ในบริษัท
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check1[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check1[0] != null">
							<b-icon v-bind:icon="icon(form.five[0].check1[0])" v-bind:variant="variant(form.five[0].check1[0])"></b-icon>&nbsp;{{
								status(form.five[0].check1[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							5.2 ทางองค์กรมีการควบคุมการเข้าถึงข้อมูลส่วนบุคคลเฉพาะผู้เกี่ยวข้องทั้งในรูปแบบ Physical และ Digital
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check2[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check2[0] != null">
							<b-icon v-bind:icon="icon(form.five[0].check2[0])" v-bind:variant="variant(form.five[0].check2[0])"></b-icon>&nbsp;{{
								status(form.five[0].check2[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							5.3 ทางองค์กรมีกระบวนการในการตรวจสอบการเข้าถึงย้อนหลัง (Audit Log) ของระบบที่มีการจัดเก็บข้อมูลส่วนบุคคล
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check3[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check3[0] != null">
							<b-icon v-bind:icon="icon(form.five[0].check3[0])" v-bind:variant="variant(form.five[0].check3[0])"></b-icon>&nbsp;{{
								status(form.five[0].check3[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							5.4 ทางองค์กรมีการสร้างความตระหนักรู้หรือให้การอบรมแก่พนักงานที่เกี่ยวข้องกับการประมวลผลข้อมูลส่วนบุคคล
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check4[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.five[0].check4[0] != null">
							<b-icon v-bind:icon="icon(form.five[0].check4[0])" v-bind:variant="variant(form.five[0].check4[0])"></b-icon>&nbsp;{{
								status(form.five[0].check4[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">6. สิทธิ์ของเจ้าของข้อมูล</p></td>
					<td></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							6.1 ทางองค์กรมีการจัดทำช่องทางการใช้สิทธิ์ของเจ้าของข้อมูลหรือไม่ และมีการเก็บบันทึกการใช้สิทธิ์ของเจ้าของข้อมูล
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check1[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check1[0] != null">
							<b-icon v-bind:icon="icon(form.six[0].check1[0])" v-bind:variant="variant(form.six[0].check1[0])"></b-icon>&nbsp;{{
								status(form.six[0].check1[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							6.2 ทางองค์กรมีการประกาศช่องทางการใช้สิทธิ์ของเจ้าของข้อมูลส่วนบุคคลอย่างชัดเจนในนโยบายความเป็นส่วนตัวที่ประกาศ
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check2[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check2[0] != null">
							<b-icon v-bind:icon="icon(form.six[0].check2[0])" v-bind:variant="variant(form.six[0].check2[0])"></b-icon>&nbsp;{{
								status(form.six[0].check2[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							6.3 ช่องทางการใช้สิทธิของเจ้าของข้อมูลส่วนบุคคลขององค์กรนั้นมีความเหมาะสมในการเข้าถึงเทียบเท่ากับการขอคำยินยอม
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check3[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check3[0] != null">
							<b-icon v-bind:icon="icon(form.six[0].check3[0])" v-bind:variant="variant(form.six[0].check3[0])"></b-icon>&nbsp;{{
								status(form.six[0].check3[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							6.4 ทางองค์กรมีช่องทางให้เจ้าของข้อมูลสามารถตรวจสอบและแก้ไขความถูกต้องของข้อมูลส่วนบุคคล
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check4[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check4[0] != null">
							<b-icon v-bind:icon="icon(form.six[0].check4[0])" v-bind:variant="variant(form.six[0].check4[0])"></b-icon>&nbsp;{{
								status(form.six[0].check4[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							6.5 มีช่องทางให้เจ้าของข้อมูลสามารถลบข้อมูลส่วนบุคคลของตนเอง
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check5[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check5[0] != null">
							<b-icon v-bind:icon="icon(form.six[0].check5[0])" v-bind:variant="variant(form.six[0].check5[0])"></b-icon>&nbsp;{{
								status(form.six[0].check5[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							6.6 ทางองค์กรมีช่องทางให้เจ้าของข้อมูลสามารถจำกัดการใช้งานการประมวลผลข้อมูลส่วนบุคคลของตนเอง
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check6[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check6[0] != null">
							<b-icon v-bind:icon="icon(form.six[0].check6[0])" v-bind:variant="variant(form.six[0].check6[0])"></b-icon>&nbsp;{{
								status(form.six[0].check6[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							6.7 องค์กรมีช่องทางให้เจ้าของข้อมูลส่วนบุคคลยกเลิกหรือถอนคำยินยอมที่เคยให้ไว้ได้
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check7[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.six[0].check7[0] != null">
							<b-icon v-bind:icon="icon(form.six[0].check7[0])" v-bind:variant="variant(form.six[0].check7[0])"></b-icon>&nbsp;{{
								status(form.six[0].check7[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">7. ความรับผิดชอบและการเก็บบันทึก</p></td>
					<td></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							7.1 คุณสามารถแสดงให้เห็นถึงการปฏิบัติตามข้อกำหนดที่เกี่ยวข้องทั้งหมดที่เกี่ยวข้องกับการประมวลผลข้อมูลส่วนบุคคลหรือไม่?
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.seven[0].check1[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.seven[0].check1[0] != null">
							<b-icon v-bind:icon="icon(form.seven[0].check1[0])" v-bind:variant="variant(form.seven[0].check1[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check1[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							7.2 คุณเก็บบันทึกโดยละเอียดเกี่ยวกับกิจกรรมการประมวลผลข้อมูลและกระแสข้อมูลขององค์กรของคุณหรือไม่
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.seven[0].check2[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.seven[0].check2[0] != null">
							<b-icon v-bind:icon="icon(form.seven[0].check2[0])" v-bind:variant="variant(form.seven[0].check2[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check2[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							7.3 คุณมีขั้นตอนและคู่มือการใช้งาน (เอกสารที่มีรายละเอียดวิธีดำเนินนโยบายภายในของคุณอย่างชัดเจน)
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.seven[0].check3[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.seven[0].check3[0] != null">
							<b-icon v-bind:icon="icon(form.seven[0].check3[0])" v-bind:variant="variant(form.seven[0].check3[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check3[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td><p style="margin-bottom: 0px; padding-bottom: 0px">8. สิทธิ์ของเจ้าของข้อมูล</p></td>
					<td></td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							8.1 องค์กรมีการจัดทำช่องทางการใช้สิทธิ์ของเจ้าของหรือไม่ และมีการเก็บบันทึกการใช้สิทธิ์ของเจ้าของข้อมูล
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.eight[0].check1[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.eight[0].check1[0] != null">
							<b-icon v-bind:icon="icon(form.eight[0].check1[0])" v-bind:variant="variant(form.eight[0].check1[0])"></b-icon>&nbsp;{{
								status(form.eight[0].check1[0])
							}}
						</p>
					</td>
				</tr>
				<tr>
					<td>
						<p style="margin-left: 50px; text-indent: -25px; margin-bottom: 0px; padding-bottom: 0px">
							8.2 มีการประกาศช่องทางการใช้สิทธิ์ของเจ้าของข้อมูลส่วนบุคคลอย่างชัดเจนในประกาศความเป็นส่วนตัว (Privacy Notice)
							หรือนโยบายคุ้มครองข้อมูลส่วนบุคคล (Privacy Policy)
						</p>
					</td>
					<td style="text-align: center">
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.eight[0].check2[0] == null">ยังไม่ได้ระบุ</p>
						<p style="margin-bottom: 0px; padding-bottom: 0px" v-if="form.eight[0].check2[0] != null">
							<b-icon v-bind:icon="icon(form.eight[0].check2[0])" v-bind:variant="variant(form.eight[0].check2[0])"></b-icon>&nbsp;{{
								status(form.eight[0].check2[0])
							}}
						</p>
					</td>
				</tr>
			</tbody>
		</table>
		<b-form>
			<base-block ref="p1" title="1. บทนำ" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col xl="12" lg="12" md="12">
						<h6>1.1 พระราชบัญญัติคุ้มครองข้อมูลส่วนบุคคล (Thailand Personal Data Protection Act)</h6>
						<p>
							&emsp;เนื่องจากปัจจุบันมีการล่วงละเมิด สิทธิความเป็นส่วนตัวของข้อมูลส่วนบุคคลเป็นจำนวนมากจนสร้างความเดือดร้อนรำคาญหรือความเสียหาย
							ให้แก่เจ้าของข้อมูลส่วนบุคคล ประกอบกับความก้าวหน้าของเทคโนโลยีทำให้การเก็บรวบรวม ใช้ หรือ
							เปิดเผยข้อมูลส่วนบุคคลอันเป็นการล่วงละเมิดดังกล่าว ทำได้โดยง่าย สะดวก และรวดเร็ว ก่อให้เกิด ความเสียหายต่อเศรษฐกิจโดยรวม
							สมควรกำหนดให้มีกฎหมายว่าด้วยการคุ้มครองข้อมูลส่วนบุคคล เป็นการทั่วไปขึ้น เพื่อกำหนดหลักเกณฑ์ กลไก
							หรือมาตรการกำกับดูแลเกี่ยวกับการให้ความคุ้มครองข้อมูล ส่วนบุคคลที่เป็นหลักการทั่วไป จึงจำเป็นต้องตราพระราชบัญญัตินี้
							พระราชบัญญัติที่ได้รับอนุมัติได้รับการแก้ไขเพื่อให้สอดคล้องกับแนวคิดและภาระหน้าที่ที่ได้รับการยอมรับทั่วโลก พระราชบัญญัติมีหน้าที่:
						</p>
						<ul class="example">
							<li>ปกป้องข้อมูลส่วนบุคคลโดย จำกัด การเก็บรวบรวม ,ใช้ ,เปิดเผยและถ่ายโอนโดยไม่ได้รับความยินยอมจากเจ้าของข้อมูล</li>
							<li>กำหนดโทษทางอาญา ,โทษทางปกครอง และความรับผิดทางแพ่ง สำหรับการฝ่าฝืนหน้าที่ภายใต้พระราชบัญญัติ</li>
							<li>จัดตั้งคณะกรรมการคุ้มครองข้อมูลส่วนบุคคลเพื่อรับผิดชอบในการปฏิบัติตามพระราชบัญญัติ</li>
						</ul>
						<p>
							&emsp;พระราชบัญญัตินี้จะใช้กับผู้ดูแลข้อมูลส่วนบุคคลซึ่งรวมถึงหน่วยงานภาครัฐและเอกชน
							นอกจากนี้ขอบเขตของกฎหมายยังสามารถเข้าถึงบุคคลภายนอกได้หากผู้ควบคุมหรือผู้ประมวลผล (ไม่ว่าจะอยู่ในประเทศไทยหรือไม่ก็ตาม)
							รวบรวมใช้หรือเปิดเผยข้อมูลส่วนบุคคลที่รวบรวมจากบุคคลในประเทศไทย (ไม่ว่าบุคคลเหล่านั้นจะมีสัญชาติไทยหรือไม่ก็ตาม)
							พระราชบัญญัตินี้จะกำหนดให้ผู้ควบคุมข้อมูลนอกประเทศไทยแต่งตั้งตัวแทนภายในเขตอำนาจ
						</p>
					</b-col>
				</b-row>
			</base-block>
			<base-block ref="p2" title="2. ความถูกต้องตามกฎหมายในการเก็บรวบรวม ใช้ หรือเปิดเผย ข้อมูลส่วนบุคคล" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>
							2.1 ทางองค์กรมีการจัดทำบันทึกการประมวลผลข้อมูลส่วนบุคคล (Record of Processing Activity) โดยมีการบันทึกประเภทข้อมูลส่วนบุคคลที่จัดเก็บ
							วัตถุประสงค์ของการเก็บรวบรวม ฐานกฎหมายที่อ้างอิง ระยะเวลาในการจัดเก็บ แหล่งที่มา การส่งต่อหรือเปิดเผย หรือไม่
						</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group
							id="radio-group-1"
							v-model="form.two[0].check1[0]"
							:options="optionsRadios"
							name="radio-options-1"
							disabled
						></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.two[0].text1"
							disabled
							id="example-textarea-input-alt-1"
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.two[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.two[0].check1[0])" v-bind:variant="variant(form.two[0].check1[0])"></b-icon>&nbsp;{{
								status(form.two[0].check1[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.two[0].check1[0] != 'Yes' && form.two[0].check1[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.two[0].check1[0] != 'Yes' && form.two[0].check1[0] != null">
							บันกิจกรรมการประมวลผลข้อมูลส่วนบุคคลทั้งหมดขององค์กรลงในระบบ t-reg โดยกรอกข้อมูลให้ครบถ้วนตามที่ระบบต้องการ
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>
							2.2 ในกรณีที่ไม่สามารถอ้างอิงฐานกฎหมายใด ๆ ได้บริษัทได้มีการจัดทำคำยินยอม (Consent) โดยเจ้าของข้อมูลมีสิทธิที่จะยินยอมหรือไม่ยินยอม
							หรือไม่
						</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group
							id="radio-group-2"
							v-model="form.two[0].check2[0]"
							:options="optionsRadios"
							name="radio-options-2"
							disabled
						></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.two[0].text2"
							disabled
							id="example-textarea-input-alt-2"
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.two[0].check2[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.two[0].check2[0])" v-bind:variant="variant(form.two[0].check2[0])"></b-icon>&nbsp;{{
								status(form.two[0].check2[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.two[0].check2[0] != 'Yes' && form.two[0].check2[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.two[0].check2[0] != 'Yes' && form.two[0].check2[0] != null">
							จัดทำช่องทางการให้คำยินยอมโดยเจ้าของข้อมูลต้องมีสิทธิในการเลือกว่าจะให้คำยินยอมหรือไม่
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>2.3 ทางองค์กรมีระบบจัดเก็บคำยินยอมของเจ้าของข้อมูลส่วนบุคคล (Consent Management) หรือไม่</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group id="radio-group-3" v-model="form.two[0].check3[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.two[0].text3"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.two[0].check3[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.two[0].check3[0])" v-bind:variant="variant(form.two[0].check3[0])"></b-icon>&nbsp;{{
								status(form.two[0].check3[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.two[0].check3[0] != 'Yes' && form.two[0].check3[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.two[0].check3[0] != 'Yes' && form.two[0].check3[0] != null">
							จัดเก็บคำยินยอมที่ได้รับตามระยะเวลาที่มีการประมวลผลข้อมูลส่วนบุคคลดังกล่าว โดยอาจจัดเก็บในรูปแบบเอกสารหรือดิจิทัล
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>
							2.4 ทางองค์กรมีการแจ้งให้เจ้าของข้อมูลรับทราบถึงการเก็บรวบรวม ใช้
							หรือเปิดเผยข้อมูลที่เป็นข้อมูลอ่อนไหวอย่างชัดเจนในประกาศความเป็นส่วนตัวหรือคำยินยอมหรือไม่
						</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.two[0].check4[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.two[0].text4"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.two[0].check4[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.two[0].check4[0])" v-bind:variant="variant(form.two[0].check4[0])"></b-icon>&nbsp;{{
								status(form.two[0].check4[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.two[0].check4[0] != 'Yes' && form.two[0].check4[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.two[0].check4[0] != 'Yes' && form.two[0].check4[0] != null">
							กรณีที่มีการประมวลผลข้อมูล่อ่อนไหว ให้มีหัวข้อที่แสดงให้เห็นอย่างชัดเจนถึงการประมวลผลข้อมูลอ่อนไหว และวัตถุประสงค์ในการประมวลผล
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>
							2.5 กรณีที่มีการขอคำยินยอมในการประมวลผลข้อมูลส่วนบุคคล บริษัทแยกการประมวลผลข้อมูลส่วนบุคคลระหว่างผู้ที่ให้คำยินยอม
							และผู้ที่ไม่ยินยอมออกจากกันอย่างชัดเจนตามแต่วัตถุประสงค์ที่เกี่ยวข้องกับคำยินยอมหรือไม่
						</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.two[0].check5[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.two[0].text5"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.two[0].check5[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.two[0].check5[0])" v-bind:variant="variant(form.two[0].check5[0])"></b-icon>&nbsp;{{
								status(form.two[0].check5[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.two[0].check5[0] != 'Yes' && form.two[0].check5[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.two[0].check5[0] != 'Yes' && form.two[0].check5[0] != null">
							กิจกรรมที่จำเป็นต้องได้รับการยินยอมในการประมวลผลสามารถดำเนินการได้เฉพาะกับเจ้าของข้อมูลที่ให้คำยินยอมเท่านั้น โดยอาจแยกฐานข้อมูล
							หรือมีข้อมูลบางอย่างที่สามารถแบ่งแยกระหว่างเจ้าของข้อมูลที่ให้คำยินยอม หรือปฏิเสธการยินยอมได้
						</p>
					</b-col>
				</b-row>
			</base-block>
			<br />
			<br />
			<div style="break-after: page"></div>
			<base-block ref="p3" title="3. ความโปร่งใสในการใช้งานข้อมูลส่วนบุคคล" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>
							3.1 ทางองค์กรมีการประกาศนโยบายความเป็นส่วนตัว (Privacy Notice) โดยมีข้อมูล ประเภทข้อมูลส่วนบุคคลที่จัดเก็บ วัตถุประสงค์การจัดเก็บ
							ระยะเวลาการจัดเก็บ แหล่งที่มา การส่งต่อหรือเปิดเผยข้อมูล สิทธิของเจ้าของข้อมูล ช่องทางการติดต่อ อย่างชัดเจนทั้งสำหรับบุคคลภายนอก
							และบุคคลภายในหรือไม่
						</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>คุณมีการประกาศความเป็นส่วนตัว (Privacy Notice) ที่เปิดเผยต่อสาธารณะบนเว็บไซต์ของคุณหรือไม่?</li>
              <li>คุณมีการประกาศความเป็นส่วนตัว (Privacy Notice) สำหรับพนักงานในขององค์กรหรือไม่</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.three[0].check1[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.three[0].text1"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.three[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.three[0].check1[0])" v-bind:variant="variant(form.three[0].check1[0])"></b-icon>&nbsp;{{
								status(form.three[0].check1[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.three[0].check1[0] != 'Yes' && form.three[0].check1[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.three[0].check1[0] != 'Yes' && form.three[0].check1[0] != null">
							จัดทำนโยบายความเป็นส่วนตัวและประกาศไปยังช่องทางที่เจ้าของข้อมูลสามารถเข้าถึงได้ เช่น เว็บไซต์
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<h6>
							3.2 ทางองค์กรมีการแจ้งให้เจ้าของข้อมูลทราบถึง ประเภทข้อมูลส่วนบุคคล แหล่งที่มาของข้อมูล วัตถุประสงค์ในการประมวลผล ระยะเวลาการเก็บรักษา
							การเปิดเผยและส่งต่อ ครบถ้วนและสอดคล้องกับกิจกรรมการประมวลผลภายในบริษัทหรือไม่
						</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>คุณมีการประกาศความเป็นส่วนตัว (Privacy Notice) ที่เปิดเผยต่อสาธารณะบนเว็บไซต์ของคุณหรือไม่?</li>
              <li>คุณมีการประกาศความเป็นส่วนตัว (Privacy Notice) สำหรับพนักงานในขององค์กรหรือไม่</li>
            </ul> -->
						<!-- <p>Do you have a public-facing privacy notice on your website?</p>
            <p>Do you have a privacy notice for employees on your corporate intranet?</p> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.three[0].check2[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.three[0].text2"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.three[0].check2[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.three[0].check2[0])" v-bind:variant="variant(form.three[0].check2[0])"></b-icon>&nbsp;{{
								status(form.three[0].check2[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.three[0].check2[0] != 'Yes' && form.three[0].check2[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.three[0].check2[0] != 'Yes' && form.three[0].check2[0] != null">
							ตรวจสอบนโยบายความเป็นส่วนตัวว่ามีวัตถุประสงค์และประเภทข้อมูลที่จัดเก็บครบถ้วนตามกิจกรรมที่ถูกบันทึกอยู่ในบันทึกการประมวลผลข้อมูลส่วนบุคคลหรือไม่
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>
							3.3 ทางองค์กรมีกระบวนการในการปรับปรุงหรือทบทวนนโยบายความเป็นส่วนตัวให้ตรงกับกิจกรรมการประมวผลข้อมูลส่วนบุุคคลของบริษัท
							อย่างสม่ำเสมอหรือไม่
						</h6>
						<p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
						<ul class="example">
							<li>มีการกำหนดระยะเวลาในปรับปรุง หรือทบทวนนโยบายความเป็นส่วนตัวอย่างชัดเจนทุก 6 เดือน</li>
						</ul>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.three[0].check3[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.three[0].text3"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.three[0].check3[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.three[0].check3[0])" v-bind:variant="variant(form.three[0].check3[0])"></b-icon>&nbsp;{{
								status(form.three[0].check3[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.three[0].check3[0] != 'Yes' && form.three[0].check3[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.three[0].check3[0] != 'Yes' && form.three[0].check3[0] != null">
							ทบทวนนโยบายความเป็นส่วนตัวอย่างสม่ำเสมอและปรับเปลี่ยนหากมีกิจกรรมการประมวลผลที่ถูกเพิ่มและทำให้นโยบายความเป็นส่วนตัวไม่ครอบคลุม
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<h6>
							3.4 ในกรณีที่ต้องมีการจัดทำคำยินยอม (Consent) คำยินยอมดังกล่าวมีความชัดเจน และอยู่ในรูปแบบที่เจ้าของข้อมูลต้องเลือกที่จะยินยอม (Opt-in)
							ใช่หรือไม่
						</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>คุณมีการประกาศความเป็นส่วนตัว (Privacy Notice) ที่เปิดเผยต่อสาธารณะบนเว็บไซต์ของคุณหรือไม่?</li>
              <li>คุณมีการประกาศความเป็นส่วนตัว (Privacy Notice) สำหรับพนักงานในขององค์กรหรือไม่</li>
            </ul> -->
						<!-- <p>Do you have a public-facing privacy notice on your website?</p>
            <p>Do you have a privacy notice for employees on your corporate intranet?</p> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.three[0].check4[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea v-model="form.three[0].text4" class="form-control-alt" rows="7" placeholder="ระบุรายละเอียดที่นี่.."></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.three[0].check4[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.three[0].check4[0])" disabled v-bind:variant="variant(form.three[0].check4[0])"></b-icon>&nbsp;{{
								status(form.three[0].check4[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.three[0].check4[0] != 'Yes' && form.three[0].check4[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.three[0].check4[0] != 'Yes' && form.three[0].check4[0] != null">
							จัดทำคำยินยอมให้จัดแจ้งและมีลักษณะเป็นการเลือกที่จะยินยอม (Opt-in)
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block ref="p4" title="4. การจัดเก็บข้อมูลเฉพาะที่ใช้งาน และการทำลายข้อมูลที่ไม่มีความจำเป็น" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>
							4.1 ทางองค์กรมีข้อมูลส่วนบุคคลที่มีการประมวลผลทั้งหมดเป็นข้อมูลที่มีความจำเป็นต่อการใช้งาน
							โดยมีวัตถุประสงค์ของการประมวลผลข้อมูลส่วนบุคคลที่ชัดเจนรองรับทุกประเภทใช่หรือไม่
						</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีการแจ้งวัตถุประสงค์ในการจัดเก็บข้อมูลก่อนที่จะมีการเก็บรวบรวมข้อมูลส่วนบุคคลหรือไม่</li>
              <li>ไม่มีการจัดเก็บข้อมูลส่วนบุคคลแบบเป็นกรณีไป (Case by Case)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.four[0].check1[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.four[0].text1"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.four[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.four[0].check1[0])" v-bind:variant="variant(form.four[0].check1[0])"></b-icon>&nbsp;{{
								status(form.four[0].check1[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.four[0].check1[0] != 'Yes' && form.four[0].check1[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.four[0].check1[0] != 'Yes' && form.four[0].check1[0] != null">
							ตรวจสอบการใช้งานข้อมูลส่วนบุคคลทั้งหมดในองค์กรและมั่นใจว่าข้อมูลทั้งหมดอยู่ในบันทึกการประมวลผลข้อมูลหรือ Record of Processing
							พร้อมทั้งสร้างความตระหนักรู้ให้แก่พนักงานที่เกี่ยวข้องกับการจัดเก็บหรือใช้ข้อมูลส่วนบุคคลถึงหน้าที่และความรับผิดชอบต่อข้อมูลส่วนบุคคล
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<h6>
							4.2
							ทางองค์กรมีกระบวนการในการตรวจสอบหรือไม่ว่าข้อมูลส่วนบุคคลที่ถูกจัดเก็บนั้นจะถูกลบและทำลายหลังจากไม่ได้ใช้งานตามระยะเวลาที่ระบุไว้ในนโยบายความเป็นส่วนตัว
						</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีการแจ้งวัตถุประสงค์ในการจัดเก็บข้อมูลก่อนที่จะมีการเก็บรวบรวมข้อมูลส่วนบุคคลหรือไม่</li>
              <li>ไม่มีการจัดเก็บข้อมูลส่วนบุคคลแบบเป็นกรณีไป (Case by Case)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.four[0].check2[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.four[0].text2"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.four[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.four[0].check2[0])" v-bind:variant="variant(form.four[0].check2[0])"></b-icon>&nbsp;{{
								status(form.four[0].check2[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.four[0].check2[0] != 'Yes' && form.four[0].check2[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.four[0].check2[0] != 'Yes' && form.four[0].check2[0] != null">
							ตรวจสอบการใช้งานข้อมูลส่วนบุคคลทั้งหมดในองค์กรและมั่นใจว่าข้อมูลทั้งหมดอยู่ในบันทึกการประมวลผลข้อมูลหรือ Record of Processing
							พร้อมทั้งสร้างความตระหนักรู้ให้แก่พนักงานที่เกี่ยวข้องกับการจัดเก็บหรือใช้ข้อมูลส่วนบุคคลถึงหน้าที่และความรับผิดชอบต่อข้อมูลส่วนบุคคล
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>4.3 ทางองค์กรมีมาตรการในการทบทวนความจำเป็นของข้อมูลส่วนบุคคลที่จัดเก็บอย่างสม่ำเสมอหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีการแจ้งวัตถุประสงค์ในการจัดเก็บข้อมูลก่อนที่จะมีการเก็บรวบรวมข้อมูลส่วนบุคคลหรือไม่</li>
              <li>ไม่มีการจัดเก็บข้อมูลส่วนบุคคลแบบเป็นกรณีไป (Case by Case)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.four[0].check3[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.four[0].text3"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.four[0].check3[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.four[0].check3[0])" v-bind:variant="variant(form.four[0].check3[0])"></b-icon>&nbsp;{{
								status(form.four[0].check3[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.four[0].check3[0] != 'Yes' && form.four[0].check3[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.four[0].check3[0] != 'Yes' && form.four[0].check3[0] != null">
							ตรวจสอบการใช้งานข้อมูลส่วนบุคคลทั้งหมดในองค์กรและมั่นใจว่าข้อมูลทั้งหมดอยู่ในบันทึกการประมวลผลข้อมูลหรือ Record of Processing
							พร้อมทั้งสร้างความตระหนักรู้ให้แก่พนักงานที่เกี่ยวข้องกับการจัดเก็บหรือใช้ข้อมูลส่วนบุคคลถึงหน้าที่และความรับผิดชอบต่อข้อมูลส่วนบุคคล
						</p>
					</b-col>
				</b-row>
			</base-block>
			<base-block ref="p5" title="5. มาตรฐานความปลอดภัยและการสร้างความตระหนักรู้" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>5.1 ทางองค์กรมีการประกาศนโยบายคุ้มครองข้อมูลส่วนบุคคลและบังคับใช้ในบริษัทหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="exampler">
              <li>มีการตรวจสอบว่าข้อมูลที่จัดเก็บมีความเพียงพอต่อการบรรลุวัตถุประสงค์หรือไม่</li>
              <li>ไม่มีการจัดเก็บข้อมูลเกินความจำเป็นที่จะบรรลุวัตถุประสงค์ใช่หรือไม่</li>
              <li>การส่งต่อข้อมูลไปยังหน่วยงานภายนอกเป็นการส่งเฉพาะข้อมูลที่จำเป็นเพื่อบรรลุวัตถุประสงค์ใช่หรือไม่</li>
              <li>มีการติดตั้งระบบลบและทำลายข้อมูลโดยอัตโนมัติเมื่อบรรลุวัตถุประสงค์หรือครบกำหนดระยะใช่หรือไม่</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.five[0].check1[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea v-model="form.five[0].text1" class="form-control-alt" rows="7" placeholder="ระบุรายละเอียดที่นี่.."></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.five[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.five[0].check1[0])" disabled v-bind:variant="variant(form.five[0].check1[0])"></b-icon>&nbsp;{{
								status(form.five[0].check1[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.five[0].check1[0] != 'Yes' && form.five[0].check1[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.five[0].check1[0] != 'Yes' && form.five[0].check1[0] != null">
							ออกนโยบายคุ้มครองข้อมูลส่วนบุคคลและสื่อสารไปทั่วทั้งองค์กรให้พนักงานรับทราบ
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>5.2 ทางองค์กรมีการควบคุมการเข้าถึงข้อมูลส่วนบุคคลเฉพาะผู้เกี่ยวข้องทั้งในรูปแบบ Physical และ Digital ใช่หรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="exampler">
              <li>มีการตรวจสอบว่าข้อมูลที่จัดเก็บมีความเพียงพอต่อการบรรลุวัตถุประสงค์หรือไม่</li>
              <li>ไม่มีการจัดเก็บข้อมูลเกินความจำเป็นที่จะบรรลุวัตถุประสงค์ใช่หรือไม่</li>
              <li>การส่งต่อข้อมูลไปยังหน่วยงานภายนอกเป็นการส่งเฉพาะข้อมูลที่จำเป็นเพื่อบรรลุวัตถุประสงค์ใช่หรือไม่</li>
              <li>มีการติดตั้งระบบลบและทำลายข้อมูลโดยอัตโนมัติเมื่อบรรลุวัตถุประสงค์หรือครบกำหนดระยะใช่หรือไม่</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.five[0].check2[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.five[0].text2"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.five[0].check2[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.five[0].check2[0])" v-bind:variant="variant(form.five[0].check2[0])"></b-icon>&nbsp;{{
								status(form.five[0].check2[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.five[0].check2[0] != 'Yes' && form.five[0].check2[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.five[0].check2[0] != 'Yes' && form.five[0].check2[0] != null">
							สินทรัพย์ต่าง ๆ ที่มีข้อมูลส่วนบุคคลมีการควบคุมการเข้าถึงที่เพียงพอและเข้าถึงได้เฉพาะผู้เกี่ยวข้องเท่านั้น เช่น
							ไม่วางเอกสารที่มีข้อมูลส่วนบุคคลไว้บนโต๊ะ หรือเปิดระบบให้เข้าถึงได้แบบสาธารณะ
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<h6>5.3 ทางองค์กรมีกระบวนการในการตรวจสอบการเข้าถึงย้อนหลัง (Audit Log) ของระบบที่มีการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</h6>
						<p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
						<!-- <ul class="exampler">
              <li>มีการตรวจสอบว่าข้อมูลที่จัดเก็บมีความเพียงพอต่อการบรรลุวัตถุประสงค์หรือไม่</li>
              <li>ไม่มีการจัดเก็บข้อมูลเกินความจำเป็นที่จะบรรลุวัตถุประสงค์ใช่หรือไม่</li>
              <li>การส่งต่อข้อมูลไปยังหน่วยงานภายนอกเป็นการส่งเฉพาะข้อมูลที่จำเป็นเพื่อบรรลุวัตถุประสงค์ใช่หรือไม่</li>
              <li>มีการติดตั้งระบบลบและทำลายข้อมูลโดยอัตโนมัติเมื่อบรรลุวัตถุประสงค์หรือครบกำหนดระยะใช่หรือไม่</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.five[0].check3[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.five[0].text3"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.five[0].check3[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.five[0].check3[0])" v-bind:variant="variant(form.five[0].check3[0])"></b-icon>&nbsp;{{
								status(form.five[0].check3[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.five[0].check3[0] != 'Yes' && form.five[0].check3[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.five[0].check3[0] != 'Yes' && form.five[0].check3[0] != null">
							จัดให้มีระบบการจัดเก็บ Log ที่บันทึกการสร้าง แก้ไข ลบข้อมูลส่วนบุคคลที่เป็นระบบสารสนเทศ
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>5.4 ทางองค์กรมีการสร้างความตระหนักรู้หรือให้การอบรมแก่พนักงานที่เกี่ยวข้องกับการประมวลผลข้อมูลส่วนบุคคลหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการลบและทำลายข้อมูลพร้อมการตรวจสอบกระบวนการหรือไม่</li>
              <li>มีการตั้งเวลาในการลบและทำลายข้อมูลหรือไม่</li>
              <li>มีการลบข้อมูลหลังจากที่เจ้าของข้อมูลมีการลบบัญชีผู้ใช้หรือไม่</li>
              <li>วิธีการลบและทำลายข้อมูลมีความปลอดภัยหรือไม่</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.five[0].check4[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.five[0].text4"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.five[0].check4[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.five[0].check4[0])" v-bind:variant="variant(form.five[0].check4[0])"></b-icon>&nbsp;{{
								status(form.five[0].check4[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.five[0].check4[0] != 'Yes' && form.five[0].check4[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.five[0].check4[0] != 'Yes' && form.five[0].check4[0] != null">
							จัดอบรมหรือมีสื่อในการประชาสัมพันธ์เรื่องข้อมูลส่วนบุคคลและการดูแลรักษาข้อมูลให้เป็นไปตามที่กฎหมายกำหนด
						</p>
					</b-col>
				</b-row>
			</base-block>
			<br />
			<br />
			<base-block ref="p6" title="6. สิทธิ์ของเจ้าของข้อมูล" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h6>6.1 ทางองค์กรมีการจัดทำช่องทางการใช้สิทธิ์ของเจ้าของข้อมูลหรือไม่ และมีการเก็บบันทึกการใช้สิทธิ์ของเจ้าของข้อมูลหรือไม่</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.six[0].check1[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.six[0].text1"
							disabled
							id="example-textarea-input-alt-9"
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.six[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.six[0].check1[0])" v-bind:variant="variant(form.six[0].check1[0])"></b-icon>&nbsp;{{
								status(form.six[0].check1[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.six[0].check1[0] != 'Yes' && form.six[0].check1[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.six[0].check1[0] != 'Yes' && form.six[0].check1[0] != null">จัดทำช่องทางให้เจ้าของข้อมูลสามารถติดต่อมาขอใช้สิทธิได้</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>6.2 ทางองค์กรมีการประกาศช่องทางการใช้สิทธิ์ของเจ้าของข้อมูลส่วนบุคคลอย่างชัดเจนในนโยบายความเป็นส่วนตัวที่ประกาศหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.six[0].check2[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.six[0].text2"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.six[0].check2[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.six[0].check2[0])" v-bind:variant="variant(form.six[0].check2[0])"></b-icon>&nbsp;{{
								status(form.six[0].check2[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.six[0].check2[0] != 'Yes' && form.six[0].check2[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.six[0].check2[0] != 'Yes' && form.six[0].check2[0] != null">
							ระบุช่องทางการใช้สิทธิที่ชัดเจนลงในนโยบายความเป็นส่วนตัวที่ประกาศ
						</p>
					</b-col>
				</b-row>
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>6.3 ช่องทางการใช้สิทธิของเจ้าของข้อมูลส่วนบุคคลขององค์กรนั้นมีความเหมาะสมในการเข้าถึงเทียบเท่ากับการขอคำยินยอมหรือไม่</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.six[0].check3[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.six[0].text3"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.six[0].check3[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.six[0].check3[0])" v-bind:variant="variant(form.six[0].check3[0])"></b-icon>&nbsp;{{
								status(form.six[0].check3[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.six[0].check3[0] != 'Yes' && form.six[0].check3[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.six[0].check3[0] != 'Yes' && form.six[0].check3[0] != null">
							ช่องทางในการใช้สิทธิต้องมีความสะดวกในการใช้งานระดับเดียวกับการขอคำยินยอม
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>6.4 ทางองค์กรมีช่องทางให้เจ้าของข้อมูลสามารถตรวจสอบและแก้ไขความถูกต้องของข้อมูลส่วนบุคคลหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.six[0].check4[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.six[0].text4"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.six[0].check4[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.six[0].check4[0])" v-bind:variant="variant(form.six[0].check4[0])"></b-icon>&nbsp;{{
								status(form.six[0].check4[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.six[0].check4[0] != 'Yes' && form.six[0].check4[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.six[0].check4[0] != 'Yes' && form.six[0].check4[0] != null">
							จัดทำช่องทางให้เจ้าของข้อมูลสามารถตรวจสอบและแก้ไขข้อมูลได้ เช่น Customer Portal
							หรือจัดหาผู้ดูแลที่สามารถให้เจ้าของข้อมูลติดต่อเพื่อขอให้ตรวจสอบหรือแก้ไขข้อมูลส่วนบุคคลของตนได้
						</p>
					</b-col>
				</b-row>
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>6.5 มีช่องทางให้เจ้าของข้อมูลสามารถลบข้อมูลส่วนบุคคลของตนเองได้หรือไม่</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.six[0].check5[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.six[0].text5"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.six[0].check5[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.six[0].check5[0])" v-bind:variant="variant(form.six[0].check5[0])"></b-icon>&nbsp;{{
								status(form.six[0].check5[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.six[0].check5[0] != 'Yes' && form.six[0].check5[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.six[0].check5[0] != 'Yes' && form.six[0].check5[0] != null">
							จัดทำช่องทางให้เจ้าของข้อมูลสามารถลบข้อมูลส่วนบุคคลของตนออกจากระบบได้ในกรณีที่ไม่มีความจำเป็นในการใช้งาน เช่น มีระบบ Delete Account
							หรือจัดหาผู้ดูแลที่สามารถให้เจ้าของข้อมูลติดต่อเพื่อขอให้ลบข้อมูลของตนได้
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>6.6 ทางองค์กรมีช่องทางให้เจ้าของข้อมูลสามารถจำกัดการใช้งานการประมวลผลข้อมูลส่วนบุคคลของตนเองหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.six[0].check6[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.six[0].text6"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.six[0].check6[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.six[0].check6[0])" v-bind:variant="variant(form.six[0].check6[0])"></b-icon>&nbsp;{{
								status(form.six[0].check6[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.six[0].check6[0] != 'Yes' && form.six[0].check6[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.six[0].check6[0] != 'Yes' && form.six[0].check6[0] != null">
							จัดทำช่องทางให้เจ้าของข้อมูลสามารถจำกัดการใช้งานการประมวลผลข้อมูลส่วนบุคคลของตนได้ เช่น มีระบบ Customer Portal
							ที่สามารถเลือกปิด-เปิดการประมวลผลข้อมูลส่วนบุคคล หรือจัดหาผู้ดูแลที่สามารถให้เจ้าของข้อมูลติดต่อเพื่อขอให้ลบข้อมูลของตนได้
						</p>
					</b-col>
				</b-row>
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>6.7 องค์กรมีช่องทางให้เจ้าของข้อมูลส่วนบุคคลยกเลิกหรือถอนคำยินยอมที่เคยให้ไว้ได้หรือไม่</h6>
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.six[0].check7[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.six[0].text7"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.six[0].check7[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.six[0].check7[0])" v-bind:variant="variant(form.six[0].check7[0])"></b-icon>&nbsp;{{
								status(form.six[0].check7[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p v-if="form.six[0].check7[0] != 'Yes' && form.six[0].check7[0] != null" style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0">
							คำแนะนำ
						</p>
						<p v-if="form.six[0].check7[0] != 'Yes' && form.six[0].check7[0] != null">
							จัดทำช่องทางให้เจ้าของข้อมูลสามารถยกเลิกการประมวลผลข้อมูลหรือถอนคำยินยอมในการประมวลผลข้อมูลส่วนบุคคลของตนได้ เช่น ระบบ Customer Portal
							ที่สามารถเลือกปิด-เปิดการประมวลผลข้อมูลส่วนบุคคล หรือจัดหาผู้ดูแลที่สามารถให้เจ้าของข้อมูลติดต่อเพื่อขอให้ลบข้อมูลของตนได้
						</p>
					</b-col>
				</b-row>
			</base-block>
			<br />
			<br />
			<div style="break-after: page"></div>
			<base-block ref="p7" title="7. การบริหารจัดการผู้ประมวลผลข้อมูลส่วนบุคคล" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>7.1 ทางองค์กรมีการจัดทำข้อตกลงการประมวลผลข้อมูลส่วนบุคคลสำหรับกิจกรรมที่มีการส่งต่อหรือเปิดเผยข้อมูลส่วนบุคคลครบถ้วนหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.seven[0].check1[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.seven[0].text1"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.seven[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.seven[0].check1[0])" v-bind:variant="variant(form.seven[0].check1[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check1[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.seven[0].check1[0] != 'Yes' && form.seven[0].check1[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.seven[0].check1[0] != 'Yes' && form.seven[0].check1[0] != null">
							ตรวจสอบกิจกรรมการประมวลผลข้อมูลส่วนบุคคลทั้งหมดที่มีการส่งต่อหรือเปิดเผยข้อมูลส่วนบุคคลไปยังหน่วยงานภายนอก
							และจัดทำข้อตกลงการประมวลผลข้อมูลส่วนบุคคลทั้งหมด
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>7.2 ทางองค์กรมีการบันทึกรายชื่อของผู้ประมวลผลข้อมูลส่วนบุคคลลงในบันทึกการประมวลผลข้อมูลส่วนบุคคล (RoP) หรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.seven[0].check2[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.seven[0].text1"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.seven[0].check2[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.seven[0].check2[0])" v-bind:variant="variant(form.seven[0].check2[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check2[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.seven[0].check2[0] != 'Yes' && form.seven[0].check2[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.seven[0].check2[0] != 'Yes' && form.seven[0].check2[0] != null">
							บันทึกรายชื่อของหน่วยงานภายนอกที่ได้รับข้อมูลส่วนบุคคลจากบริษัทลงในบันทึกข้อตกลงการประมวลผลข้อมูลส่วนบุคคล (RoP)
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>
							7.3 ทางองค์กรมีการบันทึกรายชื่อของผู้ประมวลผลข้อมูลส่วนบุคคลที่อยู่ต่างประเทศ
							และประเทศปลายทางลงในบันทึกการประมวลผลข้อมูลส่วนบุคคลหรือไม่
						</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.seven[0].check3[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.seven[0].text3"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.seven[0].check3[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.seven[0].check3[0])" v-bind:variant="variant(form.seven[0].check3[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check3[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.seven[0].check3[0] != 'Yes' && form.seven[0].check3[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.seven[0].check3[0] != 'Yes' && form.seven[0].check3[0] != null">
							บันทึกข้อมูลเกี่ยวกับประเทศปลายทางหรือองค์กรระหว่างประเทศที่ได้รับข้อมูลส่วนบุคคลจากบริษัทลงในบันทึกข้อตกลงการประมวลผลข้อมูลส่วนบุคคล
							(RoP)
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>7.4 ทางองค์กรมีการบันทึกรายชื่อของผู้ประมวลผลข้อมูลส่วนบุคคลลงในบันทึกการประมวลผลข้อมูลส่วนบุคคล (RoP) หรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.seven[0].check4[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.seven[0].text4"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.seven[0].check4[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.seven[0].check4[0])" v-bind:variant="variant(form.seven[0].check4[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check4[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.seven[0].check4[0] != 'Yes' && form.seven[0].check4[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.seven[0].check4[0] != 'Yes' && form.seven[0].check4[0] != null">
							กำหนดมาตรฐานในการรักษาความปลอดภัยลงในข้อตกลงการประมวลผลข้อมูลส่วนบุคคล
							หรือขอให้ผู้ประมวลผลข้อมูลส่วนบุคคลชี้แจงเกี่ยวกับนโยบายคุ้มครองข้อมูลส่วนบุคคลของตน
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h6>7.5 กรณีที่มีผู้ประมวลผลข้อมูลในต่างประเทศ ประเทศหรือหน่วยงานปลายทางดังกล่าวมีมาตรฐานในการคุ้มครองข้อมูลส่วนบุคคลหรือไม่</h6>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีกระบวนการในการป้องกันการลบหรือทำลายข้อมูลโดยความไม่ตั้งใจหรือไม่</li>
              <li>กระบวนการในการบริหารจัดการสินทรัพย์ที่เกี่ยวข้องกับการจัดเก็บข้อมูลส่วนบุคคลหรือไม่</li>
              <li>มีการเข้ารหัสระบบจัดเก็บข้อมูลส่วนบุคคลหรือไม่ (Secure Data In Rest)</li>
              <li>มีกระบวนการในการรักษาความปลอดภัยการถ่ายโอนข้อมูลหรือไม่ (Secure Data Transfer)</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.seven[0].check5[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.seven[0].text5"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.seven[0].check5[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.seven[0].check5[0])" v-bind:variant="variant(form.seven[0].check5[0])"></b-icon>&nbsp;{{
								status(form.seven[0].check5[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.seven[0].check5[0] != 'Yes' && form.seven[0].check5[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.seven[0].check5[0] != 'Yes' && form.seven[0].check5[0] != null">
							ตรวจสอบให้แน่ใจว่าหน่วยงานระหว่างประเทศหรือประเทศปลายทางมีกฎหมายหรือมาตรฐานในการคุ้มครองข้อมูลส่วนบุคคล
						</p>
					</b-col>
				</b-row>
			</base-block>
			<base-block ref="p8" title="8. กระบวนการรับมือเมื่อเกิดเหตุละเมิดข้อมูลส่วนบุคคล" content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12">
						<h5>8.1 ทางองค์กรมีกระบวนการในการรับมือเมื่อเกิดเหตุละเมิดข้อมูลส่วนบุคคลหรือไม่</h5>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีการแสดงวิธีการใช้สิทธิ์ให้แก่เจ้าของข้อมูลส่วนบุคคลอย่างชัดเจนและถูกต้องหรือไม่</li>
              <li>มีการสร้าง Web Portal ให้แก่เจ้าของข้อมูลส่วนบุคคลในการใช้สิทธิ์หรือไม่</li>
              <li>มีนโยบายภายใน, กระบวนการ และการอบรมวิธีการตอบสนองคำร้องของเจ้าของข้อมูลส่วนบุคคลหรือไม่</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.eight[0].check1[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.eight[0].text1"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.eight[0].check1[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.eight[0].check1[0])" v-bind:variant="variant(form.eight[0].check1[0])"></b-icon>&nbsp;{{
								status(form.eight[0].check1[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.eight[0].check1[0] != 'Yes' && form.eight[0].check1[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.eight[0].check1[0] != 'Yes' && form.eight[0].check1[0] != null">
							ออกแบบกระบวนการในการรับมือเมื่อเกิดเหตุละเมิดข้อมูลส่วนบุคคล เช่น การรับแจ้ง การเรียกประชุม การตรวจสอบแหล่งของการละเมิด
							และการรวบรวมข้อมูลที่ต้องแจ้งแก่หน่วยงานกำกับดูแล เป็นต้น
						</p>
					</b-col>
				</b-row>
			</base-block>
			<div style="break-after: page"></div>
			<base-block content-full style="margin-left: 25px">
				<b-row class="push">
					<b-col lg="12" xl="12" md="12" class="mt-5">
						<h5>8.2 ทางองค์กรมีแบบฟอร์มเตรียมการสำหรับแจ้งเหตุเมื่อเกิดเหตุละเมิดข้อมูลส่วนบุคคลหรือไม่</h5>
						<!-- <p class="example">ตัวอย่างที่ต้องดำเนินการ:</p>
            <ul class="example">
              <li>มีการประกาศนโยบายความเป็นส่วนตัว (Privacy Notice) ไว้ในพื้นที่สาธารณะที่สามารถเข้าถึงได้ เช่น เว็บไซต์ขององค์กร หรือไม่</li>
              <li>มีช่องทางการติดต่อเจ้าหน้าที่คุ้มครองข้อมูลส่วนบุคคล (DPO) หรือ Customer Support เปิดเผยอย่างชัดเจนหรือไม่</li>
              <li>มีกระบวนการในการปรับปรุงนโยบายความเป็นส่วนตัวเมื่อมีการเพิ่มวัตถุประสงค์หรือประเภทข้อมูลส่วนบุคคลที่เก็บหรือไม่</li>
            </ul> -->
					</b-col>
					<b-col lg="9" xl="9" md="9">
						<b-form-radio-group v-model="form.eight[0].check2[0]" :options="optionsRadios" disabled></b-form-radio-group>
						<p class="mt-2">โปรดระบุรายละเอียดเพิ่มเติมด้านล่าง</p>
						<b-form-textarea
							v-model="form.eight[0].text2"
							disabled
							class="form-control-alt"
							rows="7"
							placeholder="ระบุรายละเอียดที่นี่.."
						></b-form-textarea>
					</b-col>
					<b-col lg="1" xl="1" md="1"></b-col>
					<b-col lg="2" xl="2" md="2">
						<p style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0" v-if="form.eight[0].check2[0] != null">สถานะ</p>
						<p>
							<b-icon v-bind:icon="icon(form.eight[0].check2[0])" v-bind:variant="variant(form.eight[0].check2[0])"></b-icon>&nbsp;{{
								status(form.eight[0].check2[0])
							}}
						</p>
					</b-col>
					<b-col lg="12" xl="12" md="12">
						<p
							v-if="form.eight[0].check2[0] != 'Yes' && form.eight[0].check2[0] != null"
							style="font-size: 115%; margin-bottom: 10px; padding-bottom: 0"
						>
							คำแนะนำ
						</p>
						<p v-if="form.eight[0].check2[0] != 'Yes' && form.eight[0].check2[0] != null">
							ออกแบบจดหมายหรือหนังสือแจ้งเหตุเมื่อเกิดเหตุละเมิดความเป็นส่วนตัวโดยมีข้อมูล เช่น ประเภทข้อมูลส่วนบุคคลที่รั่วไหล
							จำนวนเจ้าของข้อมูลที่ได้รับผลกระทบ ความร้ายแรงและผลกระทบที่อาจเกิดต่อเจ้าของข้อมูล รายละเอียดของเหตุการณ์ เป็นต้น
						</p>
					</b-col>
				</b-row>
			</base-block>
		</b-form>
	</div>
</template>

<script>
export default {
	data() {
		return {
			optionsRadios: [
				{ value: 'Yes', text: 'ดำเนินการแล้ว' },
				{ value: 'No', text: 'ยังไม่ได้ดำเนินการ' },
				{ value: 'Partially', text: 'ดำเนินการแล้วบางส่วน' },
				{ value: 'Not Sure', text: 'ไม่แน่ใจ' },
				{ value: 'Not Applicable', text: 'ไม่ได้รับอนุญาต' },
			],
		}
	},
	props: {
		form: Object,
	},
	methods: {
		status(evt) {
			var stat
			if (evt == 'Yes') {
				stat = 'ดำเนินการแล้ว'
			}
			if (evt == 'No') {
				stat = 'ยังไม่ได้ดำเนินการ'
			}
			if (evt == 'Partially') {
				stat = 'กำลังดำเนินการ'
			}
			if (evt == 'Not Sure') {
				stat = 'ไม่แน่ใจ'
			}
			if (evt == 'Not Applicable') {
				stat = 'ไม่ได้รับอนุญาต'
			}
			return stat
		},
		icon(evt) {
			var name
			if (evt == 'Yes') {
				name = 'check-circle-fill'
			}
			if (evt == 'No') {
				name = 'x-circle-fill'
			}
			if (evt == 'Partially') {
				name = 'exclamation-circle-fill'
			}
			if (evt == 'Not Sure') {
				name = 'x-circle-fill'
			}
			if (evt == 'Not Applicable') {
				name = 'x-circle-fill'
			}
			return name
		},
		variant(evt) {
			var name
			if (evt == 'Yes') {
				name = 'success'
			}
			if (evt == 'No') {
				name = 'danger'
			}
			if (evt == 'Partially') {
				name = 'warning'
			}
			if (evt == 'Not Sure') {
				name = 'secondary'
			}
			if (evt == 'Not Applicable') {
				name = 'dark'
			}
			return name
		},
	},
}
</script>

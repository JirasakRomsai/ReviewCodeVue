<template>
  <div class="mb-12 font-s">
    <v-card tile v-if="data.title !== 1">
      <v-flex d-flex xs12>
        <v-card :class="checkSuccess[1]" width="100%" style="height: 200px">
          <v-row class="h-100 pt-9">
            <v-col cols="8" sm="7" md="8" lg="6" xl="6" class="pl-md-12 pl-sm-4 pl-4">
              <p style="font-size: 26px; font-weight: 700" :class="checkSuccess[0]">
								{{ $t('GapAnalysis.Progress') }}
              </p>
              <p style="font-size: 18px; font-weight: 700">{{ $t('GapAnalysis.NotImplemented') }} : {{ checkNumNotSuccess }}</p>
            </v-col>
            <v-col cols="4" sm="5" md="4" lg="6" xl="6" class="text-center">
              <!-- <p style="font-size: 74px; font-weight: 700" class="txtPerSuc" :class="checkSuccess[0]">{{ checkSumSuccessTitle }}/{{ checkNumVerses }}</p> -->
              <p  class="txtPerSuc" :class="checkSuccess[0]">{{ checkSumSuccessTitle }}/{{ checkNumVerses }}</p>
            </v-col>
          </v-row>
        </v-card>
      </v-flex>
      </v-card>

    <v-flex class="d-flex flex-column mt-4" v-if="data.title === 1">
      <p style="font-size: 20px; font-weight: 700">
        1. {{ $t('GapAnalysis.ListAssForm.Title1') }}
      </p>
      <v-card width="100%">
        <v-card-title>
          <p class="p-title">1.1 {{ $t('GapAnalysis.ListAssForm.SubTitle1.Title') }}</p>
          <p class="example">
             {{ $t('GapAnalysis.ListAssForm.SubTitle1.Detail') }}
          </p>
          <ul class="example">
            <li> {{ $t('GapAnalysis.ListAssForm.SubTitle1.Sub1') }}</li>
            <li>{{ $t('GapAnalysis.ListAssForm.SubTitle1.Sub2') }}</li>
            <li>{{ $t('GapAnalysis.ListAssForm.SubTitle1.Sub3') }}</li>
          </ul>
          <p class="example">
           {{ $t('GapAnalysis.ListAssForm.SubTitle1.Sumary') }}
          </p>
        </v-card-title>
      </v-card>
    </v-flex>

    <v-flex class="d-flex flex-column mt-8" v-if="data.title === 2">
      <p style="font-size: 20px; font-weight: 700">2. {{ $t('GapAnalysis.ListAssForm.Title2') }}</p>
      <v-card width="100%" class="mt-4">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              2.1 {{ $t('GapAnalysis.ListAssForm.Subtitle2.Sub1') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.two[0].check1[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-1 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.two[0].text1"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle2.Advice1') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              2.2 {{ $t('GapAnalysis.ListAssForm.Subtitle2.Sub2') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.two[0].check2[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.two[0].text2"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle2.Advice2') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">2.3 {{ $t('GapAnalysis.ListAssForm.Subtitle2.Sub3') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.two[0].check3[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.two[0].text3"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle2.Advice3') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              2.4 {{ $t('GapAnalysis.ListAssForm.Subtitle2.Sub4') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.two[0].check4[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.two[0].text4"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle2.Advice4') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              2.5 {{ $t('GapAnalysis.ListAssForm.Subtitle2.Sub5') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.two[0].check5[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.two[0].text5"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle2.Advice5') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
    </v-flex>

    <v-flex class="d-flex flex-column mt-8" v-if="data.title === 3">
      <p style="font-size: 20px; font-weight: 700">3. {{ $t('GapAnalysis.ListAssForm.Title3') }}</p>
      <v-card width="100%" class="mt-4">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              3.1 {{ $t('GapAnalysis.ListAssForm.Subtitle3.Sub1') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.three[0].check1[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.three[0].text1"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle3.Advice1') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              3.2 {{ $t('GapAnalysis.ListAssForm.Subtitle3.Sub2') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.three[0].check2[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.three[0].text2"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle3.Advice2') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              3.3 {{ $t('GapAnalysis.ListAssForm.Subtitle3.Sub3') }}
            </p>
            <p class="p-sub-title">
            <ul>
              <li>{{ $t('GapAnalysis.ListAssForm.Subtitle3.Ex3') }}</li>
            </ul>
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.three[0].check3[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.three[0].text3"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle3.Advice3') }} </p>       
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              3.4 {{ $t('GapAnalysis.ListAssForm.Subtitle3.Sub4') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.three[0].check4[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.three[0].text4"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle3.Advice4') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
    </v-flex>

    <v-flex class="d-flex flex-column mt-8" v-if="data.title === 4">
      <p style="font-size: 20px; font-weight: 700">4. {{ $t('GapAnalysis.ListAssForm.Title4') }}</p>
      <v-card width="100%" class="mt-4">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              4.1 {{ $t('GapAnalysis.ListAssForm.Subtitle4.Sub1') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.four[0].check1[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.four[0].text1"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle4.Advice1') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              4.2 {{ $t('GapAnalysis.ListAssForm.Subtitle4.Sub2') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.four[0].check2[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.four[0].text2"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle4.Advice2') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">4.3 {{ $t('GapAnalysis.ListAssForm.Subtitle4.Sub3') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.four[0].check3[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.four[0].text3"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            >
            </v-textarea>
            <p class="p-sub-title" >
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle4.Advice3') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
    </v-flex>

    <v-flex class="d-flex flex-column mt-8" v-if="data.title === 5">
      <p style="font-size: 20px; font-weight: 700">5. {{ $t('GapAnalysis.ListAssForm.Title5') }}</p>
      <v-card width="100%" class="mt-4">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">5.1 {{ $t('GapAnalysis.ListAssForm.Subtitle5.Sub1') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.five[0].check1[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.five[0].text1"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle5.Advice1') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              5.2 {{ $t('GapAnalysis.ListAssForm.Subtitle5.Sub2') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.five[0].check2[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.five[0].text2"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} :{{ $t('GapAnalysis.ListAssForm.Subtitle5.Advice2') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              5.3 {{ $t('GapAnalysis.ListAssForm.Subtitle5.Sub3') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.five[0].check3[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.five[0].text3"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle5.Advice3') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              5.4 {{ $t('GapAnalysis.ListAssForm.Subtitle5.Sub4') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.five[0].check4[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.five[0].text4"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
          </v-flex>
          <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} :  {{ $t('GapAnalysis.ListAssForm.Subtitle5.Advice4') }}</p>
        </v-card-title>
      </v-card>
    </v-flex>

    <v-flex class="d-flex flex-column mt-8" v-if="data.title === 6">
      <p style="font-size: 20px; font-weight: 700">6. {{ $t('GapAnalysis.ListAssForm.Title6') }}</p>
      <v-card width="100%" class="mt-4">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              6.1  {{ $t('GapAnalysis.ListAssForm.Subtitle6.Sub1') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.six[0].check1[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.six[0].text1"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
          </v-flex>
          <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle6.Advice1') }}</p>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              6.2 {{ $t('GapAnalysis.ListAssForm.Subtitle6.Sub2') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.six[0].check2[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.six[0].text2"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle6.Advice2') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              6.3 {{ $t('GapAnalysis.ListAssForm.Subtitle6.Sub3') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.six[0].check3[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.six[0].text3"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">{{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle6.Advice3') }}</p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">6.4 {{ $t('GapAnalysis.ListAssForm.Subtitle6.Sub4') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.six[0].check4[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.six[0].text4"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle6.Advice4') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">6.5 {{ $t('GapAnalysis.ListAssForm.Subtitle6.Sub5') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.six[0].check5[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.six[0].text5"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
          </v-flex>
          <p class="p-sub-title">
            {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle6.Advice5') }}
          </p>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">6.6 {{ $t('GapAnalysis.ListAssForm.Subtitle6.Sub6') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.six[0].check6[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.six[0].text6"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
          </v-flex>
          <p class="p-sub-title">
            {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle6.Advice6') }}
          </p>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">6.7 {{ $t('GapAnalysis.ListAssForm.Subtitle6.Sub7') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.six[0].check7[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.six[0].text7"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
          </v-flex>
          <p class="p-sub-title">
            {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle6.Advice7') }}
          </p>
        </v-card-title>
      </v-card>
    </v-flex>

    <v-flex class="d-flex flex-column mt-8" v-if="data.title === 7">
      <p style="font-size: 20px; font-weight: 700">7. {{ $t('GapAnalysis.ListAssForm.Title7') }}</p>
      <v-card width="100%" class="mt-4">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              7.1 {{ $t('GapAnalysis.ListAssForm.Subtitle7.Sub1') }}
            </p>
              <b-form-radio-group
              @change="changeValue"
              v-model="formD.seven[0].check1[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.seven[0].text1"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle7.Advice1') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              7.2 {{ $t('GapAnalysis.ListAssForm.Subtitle7.Sub2') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.seven[0].check2[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.seven[0].text2"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle7.Advice2') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              7.3 {{ $t('GapAnalysis.ListAssForm.Subtitle7.Sub3') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.seven[0].check3[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.seven[0].text3"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} :
            {{ $t('GapAnalysis.ListAssForm.Subtitle7.Advice3') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              7.4  {{ $t('GapAnalysis.ListAssForm.Subtitle7.Sub4') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.seven[0].check4[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.seven[0].text4"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle7.Advice4') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">
              7.5 {{ $t('GapAnalysis.ListAssForm.Subtitle7.Sub5') }}
            </p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.seven[0].check5[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.seven[0].text5"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle7.Advice5') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
    </v-flex>

    <v-flex class="d-flex flex-column mt-8" v-if="data.title === 8">
      <p style="font-size: 20px; font-weight: 700">8. {{ $t('GapAnalysis.ListAssForm.Title8') }}</p>
      <v-card width="100%" class="mt-4">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">8.1 {{ $t('GapAnalysis.ListAssForm.Subtitle8.Sub1') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.eight[0].check1[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.eight[0].text1"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle8.Advice1') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
      <v-card width="100%" class="mt-10">
        <v-card-title>
          <v-flex class="d-flex flex-column">
            <p class="p-title">8.2 {{ $t('GapAnalysis.ListAssForm.Subtitle8.Sub2') }}</p>
            <b-form-radio-group
              @change="changeValue"
              v-model="formD.eight[0].check2[0]"
              :options="optionsRadios"
              stacked
              class="form-radio"
            ></b-form-radio-group>
            <p class="mt-2 p-detail">{{ $t('GapAnalysis.ListAssForm.DescripOptional') }}</p>
            <v-textarea
              @keydown="changeValue"
              v-model="formD.eight[0].text2"
              background-color="#F8F9FA"
              color="black"
              filled
              auto-grow
              row-height="10"
            ></v-textarea>
            <p class="p-sub-title">
              {{ $t('GapAnalysis.ListAssForm.Advice') }} : {{ $t('GapAnalysis.ListAssForm.Subtitle8.Advice2') }}
            </p>
          </v-flex>
        </v-card-title>
      </v-card>
    </v-flex>
    <div
      v-if="data.title !== 1"
      style="position: fixed; z-index: 1; background-color: #343a40; bottom: 0px; right: 0px; width: 100%; height: 80px; opacity: 0.1"
    ></div>

    <b-button
      v-if="data.title !== 1"
      fab
      variant="primary"
      style="color: #ffffff; margin: 0px 30px 21px 0px; width: 180px; position: fixed; z-index: 1; bottom: 0px; right: 0px; opacity: 1"
      class="mt-3"
      @click="submitForm()"
      >{{ $t('GapAnalysis.ListAssForm.Button.Save') }}
    </b-button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['data'],
  data() {
    return {
      // optionsRadios: [
      //   { value: 'Yes', text: this.$t('GapAnalysis.Select.Implemented') },
      //   { value: 'No', text:  this.$t('GapAnalysis.Select.NotImplemented') },
      //   { value: 'Partially', text: this.$t('GapAnalysis.Select.Partial') },
      //   { value: 'Not Sure', text: this.$t('GapAnalysis.Select.NotSure') },
      // ],
      formD: {
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
        three: [{ 
          check1: ['Not Sure'],
          check2: ['Not Sure'],
          check3: ['Not Sure'],
          check4: ['Not Sure'],
          text1: '-',
          text2: '-', 
          text3: '-',
          text4: '-' },
        ],
        four: [{ 
           check1: ['Not Sure'],
           check2: ['Not Sure'], 
           check3: ['Not Sure'],
           text1: '-', 
           text2: '-',
           text3: '-'
         }],
        five: [{ 
          check1: ['Not Sure'],
          check2: ['Not Sure'],
          check3: ['Not Sure'],
          check4: ['Not Sure'],
          text1: '-',
          text2: '-',
          text3: '-',
          text4: '-' },
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
            text3: '-',
            text1: '-',
            text2: '-',
            text4: '-',
            text5: '-',
          },
        ],
        eight: [{
           check1: ['Not Sure'],
           check2: ['Not Sure'], 
           text1: '-',
           text2: '-'
         }],
      },
     
      checkClickSave: false,
      dataCheckNumNotSuccess: null,
      dataCheckSuccess: null,
      dataCheckSumSuccessTitle: null,
      dataCheckNumVerses: null,

      dtTestST:'',
      storeChangeLanguage:''

      
    }
  },
  created: function () {
    this.pushForm()
    this.prepareLayout()
    // console.log("getter ",this.$store.getters['for18n/getChangeLanguage']);
    // console.log("ny ",this.$store.state.for18n.changeLanguage);
    // this.storeChangeLanguage = this.$store.state.for18n.changeLanguage

  },

  watch: {
    data() {
      window.scrollTo({
        top: 0,
        left: 0,
        behavior: 'smooth',
      })
      this.prepareLayout()
    },
  },
  computed: {
    optionsRadios(){
      return [
        { value: 'Yes', text: this.$t('GapAnalysis.Select.Implemented') },
        { value: 'No', text:  this.$t('GapAnalysis.Select.NotImplemented') },
        { value: 'Partially', text: this.$t('GapAnalysis.Select.Partial') },
        { value: 'Not Sure', text: this.$t('GapAnalysis.Select.NotSure') },
      ]
    },
    checkSuccess() {
      let result = false
      let sum = 0

      if (this.data.title === 2) {
        for (const k in this.formD.two[0]) {
          if (`${this.formD.two[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
        result = sum === 5 ? true : false
      }
      if (this.data.title === 3) {
        for (const k in this.formD.three[0]) {
          if (`${this.formD.three[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
        result = sum === 4 ? true : false
      }
      if (this.data.title === 4) {
        for (const k in this.formD.four[0]) {
          if (`${this.formD.four[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
        result = sum === 3 ? true : false
      }
      if (this.data.title === 5) {
        for (const k in this.formD.five[0]) {
          if (`${this.formD.five[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
        result = sum === 4 ? true : false
      }
      if (this.data.title === 6) {
        for (const k in this.formD.six[0]) {
          if (`${this.formD.six[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
        result = sum === 7 ? true : false
      }
      if (this.data.title === 7) {
        for (const k in this.formD.seven[0]) {
          if (`${this.formD.seven[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
        result = sum === 5 ? true : false
      }
      if (this.data.title === 8) {
        for (const k in this.formD.eight[0]) {
          if (`${this.formD.eight[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
        result = sum === 2 ? true : false
      }
      return result ? ['successed', 'box-shadow-card-success '] : ['not-successed', 'box-shadow-card-not-success']
    },
    checkNumNotSuccess() {
      let { title } = this.data
      let str = ''
      if (this.data.title === 2) {
        for (const k in this.formD.two[0]) {
          if (`${this.formD.two[0][k][0]}` !== 'Yes' && k.includes('check')) {
            str += title + '.' + k.substr(5, 5) + ', '
          }
        }
      }
      if (this.data.title === 3) {
        for (const k in this.formD.three[0]) {
          if (`${this.formD.three[0][k][0]}` !== 'Yes' && k.includes('check')) {
            str += title + '.' + k.substr(5, 5) + ', '
          }
        }
      }
      if (this.data.title === 4) {
        for (const k in this.formD.four[0]) {
          if (`${this.formD.four[0][k][0]}` !== 'Yes' && k.includes('check')) {
            str += title + '.' + k.substr(5, 5) + ', '
          }
        }
      }
      if (this.data.title === 5) {
        for (const k in this.formD.five[0]) {
          if (`${this.formD.five[0][k][0]}` !== 'Yes' && k.includes('check')) {
            str += title + '.' + k.substr(5, 5) + ', '
          }
        }
      }
      if (this.data.title === 6) {
        for (const k in this.formD.six[0]) {
          if (`${this.formD.six[0][k][0]}` !== 'Yes' && k.includes('check')) {
            str += title + '.' + k.substr(5, 5) + ', '
          }
        }
      }
      if (this.data.title === 7) {
        for (const k in this.formD.seven[0]) {
          if (`${this.formD.seven[0][k][0]}` !== 'Yes' && k.includes('check')) {
            str += title + '.' + k.substr(5, 5) + ', '
          }
        }
      }
      if (this.data.title === 8) {
        for (const k in this.formD.eight[0]) {
          if (`${this.formD.eight[0][k][0]}` !== 'Yes' && k.includes('check')) {
            str += title + '.' + k.substr(5, 5) + ', '
          }
        }
      }
      return str.length === 0 ? ' - ' : str.substring(0, str.length - 2)
    },
    checkSumSuccessTitle() {
      let sum = 0
      if (this.data.title === 2) {
        for (const k in this.formD.two[0]) {
          if (`${this.formD.two[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
      }
      if (this.data.title === 3) {
        for (const k in this.formD.three[0]) {
          if (`${this.formD.three[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
      }
      if (this.data.title === 4) {
        for (const k in this.formD.four[0]) {
          if (`${this.formD.four[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
      }
      if (this.data.title === 5) {
        for (const k in this.formD.five[0]) {
          if (`${this.formD.five[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
      }
      if (this.data.title === 6) {
        for (const k in this.formD.six[0]) {
          if (`${this.formD.six[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
      }
      if (this.data.title === 7) {
        for (const k in this.formD.seven[0]) {
          if (`${this.formD.seven[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
      }
      if (this.data.title === 8) {
        for (const k in this.formD.eight[0]) {
          if (`${this.formD.eight[0][k][0]}` === 'Yes') {
            sum += 1
          }
        }
      }
      return sum
    },
    checkNumVerses() {
      let { title } = this.data
      let x = 0
      if (title === 2) {
        x = 5
      } else if (title === 3) {
        x = 4
      } else if (title === 4) {
        x = 3
      } else if (title === 5) {
        x = 4
      } else if (title === 6) {
        x = 7
      } else if (title === 7) {
        x = 5
      } else if (title === 8) {
        x = 2
      }
      return x
    },
  },
  methods: {
    prepareLayout() {
      // this.checkNumNotSuccess()
      // this.checkSuccess()
      // this.checkSumSuccessTitle()
      // this.checkSumSuccessTitle()
      // this.checkNumVerses()
    },
    // checkNumVerses() {
    //   let { title } = this.data
    //   let x = 0
    //   if (title === 2) {
    //     x = 5
    //   } else if (title === 3) {
    //     x = 4
    //   } else if (title === 4) {
    //     x = 3
    //   } else if (title === 5) {
    //     x = 4
    //   } else if (title === 6) {
    //     x = 7
    //   } else if (title === 7) {
    //     x = 5
    //   } else if (title === 8) {
    //     x = 2
    //   }
    //   this.dataCheckNumVerses = x
    // },
    // checkSumSuccessTitle() {
    //   let sum = 0
    //   for (const k in this.data.data) {
    //     if (`${this.data.data[k][0]}` === 'Yes') {
    //       sum += 1
    //     }
    //   }
    //   this.dataCheckSumSuccessTitle = sum
    // },
    // checkNumNotSuccess() {
    //   let { data, title } = this.data
    //   let str = ''
    //   for (const k in data) {
    //     if (`${data[k][0]}` !== 'Yes' && k.includes('check')) {
    //       str += title + '.' + k.substr(5, 5) + ', '
    //     }
    //   }
    //   this.dataCheckNumNotSuccess = str.length === 0 ? ' - ' : str.substring(0, str.length - 2)
    // },
    // checkSuccess() {
    //   let result = false
    //   let sum = 0
    //   for (const k in this.data.data) {
    //     if (`${this.data.data[k][0]}` === 'Yes') {
    //       sum += 1
    //     }
    //   }
    //   if (this.data.title === 2 && sum === 5) {
    //     result = true
    //   }
    //   if (this.data.title === 3 && sum === 4) {
    //     result = true
    //   }
    //   if (this.data.title === 4 && sum === 3) {
    //     result = true
    //   }
    //   if (this.data.title === 5 && sum === 4) {
    //     result = true
    //   }
    //   if (this.data.title === 6 && sum === 7) {
    //     result = true
    //   }
    //   if (this.data.title === 7 && sum === 5) {
    //     result = true
    //   }
    //   if (this.data.title === 8 && sum === 2) {
    //     result = true
    //   }

    //   this.dataCheckSuccess = result ? ['successed', 'box-shadow-card-success '] : ['not-successed', 'box-shadow-card-not-success']
    // },
    pushForm() {
      let { dataAll } = this.data
      for (let i in dataAll) {
        for (let j in dataAll[i][0]) {
          if(this.formD[i] !== undefined){
            this.formD[i][0][j] = dataAll[i][0][j]
          }
        }
      }
    },
    changeValue() {
       const formData = {
        data: this.formD,
        date: new Date().toLocaleDateString(),
      }
      this.$store.dispatch('asessmentUpdate/changeInput',{inputChanege: true, formData})
    },
    checkTextEmpthy() {
      if (this.formD.two[0].text1 == (null || '')) {
        this.formD.two[0].text1 = '-'
      }
      if (this.formD.two[0].text2 == (null || '')) {
        this.formD.two[0].text2 = '-'
      }
      if (this.formD.two[0].text3 == (null || '')) {
        this.formD.two[0].text3 = '-'
      }
      if (this.formD.two[0].text4 == (null || '')) {
        this.formD.two[0].text4 = '-'
      }
      if (this.formD.two[0].text5 == (null || '')) {
        this.formD.two[0].text5 = '-'
      }
      if (this.formD.three[0].text1 == (null || '')) {
        this.formD.three[0].text1 = '-'
      }
      if (this.formD.three[0].text2 == (null || '')) {
        this.formD.three[0].text2 = '-'
      }
      if (this.formD.three[0].text3 == (null || '')) {
        this.formD.three[0].text3 = '-'
      }
      if (this.formD.three[0].text4 == (null || '')) {
        this.formD.three[0].text4 = '-'
      }
      if (this.formD.four[0].text1 == (null || '')) {
        this.formD.four[0].text1 = '-'
      }
      if (this.formD.four[0].text2 == (null || '')) {
        this.formD.four[0].text2 = '-'
      }
      if (this.formD.four[0].text3 == (null || '')) {
        this.formD.four[0].text3 = '-'
      }
      if (this.formD.five[0].text1 == (null || '')) {
        this.formD.five[0].text1 = '-'
      }
      if (this.formD.five[0].text2 == (null || '')) {
        this.formD.five[0].text2 = '-'
      }
      if (this.formD.five[0].text3 == (null || '')) {
        this.formD.five[0].text3 = '-'
      }
      if (this.formD.five[0].text4 == (null || '')) {
        this.formD.five[0].text4 = '-'
      }
      if (this.formD.six[0].text1 == (null || '')) {
        this.formD.six[0].text1 = '-'
      }
      if (this.formD.six[0].text2 == (null || '')) {
        this.formD.six[0].text2 = '-'
      }
      if (this.formD.six[0].text3 == (null || '')) {
        this.formD.six[0].text3 = '-'
      }
      if (this.formD.six[0].text4 == (null || '')) {
        this.formD.six[0].text4 = '-'
      }
      if (this.formD.six[0].text5 == (null || '')) {
        this.formD.six[0].text5 = '-'
      }
      if (this.formD.six[0].text6 == (null || '')) {
        this.formD.six[0].text6 = '-'
      }
      if (this.formD.six[0].text7 == (null || '')) {
        this.formD.six[0].text7 = '-'
      }
      if (this.formD.seven[0].text1 == (null || '')) {
        this.formD.seven[0].text1 = '-'
      }
      if (this.formD.seven[0].text2 == (null || '')) {
        this.formD.seven[0].text2 = '-'
      }
      if (this.formD.seven[0].text3 == (null || '')) {
        this.formD.seven[0].text3 = '-'
      }
      if (this.formD.seven[0].text4 == (null || '')) {
        this.formD.seven[0].text4 = '-'
      }
      if (this.formD.seven[0].text5 == (null || '')) {
        this.formD.seven[0].text5 = '-'
      }
      if (this.formD.eight[0].text1 == (null || '')) {
        this.formD.eight[0].text1 = '-'
      }
      if (this.formD.eight[0].text2 == (null || '')) {
        this.formD.eight[0].text2 = '-'
      }
    },
    async submitForm() {
      this.checkTextEmpthy()
      this.$store.dispatch('asessmentUpdate/changeInput',{inputChanege: false, formData:{}})

      const formData = {
        data: this.formD,
        date: new Date().toLocaleDateString(),
      }
      var URL = this.$appurl + '/Report'
      await axios
        .post(URL, formData)
        .then(() => {
          this.$swal(this.$t('GapAnalysis.DialogConfirmUpdate.Done'), '', 'success')
        })
        .catch((error) =>{
          console.log(error)
          this.$swal(this.$t('GapAnalysis.DialogConfirmUpdate.Failed2'), '', 'error')
        })

          this.$router.push({ name: 'Assessments' })
    },
  },
}
</script>

<style scoped>
.box-shadow-card-success {
  border-left-style: solid;
  border-left-color: green;
  border-left-width: 83px;
}
.box-shadow-card-not-success {
  border-left-style: solid;
  border-left-color: #151d44;
  border-left-width: 83px;
}
.radios-chooise {
  font-size: 13px !important;
  font-weight: 400;
}
.p-title {
  font-size: 16px;
  font-weight: 700;
  word-break: break-word !important;
}
.p-sub-title {
  font-size: 15px;
  font-weight: 400;
  word-break: break-word !important;
}
.p-detail {
  font-size: 15px;
  font-weight: 600;
}
.form-radio {
  font-size: 15px;
}

.example {
  text-indent: 1.5em;
  font-size: 16px;
  word-break: break-word !important;
}
.successed {
  color: green;
}
.not-successed {
  color: #151d44;
}
.font-s {
  font-family: 'Sarabun', sans-serif;
}


@media only screen and (min-width: 600px) {
  .txtPerSuc{
    font-size: 55px;
  }
}
@media only screen and (min-width: 960px) {
  .txtPerSuc{
    font-size: 65px;
  }
}
@media only screen and (min-width: 1464px) {
  .txtPerSuc{
    font-size: 72px;
  }
}
@media only screen and (min-width: 1904px) {
  .txtPerSuc{
    font-size: 50px;
  }
}
.txtPerSuc{
  font-weight: 700;
  font-size: 50px;
}
</style>

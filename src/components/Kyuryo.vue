<template>
  <v-container>
    <v-row justify="center" class="mb-n10">
      <v-col cols="12">
        <div id="kyuryo-main">
          {{ String(earn).split(".")[0] }}
          <span id="kyuryo-sub">
          .{{ String(earn).split(".")[1] }}
          </span>
            円
          </div>
        <button @click='calcEarnedMoney'></button>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-btn
      color="#1B95E0"
      dark
      depressed
      :href="tweetContent"
      target="_blank"
      rel="nofollow noopener"
      >
      <v-icon>
        mdi-twitter
      </v-icon>
         Tweet
      </v-btn>
    </v-row>
    <v-row justify="center">
      <v-col cols="6" md="5" lg="5" xl="3">
        <v-text-field
          label="時給"
          type="number"
          suffix="円/時"
          @change="changeHourlyWage"
          v-model="hourlyWage"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="center">
     <v-col
      cols="6"
      lg="4"
      xl="2"
    >
      <v-menu
        ref="menu1"
        v-model="menu1"
        :close-on-content-click="false"
        :nudge-right="40"
        :return-value.sync="start"
        transition="scale-transition"
        offset-y
        max-width="290px"
        min-width="290px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="start"
            label="始業時間"
            prepend-icon="mdi-clock-time-four-outline"
            readonly
            v-bind="attrs"
            v-on="on"
          ></v-text-field>
        </template>
        <v-time-picker
          v-if="menu1"
          v-model="start"
          format="24hr"
          full-width
          @input="changeStartTime"
          @click:minute="$refs.menu1.save(start)"
        ></v-time-picker>
      </v-menu>
    </v-col>
     <v-col
      cols="6"
      lg="4"
      xl="2"
    >
      <v-menu
        ref="menu2"
        v-model="menu2"
        :close-on-content-click="false"
        :nudge-right="40"
        :return-value.sync="end"
        transition="scale-transition"
        offset-y
        max-width="290px"
        min-width="290px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="end"
            label="終業時間"
            prepend-icon="mdi-clock-time-four-outline"
            readonly
            v-bind="attrs"
            v-on="on"
          ></v-text-field>
        </template>
        <v-time-picker
          v-if="menu2"
          v-model="end"
          full-width
          format="24hr"
          @input="changeEndTime"
          @click:minute="$refs.menu2.save(end);"
        ></v-time-picker>
      </v-menu>
    </v-col>
    </v-row>
    <v-row justify="center" dense>
      <v-col cols="6" xs="12" sm="6" md="6" lg="4" xl="2">
        <v-switch
          dense
          v-model="allowMinus"
          label="始業前労働を許す"
          @change="changeAllowMinueSwitch"
          >
        </v-switch>
      </v-col>
      <v-col cols="6" xs="12" sm="6" md="6" lg="4" xl="2">
        <v-switch
          dense
          v-model="allowOver"
          label="終業後労働を許す"
          @change="changeAllowOverSwitch">
        </v-switch>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-switch
        dense
        v-model="viewSalaryOnTitle"
        label="タイトルに表示する"
        @change="changeViewSalaryOnTitleSwitch"
        >
      </v-switch>
    </v-row>
    <h4 style="text-align: center" class="ma-3 mt-10">
      ↓自分へのご褒美を買う↓
    </h4>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="7" lg="5" xl="4">
        <rakuten-affiliate />
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="7" lg="5" xl="4">
        <amazon-affiliate />
      </v-col>
    </v-row>
    <h4 style="text-align: center" class="ma-3 mt-10">
      休憩や残業は考慮しません．<br />
      入力した情報は外部に保存していません．<br /> 作者: <a href="https://sumika.unyacat.net">うにゃ</a>
    </h4>
  </v-container>
</template>

<script>
import dayjs from 'dayjs'
import RakutenAffiliate from './RakutenAffiliate.vue'
import AmazonAffiliate from './AmazonAffiliate.vue'


export default {
  name: 'kyuryo',
  data() {
    return {
      earn: 0,        // 獲得
      hourlyWage: 1000,   // 時給
      start: "16:30",
      end: "21:30",
      menu1: false,
      menu2: false,
      allowMinus: false,
      allowOver: false,
      viewSalaryOnTitle: false
    }
  },
  components: {
    RakutenAffiliate,
    AmazonAffiliate
  },
  methods: {
    calcEarnedMoney: function() {
      const startDate = dayjs().format("YYYY/MM/DD") + " " + this.start
      const endDate = dayjs().format("YYYY/MM/DD") + " " + this.end
      let start = dayjs(startDate)
      const end = dayjs(endDate)
      if (end < start) {
        start = start.subtract(1, 'd')
      }
      const now = dayjs()
      if (now < start && !this.allowMinus) {
        this.earn = 0
      }
      else if (now < end || this.allowOver) {
        this.earn = (((now - start) / 1000) * this.secWage).toFixed(3)
      }
      else {
        this.earn = (((end - start) / 1000) * this.secWage).toFixed(3)
      }
      if (this.viewSalaryOnTitle) {
        const title = String(this.earn).split(".")[0] + " 円"
        document.title = title
      }
      else {
        document.title = "秒給メーター"
      }
    },
    changeStartTime: function() {
      localStorage.start = this.start
    },
    changeEndTime: function() {
      localStorage.end = this.end
    },
    changeHourlyWage: function() {
      localStorage.hourlyWage = this.hourlyWage
    },
    changeAllowMinueSwitch: function() {
      localStorage.allowMinus = this.allowMinus
    },
    changeAllowOverSwitch: function() {
      localStorage.allowOver = this.allowOver
    },
    changeViewSalaryOnTitleSwitch: function() {
      localStorage.viewSalaryOnTitle = this.viewSalaryOnTitle
    }
  },
  mounted() {
    if (localStorage.getItem("start")) {
      this.start = localStorage.start
    }
    if (localStorage.getItem("end")) {
      this.end = localStorage.end
    }
    if (localStorage.getItem("hourlyWage")) {
      this.hourlyWage = localStorage.hourlyWage
    }
    if (localStorage.getItem("changeAllowMinusSwitch")) {
      this.allowMinus = localStorage.allowMinus
    }
    if (localStorage.getItem("allowOver")) {
      this.allowOver = localStorage.allowOver
    }
    if (localStorage.getItem("viewSalaryOnTitle")) {
      this.viewSalaryOnTitle = localStorage.viewSalaryOnTitle
    }
    setInterval(this.calcEarnedMoney, 1)
  },
  computed: {
    secWage: function() {
      return this.hourlyWage / (60 * 60)
    },
    tweetContent: function() {
      const url = "https://twitter.com/intent/tweet?text="
      const content = "今の給料は " + this.earn + " 円！ " + "あなたも計算してみよう %23秒給メーター https://second-pay.work"
      return url + content
    }
  }
}
</script>

<style>
#kyuryo-main {
  font-size: 84px;
  text-align: center;
}

@media screen and (max-width:480px) {
  #kyuryo-main {
    font-size: 60px
  }
}


#kyuryo-sub {
  font-size : 32px;
  margin-left: -10px;
}
</style>
<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" >
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
      <v-col cols="3">
        <v-text-field
          label="時給"
          type="number"
          suffix="円/時"
          @change="changeHourlyWage"
          v-model="hourlyWage"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
     <v-col
      cols="11"
      sm="5"
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
            label="Picker in menu"
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
    <v-spacer />
     <v-col
      cols="11"
      sm="5"
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
            label="Picker in menu"
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
    <v-row justify="center">
      <v-col cols="4" >
        <v-switch
          v-model="allowMinus"
          label="始業前労働を許す">
        </v-switch>
      </v-col>
    </v-row>
     <v-row justify="center">
      <v-col cols="4">
        <v-switch
          v-model="allowOver"
          label="終業後労働を許す">
        </v-switch>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import dayjs from 'dayjs'
export default {
  data() {
    return {
      earn: 0,        // 獲得
      hourlyWage: 1000,   // 時給
      start: "16:30",
      end: "21:30",
      menu1: false,
      menu2: false,
      allowMinus: false,
      allowOver: false
    }
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
    },
    changeStartTime: function() {
      localStorage.start = this.start
    },
    changeEndTime: function() {
      localStorage.end = this.end
    },
    changeHourlyWage: function() {
      localStorage.hourlyWage = this.hourlyWage
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
    setInterval(this.calcEarnedMoney, 1)
  },
  computed: {
    secWage: function() {
      return this.hourlyWage / (60 * 60)
    }
  }
}
</script>

<style>
#kyuryo-main {
  font-size: 84px;
  text-align: center;
}

#kyuryo-sub {
  font-size : 32px;
}
</style>
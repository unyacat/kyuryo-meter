<template>
  <div>
    <div id="kyuryo-main">
      {{ earnedMoney }}
      <span id="kyuryo-sub">
        .{{ earn }}
      </span>
      円
    </div>
    <button @click='calcEarnedMoney'></button>
  </div>
</template>

<script>
import dayjs from 'dayjs'

export default {
  data() {
    return {
      earn: 3853
    }
  },
  methods: {
    calcEarnedMoney: function() {
      const from = dayjs('2021-03-16 21:00')
      let now = dayjs()
      let res = now - from
      this.earn = res
    }
  },
  mounted() {
    setInterval(this.calcEarnedMoney, 1)
  },
  computed: {
    earnedMoney: function() {
      let res = this.earn
      return res
    }
  }
}
</script>

<style>
#kyuryo-main {
  font-size: 84px;
}

#kyuryo-sub {
  font-size : 32px;
}
</style>
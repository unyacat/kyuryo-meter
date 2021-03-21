<template>
  <div>
    <div v-if="windowSize > 960" style="text-align: center">
      <iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?o=9&p=13&l=ez&f=ifr&linkID=b575064ae015ad82e5bd6886ec15ae8c&t=unyacat03-22&tracking_id=unyacat03-22" width="468" height="60" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
    </div>
    <div v-if="windowSize < 960" style="text-align: center">
      <iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?o=9&p=12&l=ez&f=ifr&linkID=0d2903fc6299e4e286c2da147cdfdd5e&t=unyacat03-22&tracking_id=unyacat03-22" width="300" height="250" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AmazonAffiliate',
  data() {
    return {
      windowSize: 0
    }
  },
  created() {
    this.windowSize = window.innerWidth
  }
}
</script>

<style>
.iframe-parent {
   position: relative;
   width: 100%;
   padding-top: 12.8%;
}
.iframe-parent iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

img {
  width: 100%;
}
</style>
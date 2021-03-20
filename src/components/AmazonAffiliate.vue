<template>
  <div class="iframe-parent">
    <iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?o=9&p=13&l=ez&f=ifr&linkID=b575064ae015ad82e5bd6886ec15ae8c&t=unyacat03-22&tracking_id=unyacat03-22" width="468" height="60" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
  </div>
</template>

<script>
export default {
  name: 'AmazonAffiliate'
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
</style>
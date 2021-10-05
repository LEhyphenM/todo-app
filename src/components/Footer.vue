<template>
  <div id="footer">
    <h6>{{ footer }}</h6>
  </div>
</template>

<script>
export default {
  name: "Footer",
  data() {
    return {
      footer: "Lauren Elliott-Manheim • 2015-2022 • Ex astris, scientia"
    }
  },
}
</script>
<style lang="scss">
  @import "../scss/variables.scss";
  @import "../scss/mixins.scss";
  #footer h6 {
    @include defaultText();
    font-weight:400;
    letter-spacing:2.25px;
    text-transform:uppercase;
    padding:16px;
    text-align:center;
    margin:0 auto;
  }
  // X-Small devices (portrait phones, less than 576px)
  @media (max-width: 575.98px) {
    #footer h6 {
      max-width:300px;
    }
  }
</style>
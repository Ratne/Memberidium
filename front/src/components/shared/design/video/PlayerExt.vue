<template>
  <div class="customVideo" id="ext">
<span></span>

  </div>
</template>
<script>


export default {
  name: "PlayerExt",
  props: {
    script: {
      type: String,
    },
  },
  mounted() {

      const html = this.script
      const scriptEl = document.createRange().createContextualFragment(html);
      document.getElementById('ext').append(scriptEl)

  },

}
</script>

<style lang="scss" scoped>
#ext{
  //min-height: 300px;
  aspect-ratio: 16/9;
}
</style>

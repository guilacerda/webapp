<template lang="pug">
  q-layout
    q-page-container
      div.flex.row-reverse.topbar#background-irri
      router-view
</template>

<script>
</script>

<style lang="stylus" scoped>
#background-irri
  background-color #0a5959
  background-image url('../statics/images/ativo5.png')
  background-repeat no-repeat
  background-position center
  background-size 90px

.topbar
  width 100%
  height 50px
  top 0
  overflow hidden
  position fixed
  z-index 2
  justify-content flex-end
</style>

<template lang="pug">
.PreData(
  v-show="show"
)
  .PreData_flex
    .PreData_hide.mx-3(
      @click="show = false"
    )
      img(
        src="https://icongr.am/clarity/eye-hide.svg?size=32&color=505fd3"
      )
    //- button
    .btn_open(
      @click="open = !open"
    ) {{ open ? 'скрыть API' : 'показать API'}}
  .preBlock(
    v-show="open"
  )
    slot

</template>

<script>
export default {
  data() {
    return {
      open: false,
      show: true,
    }
  },
}
</script>

<style lang="stylus" scoped>

.PreData
  background-color: #f4f6f8;
  &_flex
    display flex
    // justify-content center
    align-items center
  &_hide
    cursor pointer



.btn_open
  // btn()
  // display: inline-block;
  // margin .5rem 0
  // height 2rem
  
  padding: 0.5em 1em;
  border: solid 1px transparent;
  font-size .7rem
  letter-spacing: .01em
  cursor pointer
  border-radius: 4px;
  color: #FFF;
  background-color: #9b4dca //rgba(#9555AF, .7);
  font-weight: 700;
  text-transform: uppercase;
  &:hover
    background #606c76;
    // color: #9050AA;
    // border-color: currentColor;
    // background-color: white;
    // box-shadow: 0 0 0 3px rgba(255, 105, 180, 0.5), 0 0 0 1.5px rgba(255, 105, 180, 0.5);


.preBlock
  background: rgba(#000, .15);
  height: 40vh;
  overflow: overlay
  resize vertical
</style>

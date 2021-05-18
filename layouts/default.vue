<template lang="pug">
div

  NLink.btn(to="/") Main
  NLink.btn( 
    v-for="(V,K) in {2:'dhtmlx-gantt', 3:'Гант + связи',4:'Гант + связи c перетаскиванием', 'fullcalendar': 'fullcalendar'}"
    :to="K" 
  ) {{V}}
  <Nuxt />
</template>

<style lang="styl">
*,
*::before,
*::after
  box-sizing: border-box;
  margin: 0;
html
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;



// https://fvsch.com/styling-buttons
btn()
  // background-color: transparent;
  // color: inherit;
  // touch-action: manipulation;
  user-select none
  cursor: pointer;

  font: inherit;
  font-weight: 500
  text-decoration: none;
  white-space: nowrap;

  transition: all .3s
  outline: none;

  display: inline-grid;
  align-items: center;
  text-align: center
  height 36px
  padding: 0 1rem;


.btn
  btn()
  margin: .3em;
  transition: all .3s
  font-size: 14px;
  color: rgba(#40a9ff,.8);
  border-radius: 4px;
  border: 1px solid #d9d9d9;
  &:hover,
  &.nuxt-link-exact-active
    border-color: #40a9ff;
    color: rgba(#000,.65);


</style>

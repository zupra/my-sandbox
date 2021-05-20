<template lang="pug">
#layout

  #layout_topNav
    NLink.btn(to="/") Main
    NLink.btn( 
      v-for="(V,K) in {2:'dhtmlx-gantt', 3:'Гант + связи',4:'Гант + связи c перетаскиванием', 'fullcalendar': 'fullcalendar'}"
      :to="K" 
  ) {{V}}

  //- SIDEBAR
  #layout_sidebar SIDEBAR


  #layout_main
    <Nuxt />


  #layout_footer © footer
</template>



<style>
#layout {
  display: grid;
  height: 100vh;
  overflow-x: hidden;
  grid-template-columns: auto 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    'sidebar header'
    'sidebar main'
    'sidebar footer';
}
#layout_main,
#layout_sidebar {
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}
#layout_topNav {
  grid-area: header;
  overflow: visible;
  padding-left: .3rem 2rem;
  box-shadow: 0 2px 4px -1px rgb(0 0 0 / 20%), 0 4px 5px 0 rgb(0 0 0 / 14%), 0 1px 10px 0 rgb(0 0 0 / 12%);
  position: relative;
  z-index: 1000;
}
#layout_sidebar {
  grid-area: sidebar;
  background-color: #092152;
  width: 240px;
}
#layout_main {
  grid-area: main;
  padding: 2em;
}
#layout_footer {
  grid-area: footer;
  background: #CCC;
  padding: 1em;
}
</style>



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
<template>
  <div id="debug">
    <header><strong>DEBUG</strong> - Change Fundraiser State</header>
    <section>
      <main>
        <btn-group>
          <btn
            @click.native="reset" :class="resetCssClass"
            icon="refresh" value="Reset"></btn>
          <btn
            @click.native="start" :class="startCssClass"
            icon="play" value="Start"></btn>
          <btn
            @click.native="cap" :class="capCssClass"
            icon="eye" value="Cap"></btn>
          <btn
            @click.native="end" :class="endCssClass"
            icon="stop" value="End"></btn>
        </btn-group>
      </main>
    </section>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Btn from '@nylira/vue-button'
import BtnGroup from './BtnGroup'
export default {
  name: 'debug',
  components: {
    Btn,
    BtnGroup
  },
  computed: {
    resetCssClass () {
      if (this.debug.status === 0) return 'ni-btn-active'
    },
    startCssClass () {
      if (this.debug.status === 1) return 'ni-btn-active'
    },
    capCssClass () {
      if (this.debug.status === 2) return 'ni-btn-active'
    },
    endCssClass () {
      if (this.debug.status === 3) return 'ni-btn-active'
    },
    ...mapGetters(['debug'])
  },
  methods: {
    reset () {
      this.$store.commit('setDebugFundraiserStatus', 0)
    },
    start () {
      this.$store.commit('setDebugFundraiserStatus', 1)
    },
    cap () {
      this.$store.commit('setDebugFundraiserStatus', 2)
    },
    end () {
      this.$store.commit('setDebugFundraiserStatus', 3)
    }
  }
}
</script>

<style lang="stylus">
@import '../styles/variables.styl'

bc = txt

#debug
  position fixed
  bottom 0
  left 0
  background hsla(0,0,0,0.75)
  width 100vw
  max-width 30rem
  shadow()
  color #fff

  > header
    border-bottom 1px solid bc
    padding 0 0.5rem
    line-height 1.5rem
    font-size 0.75rem
  > section
    padding 0.5rem
    > main
      display flex
      flex-flow column nowrap
      .ni-btn-group:last-of-type
        margin-bottom 0
      .ni-btn
        font-size 0.75rem
      .ni-btn-active .ni-btn
        background link
        border 1px solid lighten(link, 25%)
        color c-app-fg

@media screen and (min-width: 360px)
  #debug > section > main .ni-btn
    font-size 1rem
@media screen and (min-width: 768px)
  #debug > header
    padding 0 1rem
  #debug > section
    padding 1rem
</style>

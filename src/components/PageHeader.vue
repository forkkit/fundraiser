<template>
  <header class="ni-ph">
    <div class="ni-ph-container">
      <div class="ni-ph-title">Cosmos Fundraiser</div>
      <div class="ni-ph-box">
        Issues with the fundraiser? Contact us:
        <a href="mailto:fundraiser@cosmos.network">fundraiser@cosmos.network</a<>
      </div>
      <!--
      <key-values>
        <time-remaining
          type="cap"
          :date="endHiddenDatetime"
          :started="fundraiserStarted">
        </time-remaining>
        <time-remaining
          :date="endDatetime"
          :started="fundraiserStarted">
        </time-remaining>
      </key-values>
      -->
    </div>
  </header>
</template>

<script>
import { mapGetters } from 'vuex'
import cfr from 'cosmos-fundraiser'
import moment from 'moment'
import KeyValues from './KeyValues'
import TimeRemaining from './TimeRemaining'
export default {
  name: 'page-header',
  components: {
    KeyValues,
    TimeRemaining
  },
  computed: {
    endHiddenDatetime () {
      let utcStart = moment.utc(this.config.START_DATETIME)
      let localStart = moment(utcStart).local()
      if (this.fundraiserStarted) {
        return moment(localStart).add(this.config.CAP_START, 'hours')._d
      } else {
        return localStart
      }
    },
    endDatetime () {
      let utcStart = moment.utc(this.config.START_DATETIME)
      let localStart = moment(utcStart).local()
      if (this.fundraiserStarted) {
        return moment(localStart).add(this.config.ENDS_AFTER, 'days')._d
      } else {
        return localStart
      }
    },
    ...mapGetters(['config'])
  },
  data: () => ({
    fundraiserStarted: false
  }),
  methods: {
    watchFundraiserStart () {
      let self = this
      cfr.ethereum.fetchIsActive('', function (err, res) {
        if (err) return
        if (res === 1) self.fundraiserStarted = true
        else self.fundraiserStarted = false
        // console.log('this.fundraiserStarted', self.fundraiserStarted)
      })
    }
  },
  mounted () {
    this.watchFundraiserStart()
    setInterval(() => this.watchFundraiserStart(), 1000)
  }
}
</script>

<style lang="stylus">
@import '../styles/variables.styl'

.ni-ph-container
  max-width 1024px
  margin 0 auto

.ni-ph-title
  font-size 1.5rem
  font-weight 600
  padding 1rem 0.25rem
  border-bottom 1px solid bc

  display flex
  align-items center
  justify-content center

.ni-ph-box
  display flex
  padding 0.5rem 0.25rem 0.25rem
  flex-flow column
  align-items center
  text-align center
  font-size 0.875rem

@media screen and (min-width: 360px)
  .ni-ph-box
    font-size 1rem

@media screen and (min-width: 768px)
  .ni-ph-title
    font-size 2rem
    padding 1.5rem 0.75rem

@media screen and (min-width: 1024px)
  .ni-ph
    margin-top 1rem
    border-top 1px solid bc
    border-bottom 1px solid bc

  .ni-ph-container
    display flex
    flex-flow row nowrap
    padding 0 0.75rem

  .ni-ph-title, .ni-kvs
    flex 0 0 50%

  .ni-ph-title
    justify-content flex-start
    border-bottom none

  .ni-ph-box
    flex 1
    align-items flex-end
    justify-content center
    padding 0 1rem
</style>

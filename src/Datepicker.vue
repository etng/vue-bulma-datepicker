<template>
  <partial :name="name">
    <slot></slot>
  </partial>
</template>

<script>
import Flatpickr from 'flatpickr'

// function Datepicker (selector, config, l10n) {
//   this.l10n = Object.assign({}, Flatpickr.prototype.l10n, l10n)
//   return Flatpickr.call(this, selector, config)
// }

// Datepicker.prototype = Flatpickr.prototype

export default {
  partials: {
    singleInput: require('./singleInput.html'),
    wrapperInput: require('./wrapperInput.html')
  },
  props: {
    alignment: String,
    config: {
      type: Object,
      default: () => ({})
    },
    l10n: {
      type: Object,
      default: () => ({})
    },
    placeholder: {
      type: String,
      default: 'Pick date'
    },
    readonly: Boolean,
    value: [String, Array]
  },

  ready () {
    // console.log('b4 create', this.datepicker)
    // debugger
    this.create()
  },

  beforeDestroy () {
    this.destroy()
  },

  attached () {
    this.create()
  },

  detached () {
    this.destroy()
  },

  methods: {
    create () {
      // console.log(this.$el.nextSibling)
      if (!this.datepicker) {
        this.datepicker = new Flatpickr(this.$el.nextSibling, this.config)
        // this.datepicker = new Datepicker(this.$el.nextSibling, this.config, this.l10n)
      }
      // this.datepicker.set('onOpen',  (d, s, inst) => {
      //   this.$dispatch('open', '', '', '')
      // })
      this.datepicker.set('onChange', (d, s, inst) => {
        // window.cdp = inst
        // console.log("play with cdp first plz", d, s)
        var dates =  s ? s.split(',').map((x) => {
          return x.trim()
        }): []
        console.log('new dates:', dates)
        this.$set('value', dates)
        // this.$dispatch('changed', s)
        this.$dispatch('change', dates)
      })
    },
    destroy () {
      if (this.datepicker && !this.static) {
        this.datepicker.destroy()
        this.datepicker = null
      }
    },

    activate () {
      // console.log('activate dp')
      this.$dispatch('open', this.id)
    }
  },

  computed: {
    wrap () {
      return !!this.config.wrap
    },
    static () {
      return !!this.config.static
    },
    name () {
      return this.wrap ? 'wrapperInput' : 'singleInput'
    }
  }
}
</script>

<style lang="stylus">
$calendar_background = #ffffff
$calendar_border_color = #d3d6db

$months_color = #111
$months_background = transparent

$weekdays_background = transparent

$day_text_color = #222324
$day_hover_background_color = #d3d6db

$today_color = #ed6c63
$selected_day_background = #1fc8db

@import '~flatpickr/src/style/flatpickr'
</style>

<template>
<a class="weui_cell" href="javascript:">
  <div class="weui_cell_bd weui_cell_primary">
    <p>{{title}}</p>
    <inline-desc v-if="inlineDesc">{{inlineDesc}}</inline-desc>
  </div>
  <div class="weui_cell_ft with_arrow vux-datetime-value">{{value}}</div>
</a>
</template>

<script>
import Picker from './datetimepicker'
import Group from '../Group'
import InlineDesc from '../Inline-desc'
import Base from '../../libs/base'

export default {
  mixins: [Base],
  components: {
    Group,
    InlineDesc
  },
  props: {
    format: {
      type: String,
      default: 'YYYY-MM-DD'
    },
    title: {
      type: String,
      required: true
    },
    value: {
      type: String,
      default: '',
      twoWay: true
    },
    inlineDesc: {
      type: String
    },
    placeholder: {
      type: String
    },
    minYear: {
      type: Number
    },
    maxYear: {
      type: Number
    }
  },
  created () {
    this.$dispatch('group.class.add', 'weui_cells_access')
  },
  ready () {
    var _this = this
    const uuid = this.uuid
    this.$el.setAttribute('id', 'vux-datetime-' + uuid)
    let options = {
      trigger: '#vux-datetime-' + uuid,
      format: _this.format,
      value: _this.value,
      output: '.vux-datetime-value',
      onConfirm: function (value) {
        _this.value = value
      }
    }
    if (this.minYear) {
      options.minYear = this.minYear
    }
    if (this.maxYear) {
      options.maxYear = this.maxYear
    }
    this.picker = new Picker(options)
  },
  watch: {
    value: function (val) {
      this.$dispatch('change', val)
    }
  },
  beforeDestroy () {
    this.picker.destroy()
  }
}
</script>

<style>
@import './datetimepicker.css'
</style>
<template>
  <div class="box">
    <button v-on:click="padLeft -= 1"><span>&lt;&lt;</span></button>
    <input type="number" :class="{ active: !!padLeft }" v-model="padLeft">
    <button v-on:click="padLeft += 1"><span>&gt;&gt;</span></button>

    <button v-on:click="localInvert = !localInvert" :class="{'active': localInvert}"><span>~</span></button>

    <div class="bit-rows">
      <div class="bits" v-for="bits in bitsText" :key="bits" v-html="bits"/>
    </div>

    <button v-on:click="padRight -= 1"><span>&lt;&lt;</span></button>
    <input type="number" :class="{ active: !!padRight }" v-model="padRight">
    <button v-on:click="padRight += 1"><span>&gt;&gt;</span></button>

    <span class="comments" v-if="comments" v-html="commentText"/>
  </div>
</template>

<script>
import BitString from '../bitstring'

export default {
  name: 'BitBox',
  props: {
    code: String,
    fmts: String,
    shift: Number,
    invert: Boolean,
    comments: Boolean,
  },
  data: function () {
    return {
      padLeft: 0,
      padRight: 0,
      localInvert: false,
    }
  },
  computed: {
    bitsText: function () {
      return this.fmts
        .split('\n')
        .map((fmt) => 
          new BitString(this.code)
          .invert(this.invert)
          .invert(this.localInvert)
          .shiftRight(this.shift)
          .padLeft(this.padLeft)
          .padRight(this.padRight)
          .toFormat(fmt)
        )
    },
    commentText: function () {
      return (new BitString(this.code)).comments
    },
  },
}
</script>

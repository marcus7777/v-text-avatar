<template>
  <span :style="`width:${size}px;border-radius: 50%;background-color: ${bg};text-align: center;height:${size}px;line-height:${size}px;color:${fg}`">{{init}}</span>
</template>
<script>
import Please from "PleaseJS"
export default {
  name: "textAvatar",
  computed:{
    init() {
      let initials = this.name.match(/\b\w/g) || [];
      return ((initials.shift() || '') + (initials.pop() || '')).toUpperCase();
    },
    bg() {
      return Please.make_color({from_hash: this.name+this.email})
    },
    fg(){
      let bg = this.bg

      let c = bg.substring(1);     // strip #
      let rgb = parseInt(c, 16);   // convert rrggbb to decimal
      let r = (rgb >> 16) & 0xff;  // extract red
      let g = (rgb >>  8) & 0xff;  // extract green
      let b = (rgb >>  0) & 0xff;  // extract blue

      let luma = 0.2126 * r + 0.7152 * g + 0.0722 * b; // per ITU-R BT.709

      if (luma > 128) {
        return "#000"
      } else {
        return "#FFF"
      }
    }
  },
  props:{
    size:{
      default: 36,
      type: Number,
    },
    email:{
      default:"",
      type: String,
    },
    name:{
      default:"Marcus",
      type: String,
    }
  },
}
</script>

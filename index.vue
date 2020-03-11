<template>
  <span :style="`display:inline-block;width:${cssSize};border-radius:50%;background-color:${bg};text-align:center;height:${cssSize};line-height:${cssSize};color:${fg}`">{{init}}</span>
</template>
<script>
import "PleaseJS"
function name (name) {
  const match = /(mr|ms|miss|mrs|mx|dr|sir|prof|lady|lord)(\.?)\s/i.exec(name)
  let n = name
  if (match !== null) { 
    n = name.replace(match[0], "")
  }
  return n
}
export default {
  name: "textAvatar",
  computed:{
    cssSize(){
      return this.size + "px"
    },
    init() {
      let initials = name(this.name).match(/\b\w/g) || [];
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
      default: 48,
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

<template>
  <span :style="`display:inline-block;width:${cssSize};border-radius:50%;background-color:${bg};text-align:center;height:${cssSize};line-height:${cssSize};color:${fg}`">{{init}}</span>
</template>
<script>
import "PleaseJS"
function name (name, email) {
  if (!name && email) {
    name = email.substring(0, email.lastIndexOf("@")) + ""
    if (name.length == 2) {
      name = name[0] + " " + name[1]
    } else {
      name = name.replace("."," ")
        // insert a space between lower & upper
    	.replace(/([a-z])([A-Z])/g, '$1 $2')
	// space before last upper in a sequence followed by lower
	.replace(/\b([A-Z]+)([A-Z])([a-z])/, '$1 $2$3')
	// uppercase the first character
	.replace(/^./, function(str){ return str.toUpperCase()})
    }
  }
  const match = /(mr|ms|miss|mrs|mx|dr|sir|prof|lady|lord)(\.?)\s/i.exec(name)
  let n = name+""
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
    init() {:
      let initials = name(this.name, this.email).match(/\b\w/g) || [];
      if (this.title) {
        initials = this.name.match(/\b\w/g) || [];
      }
      return ((initials.shift() || '') + (initials.pop() || '')).toUpperCase();
    },
    bg() {
      return this.color || Please.make_color({from_hash: this.name+this.email})
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
    color: {
      type:String,
      default: "",
    },
    size:{
      default: 48,
      validator: function (value) {
        return +value !== 0
      },
    },
    email:{
      default: "",
      type: String,
    },
    name:{
      type: String,
    },
    title: Boolean,
  },
}
</script>

<template>
  <div class="hello scroll">
    <div class="title">{{text}}</div>
    <div class="shadow circle" 
      :style="{width: shadowRadius+'vw', height: shadowRadius+'vw'}">
    </div>
    <div class="face circle"></div>
    <div class="eyes" 
      :style="{height: eyeHeight+'vw'}">
      <div class="left eye">
        <div class="tear" :style="{opacity: tearOpacity}"></div>
        <div class="eyebrow first" :style="{opacity: browOpacity}"></div>
      </div>
      <div class="right eye">
        <div class="tear" :style="{opacity: tearOpacity}"></div>
        <div class="eyebrow" :style="{opacity: browOpacity}"></div>
      </div>
    </div>
    <div :class="mouthClass" 
      :style="{width: mouthWidth+'px', height: mouthHeight+'px', top:mouthTop+'%'}">
    </div>
   </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data(){
    return {
      texts: ['As good as it gets', 'Pretty good', 'Okay I guess...', 'Not great...', 'Do not even ask...'],
      text: 'Pretty good',
      eyeHeight: 10,
    }
  },
  mounted(){
    let lastY=null
    let overscroll = function(el) {
      el.addEventListener('touchstart', function() {
        var top = el.scrollTop
          , totalScroll = el.scrollHeight
          , currentScroll = top + el.offsetHeight;
        //If we're at the top or the bottom of the containers
        //scroll, push up or down one pixel.
        //
        //this prevents the scroll from "passing through" to
        //the body.
        if(top === 0) {
          el.scrollTop = 1;
        } else if(currentScroll === totalScroll) {
          el.scrollTop = top - 1;
        }
      });
      el.addEventListener('touchmove', function(evt) {
        //if the content is actually scrollable, i.e. the content is long enough
        //that scrolling can occur
        if(el.offsetHeight < el.scrollHeight)
          evt._isScroller = true;
      });
    }
    overscroll(document.querySelector('.scroll'));
    document.body.addEventListener('touchmove', (e)=> {
      //In this case, the default behavior is scrolling the body, which
      //would result in an overflow. Since we don't want that, we preventDefault.
      if(!e._isScroller) {
        e.preventDefault();
      }
      let target = e.touches[0]
      let currentY = target.screenY;
      let dt = this.texts[4-Math.ceil(this.eyeHeight/4)]
      if (dt) {
        this.text = dt
      }
      if(currentY >lastY){
          // moved down
          if(this.eyeHeight>0) {
            this.eyeHeight-=0.5
          }
      }
      else if(currentY <lastY){
          // moved up
          if(this.eyeHeight<20){
            this.eyeHeight+=0.5
          }
      }
      lastY = currentY;
    }, {passive: false});
  },
  computed: {
    shadowRadius(){
      return 3 * (this.eyeHeight-10) + 20
    },
    mouthHeight(){ // 10 - 30
      let h = this.h
      if ( h < 19 ){
        
        if (h < 16) {
          if (h < 14) return h * 0.9
          return h * 0.7
        }
        return h * 0.5
      }
      if (h < 21) return h*0.7
      if (h < 23) return h*0.9
      return h
    },
    mouthWidth(){
      return 0.8 * (this.eyeHeight-10) + 40
    },
    mouthTop() {
      return 53 - ((this.eyeHeight-10)/4)
    },
    h() {
      return ((this.eyeHeight-10) * 2 / 3) + 20
    },
    sad() {
      if (this.h<=16) return true
      return false
    },
    normal() {
      if (this.h < 19 && this.h > 16) return true
      return false
    },
    mouthClass() {
      return {
        mouth: true, 
        sad: this.sad,
        normal: this.normal,
      }
    },
    tearOpacity() {
      if (this.sad) {
        if (this.eyeHeight > 2) return 0.2
        if (this.eyeHeight > 0) return 0.4
        if (this.eyeHeight > -2) return 0.6
      }
      return 0
    },
    browOpacity() {
      if (this.eyeHeight > 15) return 0
      if (this.eyeHeight > 10) return 0.2
      if (this.eyeHeight > 5) return 0.4
      if (this.eyeHeight > 0) return 0.6
      return 0.8
    }
  },
}
</script>

<style>
.title{
  top: 30vw;
  font-size: 7vw;
  position: absolute;
  color: white;
  font-family:Verdana, Geneva, Tahoma, sans-serif;
}
.hello{
  position:relative;
  font-size: 25px;
  color: #6745;
  background: linear-gradient(#e66465, #9198e5);
  width: 100%;
  height: 100%;
  position: absolute;
  display: flex;
    justify-content: center;
    align-items: center;
}
.face{
  border-radius: 50%;
  background-color:  #ffdd40;
  width:30vw;
  height: 30vw;
  z-index: 2
}
.shadow{
  z-index: 1;
  border-radius: 50%;
  background: radial-gradient( rgba(255,255,255,0.1), rgba(255,255,255,0.8));
}
.eyes{
  position: absolute;
  left:50%;
  transform: translate(-50%);
  width:20vw;
  z-index: 3;
  display: flex;
  justify-content: space-around;
}
.eye{
  width: 3vw;
  border-radius: 50%;
  height: 3vw;
  background: black;
  border: 2vw solid white;
  position: relative;
}
.eye .tear {
  position: absolute;
  content: '';
  width: 6vw;
  height: 1.5vw;
  background: rgb(151, 191, 252);
  border-radius: 5px 5px 100% 100%;
  margin-bottom: 2px; 
  left: -1.5vw;
  bottom: -2.5vw;
}
.eye .eyebrow {
  position: absolute;
  content: '';
  width: 7vw;
  height: 0.7vw;
  background: rgb(99, 55, 14);
  border-radius:  100% 100% 5px 5px;
  margin-bottom: 2px; 
  transform:rotate(10deg);
  top: -4vw;
  left: -1vw;
}
.eye .eyebrow.first {
  transform:rotate(-10deg);
  left: -3vw;
}
.mouth {
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0%);
  background: #884E2C;
  border-radius: 15px 15px 70px 70px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  z-index: 3;
}
.mouth:after {
  display: block;
  content: '';
  width: 20px;
  height: 8px;
  background: #CD5B4D;
  border-radius: 5px 5px 100% 100%;
  margin-bottom: 2px;
}
.mouth.normal{
  border-radius: 10px 10px 10px 10px;
}
.mouth.normal:after{
  display: none;
}
.mouth.sad{
  border-radius: 70px 70px 10px 10px;
}
.mouth.sad:after{
  width:10px;
  height: 2px;
  border-radius: 100% 100% 5px 5px;
}
.circle{
  position: absolute;
  left:50%;
  transform: translate(-50%)
}
.a{
  padding: 10px 200px 30px 100px;
  background-color: #777;
  color: blanchedalmond;
  width: 800px;
  height: 500px;
}
.b{
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #25bba3;
}
img{
  padding: 10px 200px 30px 100px;
}
</style>

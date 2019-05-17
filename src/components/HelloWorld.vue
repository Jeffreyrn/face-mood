<template>
  <div class="hello scroll">
    <div class="title">{{text}}</div>
    <div class="shadow circle" 
      :style="{width: shadowRadius+'vw', height: shadowRadius+'vw'}">
    </div>
    <div class="face circle"></div>
    <div class="eyes" 
      :style="{height: eyeHeight+'vw'}">
      <div class="left eye"></div>
      <div class="right eye"></div>
    </div>
    <div class="mouth"
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
      shadowRadius: 30,
      text: 'Okay I guess...',
      eyeHeight: 9,
      mouthHeight: 20,
      mouthWidth: 40,
      mouthTop: 53,
    }
  },
  mounted(){
    var lastY=null
    var overscroll = function(el) {
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
  //would result in an overflow.  Since we don't want that, we preventDefault.
  if(!e._isScroller) {
    e.preventDefault();
  }
  let target = e.touches[0]
      var currentY = target.screenY;
      let dt = this.texts[5-Math.ceil(this.eyeHeight/4)]
      if (dt) {
        this.text = dt
      }
      if(currentY >lastY){
          // moved down
          if(this.shadowRadius>5){
            this.shadowRadius-=1
          }
          if(this.eyeHeight>0){
            this.eyeHeight-=1
          }
          if(this.mouthHeight > 10){
            this.mouthHeight-=1
          }
          if(this.mouthWidth > 20){
            this.mouthWidth-=1
          }
          if(this.mouthTop < 56){
            this.mouthTop+=0.4
          }
      }else if(currentY <lastY){
          // moved up
          if(this.shadowRadius<50){
            this.shadowRadius+=1
          }
          if(this.eyeHeight<20){
            this.eyeHeight+=1
          }
          if(this.mouthHeight < 20){
            this.mouthHeight+=1
          }
          if(this.mouthWidth < 40){
            this.mouthWidth+=1
          }
          if(this.mouthTop > 50){
            this.mouthTop-=0.4
          }
      }
      lastY = currentY;
}, {passive: false});
    
  }
}
</script>

<style>
.title{
  top: 10vw;
    font-size: 7vw;
    position: absolute;
    color: white;
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

<template>
  <div class="wrapper">

    <div class="quetion-wrapper">
          <div class= "text-question" ref="question" contenteditable="true"
          
           ></div>

          <div class="button-wrapper">
                  <button class="button-load-Image"
                  v-on:mouseover="animateMouseOver"
                  v-on:click="addImage">
                  </button>
                  <button 
                  v-on:click="answerShow();animateRotate($event)" 
                  v-on:mouseover="animateMouseOver"
                  class = "button-answer-show">
                  </button>
                  <button 
                  v-on:click="$emit('remove')"
                  class = "delete-button"
                  v-on:mouseover="animateMouseOver">
                  </button>
                  <button class="button-font-size-incr"
                  v-on:mouseover="animateMouseOver"
                  @mousedown="fontSizeIncr"
                  @mouseup="mouseup=true;"
                  ></button>
                  <button class="button-font-size-decr"
                  v-on:mouseover="animateMouseOver"
                  @mousedown ="fontSizeDecr"
                  @mouseup="mouseup=true;"
                  ></button>
          </div>
    </div>

    <div v-on:keyup.delete="del=!del" ref="answer" contenteditable="true" class= "text-answer"
    v-show="answerVisible"
    v-bind:style="{ fontSize:fontSize+'px'}">
    {{text}}
        <LoadImage v-on:delete-image="deleteImage" ref="abs" v-for="image in images" 
        v-bind:key="image.id"
        v-bind:ev ="del"
        v-bind:id = "image.id">
        </LoadImage>
    </div>      
  </div>
</template>
  <script>
  import LoadImage from './LoadImage'

  export default {
      components: { LoadImage },
      props: {
        imageId: Number,
        text: String
      },
data() {
  return {
    images: [
    ],
    imageCounter: 0,
    mouseup: false,
    fontSize: 15,
    answerVisible: false,
    del: false,
    textQ: '',
    answerShowDeg: 90,
  }
},
     methods: {
       answerShow() {
          this.answerVisible=!this.answerVisible;  
       },
       fontSizeIncr() {
         var $this = this;
         this.mouseup=false; 
         if(this.mouseup ===false){
           var increaseFontSize = setInterval(function(){  
                  $this.fontSize = parseInt($this.fontSize) +2;
                  if($this.mouseup === true) {
                    clearInterval(increaseFontSize);            
                  }
           }, 100)
         }
       },
       fontSizeDecr() {
         var $this = this;
         this.mouseup=false; 
         if(this.mouseup ===false){
           var increaseFontSize = setInterval(function(){ 
                  $this.fontSize = $this.fontSize - 2;
                  if($this.mouseup === true) {
                       clearInterval(increaseFontSize);            
                  }
           }, 100)
         }
       },
       addImage() {
        this.imageCounter++;
        this.images.push({id: this.imageCounter,
        title: this.index});
      },
      deleteImage(imageId) {
           this.images.splice(imageId,1);
      },      
      animateMouseOver(event) {
        var hoverAnimate = new TimelineMax();
        hoverAnimate.to(event.srcElement, 0.2, {scale:"1.2"});
        hoverAnimate.to(event.srcElement, 0.2, {scale:"1"});
      },
      animateRotate(event) {
        this.answerShowDeg += 90;
        TweenMax.to(event.srcElement, 0.2, {rotation: this.answerShowDeg }); 
      }
  },
  watch: {
          e: function() {
             this.fontSize = parseInt(this.a);
       }

  }
  }
  </script>
<style>
  .quetion-wrapper {
    display: flex;
    flex-direction: row;
    border: 1px solid grey;
  }
  .button-wrapper {
    
    position: relative;
    margin-left: auto;
    width: 225px;
  }
  .button-load-Image {
    position: absolute;
    background: url(../../img/addImage.svg) no-repeat;
    border: none;
    top: 15px;
    right: 160px;
    width: 40px;
    height: 40px;
    cursor: pointer;
  }
  .button-font-size-incr {
    position: absolute;
    width: 30px;
    height: 30px;
    top: 15px;
    right: 80px;
    background: url(../../img/arrow.png) no-repeat;
    background-size: contain;
    border: none;
    cursor: pointer;
  }
  .button-font-size-decr {
    position: absolute;
    width: 30px;
    height: 30px;
    top: 15px;
    right: 120px;
    background: url(../../img/arrow.png) no-repeat;
    background-size: contain;
    border: none;
    transform: rotate(180deg);
    cursor: pointer;
  }
  .delete-button {
    border: none;
    position: absolute;
    top: 15px;
    right: 40px;
    background: url(../../img/delete.png) no-repeat;
    background-size: contain;
    background-position: center center;
    height: 30px;
    width: 30px;
    cursor: pointer;
  }
  .text-answer {
    min-height: 100px;
    width: 100%;
    resize: vertical;
  }

  .text-answer *:not(input) {
    font-size: inherit !important; 
  }
  .text-question {
    border: none;
    min-height: 70px;
    font-size: 30px;
    width: 85vw;
    padding: 20px 0;
    overflow: hidden;
    resize: none;
  }
  .button-answer-show {
    position: absolute;
    top: 23px;
    right: 10px;
    height: 20px;
    width: 20px;
    border: none;
    background: url(../../img/show.png) no-repeat;
    background-size: contain;
    background-position: center center;
    cursor: pointer;
    transform: rotate(90deg);
  }
  .wrapper {
    display: flex;
    flex-direction: column;
  }

@media (max-width: 850px) {
  .text-question {width: 70vw;}
}

@media (max-width: 600px) {
  .text-question {width: 60vw;}
}
</style>

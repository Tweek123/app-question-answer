<template>
  <div id="wrapper">
    <main>
        <div class="menu">
          <input class="animation new-font-size" type="text"
          v-model="allFontSize"
          v-on:mouseover="animateMouseOver">

          <button class="animation set-font-size"
           v-on:click="setAllFont=!setAllFont"
           v-on:mouseover="animateMouseOver">
          </button>

          <div class="animation load-json-file">
              <input class="load-input" 
              v-on:mouseover="anMousOvIn" 
              type="file" 
              @change="handleFileSelect">

          </div>

          <input  class ="animation-input input-json-file"
          v-model="nameJson">

          <button class="animation animateMouseOver save-json-file"
          v-on:click="exportToJsonFile"
          v-on:mouseover="animateMouseOver">
          </button>

          <div class="headline">
              question-answer
          </div>
        </div>
        <Question v-on:remove="deleteQuestion(index)" 
        ref="questionRef"
        v-for="(question, index) in questions"
        v-bind:key="question.id"
        v-bind:text="question.text"
        v-bind:a="allFontSize"
        v-bind:e="setAllFont"
        v-bind:textQ="question.question"
        :index="index"
        class=question>
        </Question>
        <button 
        v-on:click="addQuestion($event);"
        v-on:mouseover="animateMouseOver"
        class=newquestion>
        </button>
    </main>
  </div>
</template>
<script>

import Question from './LandingPage/Question'
import {TweenMax, Power2, TimelineLite} from "../gsap/TweenMax";

  export default {
    mounted() {
              var loadAnimate = new TimelineMax();
              //chain all to() methods together on one line              
              //we recommend breaking each to() onto its own line for legibility
              loadAnimate.from(".animation", 1, {y:"-100"});
              loadAnimate.to(".animation", 0.2, {rotation:"-40deg"});
              loadAnimate.to(".animation", 0.2, {rotation:"40deg"});
              loadAnimate.to(".animation", 0.2, {rotation:"0deg"});
              TweenMax.from(".animation-input",1,{width:"0", opacity:"0"});
              console.log("question.question\n");
              console.log(this.question);
    }, 
    name: 'landing-page',
    components: { Question,TweenMax,Power2,TimelineLite },
    data() {
      return {
      nameJson: 'name',
      allFontSize: 15,
      questions: [
    ],
        questionCounter: 0,
        text: '',
        setAllFont: false,
        jsondata: '',
        addButtonDeg: 0,
      }
    }, 
    methods: {
      addQuestion() {
        this.addButtonDeg -= 270;
        this.questionCounter++;
        this.questions.push({id: this.questionCounter
        });
        TweenMax.to(".newquestion",1.1,{rotation: this.addButtonDeg});
        setTimeout(() => {
          this.animateOnAdd();  
        }, 1);
      },
      deleteQuestion(i) {
        this.questions.splice(i,1)
    },
    exportToJsonFile() {
            let $this = this;
            this.questions.forEach(function(element,index) {
                element.html =  $this.$refs.questionRef[index].$refs.answer.innerHTML;
                element.question =  $this.$refs.questionRef[index].$refs.question.innerHTML;
            });
            let dataStr = JSON.stringify(this.questions);
            let dataUri = 'data:application/json;charset=utf-8,'+encodeURIComponent(dataStr);
            let exportFileDefaultName = this.nameJson;
            let linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click();
    }, 
    handleFileSelect(event) {
                  
                  var files = event.target.files; // FileList object
                  var json;
                  var er = false;
	                // files is a FileList of File objects. List some properties.
	                var output = [];
	                for (var i = 0, f; f = files[i]; i++) {
	                	var reader = new FileReader();
                
	                	// Closure to capture the file information.
	                	reader.onload = (function (theFile) {
	                		return function (e) {
	                			try {
                          
                          json = JSON.parse(e.target.result);
                          json = [...json];
	                			} catch (ex) {
                          alert('ERROR = ' + ex);
                          er = true;
	                			}
	                		}
	                	})(f);
	                	reader.readAsText(f);
                  }
                  let $this = this;
                  this.questions = [];
                  this.questionCounter = 0;
                  
                   setTimeout(() => {
                      this.jsondata = json;
                      this.jsondata.forEach(function(element,index) {
                                $this.questions.push({id: element.id,
                                question: element.question 
                                });
                                $this.questionCounter++;
                                
                      });
                      setTimeout(() => {
                            this.jsondata.forEach(function(element,index) { {

                          $this.$refs.questionRef[index].$refs.answer.innerHTML = element.html;
                          $this.$refs.questionRef[index].$refs.question.innerHTML = element.question;
                          TweenMax.to($this.$refs.questionRef[index].$el,1.5,{display: "block"});   
                          if(index%2===0) {
                                TweenMax.from($this.$refs.questionRef[index].$el,1.5,{x:1000,opacity:0.2}); 
                          } else {
                                TweenMax.from($this.$refs.questionRef[index].$el,1.5,{x:-1000,opacity:0.2});
                                } 
                              }
                            });
                      }, 100);
                  }, 100);
                },
                anMousOvIn(event) {
                  TweenMax.to(event.path[1], 0.2, {scale:"1.2"});
                  TweenMax.to(event.path[1], 0.2, {scale:"1", delay: 0.2});
                },
                animateMouseOver(event) {
                  var hoverAnimate = new TimelineMax();
                  hoverAnimate.to(event.srcElement, 0.2, {scale:"1.2"});
                  hoverAnimate.to(event.srcElement, 0.2, {scale:"1"});
                },
                animateOnAdd() {
                   TweenMax.from(this.$refs.questionRef[this.questions.length-1].$el,1.5,{x:1000,opacity:0.2}); 
                   TweenMax.to(this.$refs.questionRef[this.questions.length-1].$el,1.5,{display: "block"});  
                }
      }
    }
    var counter = 0; 
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    outline: none;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  *::selection {
  background: rgb(245,245,245); /* Safari */
  }
  body { font-family: 'Source Sans Pro', sans-serif;     background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
  }
  @media (max-width: 900px) {
      .headline {
        display: none;
      }
  }
  #wrapper {
    height: 100vh;
    width: 100%;
    overflow-x: hidden;
  }
  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding-bottom: 20px;
  }

  main > div { flex-basis: 50%; }

  .left-side {
    display: flex;
    flex-direction: column;
  }
  .menu {
    display: flex;
    flex-direction: line;
    align-items: center;
    width: 100%;
    height: 60px;
    padding-left: 30px;
    border: solid 2px black;
  }

  .headline {
    margin: auto;
    margin-top: 0;
    font-size: 40px;
  }
  .set-font-size {
    background: url(../img/check.png) bottom center no-repeat;
    background-size: cover;
    border: none;
    margin-left: 20px;
    margin-right: 20px;
    width: 36px;
    height: 36px;
    cursor: pointer;
  }
  .new-font-size {
    border: solid black 2px;
    text-align: center;
    font-size: 18px;
    width: 35px;
    height: 35px;
    border-radius: 10px;
  }

  input::placeholder {
  font-size: 15px;
  color: white;
}
  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc p {
    color: black;
    margin-bottom: 10px;
  }

  .doc button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
   .question {
    display: none;
    margin-top: -2px;
    position: relative;
    width: 100%;
    border: 2px solid black;
    background: white;
  }
  .newquestion {
        background: url(../img/add.png) no-repeat;
        cursor: pointer;
        background-position: center center;
        border: none;
        margin: auto;
        margin-top: 40px;
        width: 40px;
        height: 40px;
        background-size: contain;
  }
  .input-json-file {
    padding-left: 10px;
    height: 30px;
  }
  .save-json-file {
    background: url(../img/save.png) no-repeat;
    background-size: cover;
    width: 55px;
    height: 55px;
    border: none;
    cursor: pointer;
  }
  .load-json-file {
    background: url(../img/download.png) no-repeat;
    background-size: cover;
    width: 30px;
    height: 30px;
    margin-right: 30px;
    border: none;  
    cursor: pointer;
  }
  .load-input {
    height: 100%;
    width: 100%;
    z-index: 2;
    opacity: 0;
    cursor: pointer;
    font-size: 0;
    position: absolute;
  }
</style>

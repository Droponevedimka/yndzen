<template lang="pug">  

    section(v-bind:class="`test test-${test.id}`")        
        div(v-show="!isOpen" class="test__left test-content" :class="test.backBlack && 'test__white'")
          div(class="test__left-number")
            img(src="/assets/img/other/numberIco.png" alt="Номер теста" class="test__left-number-img")
            p(class="test__left-number-text") {{test.id}}
          div(class="test__left-block")           
            p(class="test__left-block-title" v-html="test.ask")
            div(class="test__left-block-answers")                
              div(class="test__left-block-answers-item" v-for="(item, index) in test.answers" :class="[`answer-${index+1}`, test.customColorTitle && (test.backBlackTitle ? 'isBlack': 'isWhite'), !test.backBlack && 'backWhite']" v-bind:dataId="item.answerId" ref="answer-item" :istrue="item.isTrue" @click="visModal") 
                p(class="test__left-block-answers-item-text")
                  svg(width="18" height="17" viewBox="0 0 18 17" fill="white" xmlns="http://www.w3.org/2000/svg")
                    circle(cx="9" cy="8.5" r="8.5" )                  
                  span(v-html="item.item")
            
        div(v-show="!isOpen" class="test__right test-content")
          div(class="test__right-number" :class="test.backBlack && 'isWhite'") {{test.id}}
          img(v-lazy="test.image + '.jpg'" id="rightImg" ref="rightImg" class="test__right-img")
          img(v-lazy="test.image + '-mob.jpg'" id="rightImg" ref="rightImg" class="test__right-img mob")


        vue-page-transition(name="modal")
          div(v-if="isOpen" class="overlay")
            div(class="modal")
              div(class="modal__left")
                div()
                  p(class="modal__left-role" v-html="test.tiketRole")
                  p(class="modal__left-name") {{test.tiketName}}:
                  p(class="modal__left-text" v-html="isOtherText ? toText(test.tiketOther) : toText(test.tiketCorrect) " )      
                a(class="modal__left-next" @click="isClickedNow") {{test.btnFinally ? 'Результат' : 'Дальше'}}    
              div(class="modal__right")  
                img(class="modal__right-img" v-lazy="test.tiketImg")                   
</template>
                        
<script>
export default {
  name: 'test',
  data(){
      return {
          isOpen: false,
          isTimer: null,
          isOtherText: true
      }
  },
  methods: {     
      isClickedNow(e){
        document.body.classList.remove('html__hidden')     
        this.isOpen = false
        let nextId = Number(this.id)+1
        this.$refs.rightImg.src = ''
        if(!this.test.btnFinally){
                    this.isOpen = false
                    this.$router.push({path: `/test/${nextId}/${this.getPublicUtm}`}) 
               } else {          
                    fbq('track', 'CompleteRegistration');          
                    let gg = globalNameOfItog.indexOf(Math.max.apply(Math, globalNameOfItog))+1  
                    this.$router.push({path: `/final/${gg}/${this.getPublicUtm}`})                                    
                    this.isOpen = false 
               }        
      },
      visModal(e){   
        document.body.classList.add('html__hidden')
        globalNameOfItog[Number(e.currentTarget.getAttribute('dataId'))-1]++
        
        if(e.currentTarget.getAttribute('istrue')) this.isOtherText = false
         else this.isOtherText = true
        this.isOpen = true
        ym(61131880,'reachGoal',`question${this.id}`);
      },
      isClicked(e){
          globalNameOfItog[Number(e.currentTarget.getAttribute('dataId'))-1]++
          
          this.isOpen = true    
      },
      initBack(){    
        if(this.test.backBlack) document.body.style.background = '#000000';
            else document.body.style.background = '#ffffff';
        window.scrollTo(0, 0)
      },
      toText(e){
        return e + this.test.tiketTitle
      }
  },
  computed: {
    id(){           
       return this.$store.state.route.params.id
    },
    test(){
      return this.$store.getters.getTestById(this.id)
    },
    getPublicUtm(){
      return window.globalUtm
    }
  },
  mounted() {
    this.initBack()      
    
    
  },
  beforeUpdate() {
    this.initBack()   
  },
  updated(){
    
  }
}
</script>

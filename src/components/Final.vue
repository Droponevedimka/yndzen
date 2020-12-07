<template lang="pug">
    section(class="final")
        div(class="final__left" :class="final.backBlack && 'final-white'")
          div(class="final__left-block")                
                p(class="final__left-block-p") Ты —
                p(class="final__left-block-title") {{final.title}}
                p(class="final__left-block-desc")
                  span(v-html="final.desc") 
                  a(class="final__left-block-desc-link" target="_blank" :href="final.link" @click="clickToLink") здесь <br><br>
                  span() Поделись результатом в&nbsp;соцсетях и&nbsp;получи возможность&nbsp;<a class="final__left-block-desc-link" href="/rules/" target="_blank" @click="clickToLinkRule">выиграть</a> подписку на&nbsp;Яндекс.Плюс.
                div(class="final__left-block-socials")
                    a(@click="clickToSoc(0)" target="_blank" v-bind:href="`https://www.facebook.com/sharer.php?u=${publicPath}/share-${final.id}`" :class="final.backBlack && 'black'")
                        <i class="fab fa-facebook-f final__left-block-socials-item"></i>
                        // img(src="/assets/img/other/fb.svg" class="final__left-block-socials-item")                        
                    a(@click="clickToSoc(1)" target="_blank" v-bind:href="`https://vk.com/share.php?url=${publicPath}/share-${final.id}&title=${getText(final.id)}&image=${publicPath}/assets/img/share/r${this.id}.jpg&noparse=true`" :class="final.backBlack && 'black'") 
                        <i class="fab fa-vk final__left-block-socials-item"></i>
                        // img(src="/assets/img/other/vk.svg" class="final__left-block-socials-item")
                    a(@click="clickToSoc(2)" target="_blank" v-bind:href="`https://telegram.me/share/url?url=${publicPath}/share-square&text=${getText(final.id)}`"  :class="final.backBlack && 'black'") 
                        <i class="fab fa-telegram-plane final__left-block-socials-item"></i>
                        // img(src="/assets/img/other/telg.svg" class="final__left-block-socials-item")                    
        div(class="final__right")
          img(v-lazy="final.img" class="final__right-img")
       
          
</template>


<script>
export default {
  name: 'Final',   
  methods:{    
    clickToLink(){
      ym(61131880,'reachGoal','zenadv');
    },
    clickToLinkRule(){
      ym(61131880,'reachGoal','rule');
    },
    initBack(){      
        if(this.final.backBlack) document.body.style.background = '#000000';
            else document.body.style.background = '#ffffff';
      },
    getText(e){
      switch(e){
        case 1:
          return 'Я — Cверхзвуковой таргетолог'
        break;
        case 2:
          return "Я — Навигатор звездного контента"
        break;
        case 3:
          return "Я — Генератор взрывных креативов"
        break;
        case 4:
          return "Я — Джедай межпланетных кампаний"
        break;
      }
    },
    clickToSoc(e){
      switch(e){
        case 0:
          ym(61131880,'reachGoal','fb-icon');
        break;
        case 1:
          ym(61131880,'reachGoal','vk-icon');
        break;
        case 2:
          ym(61131880,'reachGoal','tg-icon');
        break;
      }
      fbq('track', 'Lead');
    }
  },
  computed: {    
    getPublicUtm(){
      return window.globalUtm
    },
    id(){
      return this.$store.state.route.params.id
    },
    publicPath(){
      return window.location.origin
    },
    final(){
      return this.$store.getters.getFinallyById(this.id)
    }    
  },
  mounted() {
    this.initBack()
    },
    beforeUpdate() {
      this.initBack()
    }
};
</script>
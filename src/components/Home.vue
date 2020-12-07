<template lang="pug">
    section(class="home")
        div(class="home-left home__content")
          div(class="home-left-width")      
            div(class="home-left-width__block")                 
              div(class="home-left-title") Тест по рекламе будущего от 
                a(href="https://zen.yandex.ru/adv?utm_source=space&utm_medium=test&utm_campaign=1year" target="_blank") Яндекс. Дзена
              div(class="home-left-name") Юра, мы всё<br>написали
              div(class="home-left-text") Добро пожаловать в будущее!<br>Здесь&nbsp;реклама&nbsp;охватывает галактики&nbsp;под&nbsp;управлением Звездных&nbsp;Маркетологов.<br><br>Сможешь стать одним из них?<br>Сейчас проверим!
            router-link(v-bind:to="`/test/1${getPublicUtm}`" class="home-link") 
              p(class="home-link-text" @click="goToShare") Поехали 

        div(class="home-right home__content")
          img( v-lazy="`/assets/img/other/start.png`" class="home-right-img")
</template>


<script>
export default {
  name: 'Home',
  beforeMount(){ 
          
  },
  computed: {    
    getPublicUtm(){
      return window.globalUtm
    }
  },
  methods:{    
    goToShare(){
      ym(61131880,'reachGoal','start');
      fbq('track', 'StartTrial');
    }
  },
  mounted(){
    document.body.style.background = '#ffffff';
  }
}

</script>

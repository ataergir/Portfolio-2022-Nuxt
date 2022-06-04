<template>
<div>
  <section class="grid-container">
    <div class="loading-bg">
    <div class="loading-text">33%</div>
    </div>

    <div class="name">
      <h1>
        <span class="name-letter-0">[</span>
        <span class="name-letter-1">A</span>
        <span class="name-letter-2">t</span>
        <span class="name-letter-3">a</span>
        <span>&nbsp;</span>
        <span class="name-letter-4">E</span>
        <span class="name-letter-5">r</span>
        <span class="name-letter-6">g</span>
        <span class="name-letter-7">i</span>
        <span class="name-letter-8">r</span>
        <span class="name-letter-9">]</span>
      </h1>
    </div>

    <div class="services opacity-fade-in translate-in">
      <ul>
        <li class="service service-0">Web Design</li>
        <li class="service service-1">Web Development</li>
        <li class="service service-2">Art Direction</li>
        <li class="service service-3">Graphic Design</li>
        <li class="service service-4">Branding</li>
        <li class="service service-5">Design Research</li>
      </ul>
    </div>

    <ProjectsList class="projects translate-in-slow" />

    <div class="contacts opacity-fade-in translate-in">
      <ul>
        <li class="contact-0 contact">aergir@proton.me</li>
        <li class="contact-1 contact">twitter.com/ataergir</li>
      </ul>
    </div>

    <Three class="threejs-container"/>
  </section>
</div>
</template>

<script>
import { gsap } from "gsap"
export default {
mounted(){
  setTimeout(()=>{
      const tlLoad = gsap.timeline({})
      tlLoad.to('.loading-bg',{yPercent:-100, duration:3})
        .to('.loading-bg',{xPercent:1000, duration:0, ease:'none'})
        .to('.name h1',{fontSize:"5rem", duration:2, ease:'Power2.easeInOut',
        onComplete:function(){
          document.querySelector('.name').classList.add('name-hover-effect')
        }
        },"-=1")
        .from('.opacity-fade-in',{opacity:0, duration:4, ease:'Power4.easeOut'})
        .from('.translate-in',{yPercent:200, duration:3, ease:'Power4.easeOut'},"<")
        .from('.translate-in-slow',{yPercent:100,opacity:0, duration:4, ease:'Power4.easeOut',
        onComplete:function(){ $nuxt.$emit('nextPlane') }
        },"<")


        // const projects = document.querySelectorAll('.project')
        // for(let i = 0; i < projects.length ; i++){
        //   tlLoad.from(`.project-${i}`, {y:100, duration:`${1 + i * 1.5}`, ease:'Power4.easeOut'},'<')
        // }
    },2000)
}
}
</script>

<style scoped lang="scss">
  .grid-container {
    background-color:#fff;
  }
  .grid-container > * {
    z-index:1;
  }

  .loading-bg {
    width:100vw;
    height: 100vh;
    background-color:#000;
    position:fixed;
    z-index: 100;
    display:flex;
    flex-direction:column-reverse;
    .loading-text{
      color:white;
      font-size:5rem;
      align-self: flex-end;
    }
  }
  .name{
    z-index:101;
    color: white;
    mix-blend-mode: difference;
  }
  .threejs-container {
    position:absolute;
    top:0;
    left:0;
    z-index: 0;
  }
</style>
<template>
<div>
  <section class="section-container section">
    <div class="loading-bg">
      <div class="loading-text">33%</div>
    </div>
    <div class="grid-container">
    

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
      <p class="service service-0">Web Design</p>
      <p class="service service-1">Web Development</p>
      <p class="service service-2">Art Direction</p>
      <p class="service service-3">Graphic Design</p>
      <p class="service service-4">Branding</p>
      <!-- <p class="service service-5">Design Research</p> -->
      <div class="available opacity-fade-in translate-in">
      <Available/>
      </div>
    </div>

    <ProjectsList class="projects translate-in-slow" />
    </div>
  </section>
</div>
</template>

<script>
import { gsap } from "gsap"
import Available from "./Available.vue"
export default {
    mounted() {
        setTimeout(() => {
            const tlLoad = gsap.timeline({});
            tlLoad.to(".loading-bg", { yPercent: -100, duration: 3 })
                .to(".loading-bg", { xPercent: 1000, duration: 0, ease: "none" })
                .to('.name h1',{fontSize:"10rem", duration:2, ease:'Power4.easeInOut',
                onComplete:function(){
                  document.querySelector('.name h1').classList.add('name-hover-effect')
                }
                },"-=1")
                .from(".opacity-fade-in", { opacity: 0, duration: 4, ease: "Power4.easeOut" })
                .from(".translate-in", { yPercent: 200, duration: 3, ease: "Power4.easeOut" }, "<")
                .from(".translate-in-slow", { yPercent: 100, opacity: 0, duration: 4, ease: "Power4.easeOut", onComplete: function () { $nuxt.$emit("nextPlane"); }
            }, "<");
            const projects = document.querySelectorAll('.project')
            for(let i = 0; i < projects.length ; i++){
              tlLoad.from(`.project-${i}`, {y:100, duration:`${1 + i * 1.1}`, ease:'Power4.easeOut'},'<')
            }
        }, 2000);
    },
    components: { Available }
}
</script>

<style scoped lang="scss">

  section{
    background-color: white;
  }

  .loading-bg {
    width:120vw;
    height: 120vh;
    background-color:#000;
    position:fixed;
    z-index: 100;
    display:flex;
    flex-direction:column-reverse;
    margin-left: -$margin;
    .loading-text{
      color:white;
      font-size:5rem;
      align-self: flex-end;
    }
  }
</style>
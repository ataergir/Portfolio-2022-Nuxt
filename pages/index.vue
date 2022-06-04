<template>
<div class="scroll-container">
  <div class="sections-container">
    <div class="fixed-div">
      <MainGrid class="main-grid"/>
    <div class="horizontal-scroll">
      <div class="pin-wrap">
        <SecondGrid/>
        <SecondGrid/>
        <SecondGrid/>
    </div>
    </div>
    </div>
    
  </div>
  
</div>
</template>

<script>
import { gsap } from "gsap"
import { ScrollTrigger } from "gsap/ScrollTrigger"
export default {
  name: 'IndexPage',
  mounted(){
    import("locomotive-scroll").then(locomotiveModule => {
      gsap.registerPlugin(ScrollTrigger)

      const locoScroll = new locomotiveModule.default({
        el: document.querySelector('.scroll-container'),
        smooth: true,
        smoothMobile: false,
      })

      locoScroll.on("scroll", ScrollTrigger.update)

      ScrollTrigger.scrollerProxy(".scroll-container", {
        scrollTop(value) {
          return arguments.length ? locoScroll.scrollTo(value, 0, 0) : locoScroll.scroll.instance.scroll.y
        },
        getBoundingClientRect() {
          return {top: 0, left: 0, width: window.innerWidth, height: window.innerHeight}
        },
        pinType: document.querySelector(".scroll-container").style.transform ? "transform" : "fixed"
      })

      window.addEventListener("load", function () {
      let pinBoxes = document.querySelectorAll(".pin-wrap > *");
      let pinWrap = document.querySelector(".pin-wrap");
      let pinWrapWidth = pinWrap.offsetWidth;
      let pinWrapHeight = pinWrap.offsetHeight;
      let horizontalScrollLength = pinWrapWidth * pinBoxes.length - window.innerWidth;

      const tlHorizontal = gsap.timeline({
        scrollTrigger: {
          trigger:".sections-container",
          scroller:'.scroll-container',
          // markers:true,
          start:'25% top',
          end:'bottom top',
          scrub:true
        }
      })

      tlHorizontal.to('.pin-wrap', {xPercent:-100, ease:'none'})

      const tlVertical = gsap.timeline({
        scrollTrigger: {
          trigger:".sections-container",
          scroller:'.scroll-container',
          // markers:true,
          start:'top top',
          end:'22% top',
          scrub:true
        }
      })
      
      tlVertical.to('.main-grid', {yPercent:100, ease:'none'})
      .from('.horizontal-scroll', {yPercent:-100, ease:'none'},0)

      .to('.contacts', {y:'-30vh', ease:'none'},0)
      .to('.name', {y:'-25vh', ease:'none'},0)
      .to('.services', {y:'-25vh', ease:'none'},0)
      .to('.threejs-container', {opacity:'0', ease:'none', y:'10vh'},0)
      // .to('.projects', {y:'-60vh', ease:'none'},0)

      // Projects
      const projects = document.querySelectorAll('.project')
      for(let i = 0; i < projects.length ; i++){
        tlVertical.to(`.project-${i}`, {y:`${i * 10}vh`, ease:'none'},0)
      }
      // Services
      const services = document.querySelectorAll('.service')
      for(let i = 0; i < services.length ; i++){
        tlVertical.to(`.service-${i}`, {y:`${i * 3}vh`, ease:'none'},0)
      }
      
      tlVertical.to('.services ul',{height:200, ease:'none'},0)
      .to('.services ul',{css:{borderLeftColor:'transparent'}, ease:'none'},0)
      
      // Name
      const nameLetters = document.querySelectorAll('.name span')
      for(let i = 1; i < nameLetters.length - 2 ; i++){
        if(i != 1 && i!= 4)
        tlVertical.to(`.name-letter-${i}`, {
          y:`${i * 2}vh`,
          ease:'none'},0)
      }

      //Contact
      const contacts = document.querySelectorAll('.contact')
      for(let i = 0; i < contacts.length ; i++){
        tlVertical.to(`.contact-${i}`, {y:`${i * 5}vh`, ease:'none'},0)
      }





      const fixedDiv = gsap.timeline({
        scrollTrigger: {
          trigger:".sections-container",
          scroller:'.scroll-container',
          // markers:true,
          start:'top top',
          end:'bottom bottom',
          scrub:true
        }
      })

      fixedDiv.to('.fixed-div', {y:'300vh', ease:'none'})



      ScrollTrigger.addEventListener("refresh", () => locoScroll.update());
      ScrollTrigger.refresh();
    });

    })
  }
}
</script>

<style lang="scss">
* {
  @include reset;
}
/* Hide scrollbar for Chrome, Safari and Opera */
.scroll-container::-webkit-scrollbar {
    display: none;
}

.main-grid{
  position:fixed;
  overflow:hidden;
}
.horizontal-scroll{
  opacity:1;
  position: fixed;
  cursor:default;
}


.scroll-container  {
  overflow:hidden;
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
.sections-container{
  position:relative;
  height: 400vh;
}

.pin-wrap{
  display: flex;
  flex-direction: row;
}

.pin-wrap > * {
  border-right:1px solid black;
}
</style>

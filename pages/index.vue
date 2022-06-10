<template>
<div class="scroll-container">
  <div class="sections-container">

    <div class="fixed-div">
      <MainGrid class="main-grid"/>
      
      <Three/>
      <ContactGrid/>
      <div class="horizontal-scroll">
        <div class="pin-wrap">
          <SecondGrid/>
          <ThirdGrid/>
        </div>
      </div>
      
      

    </div>
    
  </div>
  
</div>
</template>

<script>
import { gsap } from "gsap"
import { ScrollTrigger } from "gsap/ScrollTrigger"
import ContactGrid from "../components/ContactGrid.vue"
export default {
    name: "IndexPage",
    mounted() {
        import("locomotive-scroll").then(locomotiveModule => {
            gsap.registerPlugin(ScrollTrigger);
            const locoScroll = new locomotiveModule.default({
                el: document.querySelector(".scroll-container"),
                smooth: true,
                lerp: 0.05,
                smoothMobile: false,
            });
            locoScroll.on("scroll", ScrollTrigger.update);
            ScrollTrigger.scrollerProxy(".scroll-container", {
                scrollTop(value) {
                    return arguments.length ? locoScroll.scrollTo(value, 0, 0) : locoScroll.scroll.instance.scroll.y;
                },
                getBoundingClientRect() {
                    return { top: 0, left: 0, width: window.innerWidth, height: window.innerHeight };
                },
                pinType: document.querySelector(".scroll-container").style.transform ? "transform" : "fixed"
            });
            window.addEventListener("load", function () {
                const tl = gsap.timeline({
                    scrollTrigger: {
                        trigger: ".sections-container",
                        scroller: ".scroll-container",
                        // markers:true,
                        start: "top top",
                        end: "bottom bottom",
                        scrub: true
                    }
                });
                const cameraParams = {
                  y: -0.3,
                  z: 1.1
                };
                const invertEase = 'Power4.easeOut'
                const invertEase2 = 'Power4.easeIn'
                gsap.set(".horizontal-scroll", { yPercent: "-100" });

                tl
                    .to(".main-grid", { yPercent: 10, ease: "none" }, 0)
                    .to(".name", { yPercent: "-100", ease: "none" }, 0)
                    .from(".webgl", { y: "-100vh", ease: "none", }, 0)
                    .to(cameraParams, { y: 0.4, ease: "none", onUpdate: updateCamera })
                    .to(cameraParams, { z: 1.01, ease: "none", onUpdate: updateCamera }, "<")
                    .to(".horizontal-scroll", { yPercent: "0", ease: "Power2.easeOut", delay: -0.2 })
                    .from(".section2", { backgroundColor: "rgba(0,0,0,0)", ease: "Power4.easeOut"},'<')
                    .to(".pin-wrap", { x: "-99.9vw", ease: "none", delay: 0.1, onStart: eyeAnimationTrue, onReverseComplete: eyeAnimationFalse })


                    // .to(".pin-wrap *", { color: "rgba(0,0,0,1)", ease: invertEase2 }, "<")
                    // .to(".points-title", { color: "rgba(0,0,0,1)", ease: invertEase2 }, "<")
                    // .to(".pin-wrap *", { backgroundColor: "rgba(255,255,255,1)", ease: invertEase2 }, "<")
                    .to(".circle-title", { backgroundColor: "rgba(255,255,255,1)", ease: invertEase }, "<")
                    // .to(".title-line", { borderColor: "rgba(0,0,0,1)", ease: invertEase2 }, "<")


                    .to(".contact-container",{display:'grid', ease:'none'},'<')
                    .to(".main-grid",{display:'none', ease:'none'},'<')

                    .to(".horizontal-scroll", { y: "100vh", ease: "none" })
                    .from(".contact-container", { y: "33vh", ease: "none" },'<')

                function updateCamera() {
                    $nuxt.$emit("cameraUpdate", cameraParams)
                }

                function eyeAnimationFalse() {
                  $nuxt.$emit("eyeAnimationFalse")
                }
                function eyeAnimationTrue() {
                  $nuxt.$emit("eyeAnimationTrue")
                }

                tl.to('.projects-ul',{yPercent:100, duration:1, ease:'none'},0)
                const projects = document.querySelectorAll('.project')
                for(let i = 0; i < projects.length ; i++){
                  tl.to(`.project-${i}`, {y:`-${i * 10}`, ease:'none',duration:2},0)
                }
                const services = document.querySelectorAll('.service')
                for(let i = 0; i < services.length ; i++){
                  tl.to(`.service-${i}`, {opacity:`${i * 3}vh`, ease:'none',duration:2},0)
                }

                const nameLetters = document.querySelectorAll('.name span')
                for(let i = 1; i < nameLetters.length - 2 ; i++){
                  if(i != 1 && i!= 4)
                  tl.to(`.name-letter-${i}`, {
                    y:`${i * 2}vh`,
                    ease:'none',
                    duration:2},0)
                }
                const fixedDiv = gsap.timeline({
                    scrollTrigger: {
                        trigger: ".sections-container",
                        scroller: ".scroll-container",
                        start: "top top",
                        end: "bottom top",
                        scrub: true
                    }
                });
                fixedDiv.to(".fixed-div", { y: "1200vh", ease: "none" });
                ScrollTrigger.addEventListener("refresh", () => locoScroll.update());
                ScrollTrigger.refresh();
            });
        });
    },
    components: { ContactGrid }
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
  height: 1200vh;
}

.pin-wrap{
  display: flex;
  flex-direction: row;
}
</style>

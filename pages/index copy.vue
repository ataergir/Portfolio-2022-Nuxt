<template>
<div class="scroll-container">
  <MainGrid class="main-grid" data-scroll data-scroll-sticky />
  <div class="horizontal-scroll ">
    <div class="pin-wrap">
      <SecondGrid class="second-grid"/>
      <SecondGrid class="second-grid"/>
      <SecondGrid class="second-grid"/>
  </div>
  <SecondGrid class="second-grid"/>
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
        }, // we don't have to define a scrollLeft because we're only scrolling vertically.
        getBoundingClientRect() {
          return {top: 0, left: 0, width: window.innerWidth, height: window.innerHeight}
        },
        // LocomotiveScroll handles things completely differently on mobile devices - it doesn't even transform the container at all! So to get the correct behavior and avoid jitters, we should pin things with position: fixed on mobile. We sense it by checking to see if there's a transform applied to the container (the LocomotiveScroll-controlled element).
        pinType: document.querySelector(".scroll-container").style.transform ? "transform" : "fixed"
      })

      window.addEventListener("load", function () {
      let pinBoxes = document.querySelectorAll(".pin-wrap > *");
      let pinWrap = document.querySelector(".pin-wrap");
      let pinWrapWidth = pinWrap.offsetWidth;
      let pinWrapHeight = pinWrap.offsetHeight;
      let horizontalScrollLength = pinWrapWidth * pinBoxes.length - window.innerWidth;

      // Pinning and horizontal scrolling

      gsap.to(".pin-wrap", {
        scrollTrigger: {
          scroller: '.scroll-container',
          scrub: true,
          trigger: ".horizontal-scroll",
          pin: true,
          // anticipatePin: 1,
          start: "top top",
          end: pinWrapWidth
        },
        x: -horizontalScrollLength,
        ease: "none"
      });

      gsap.to(".main-grid", {
        scrollTrigger: {
          scroller: '.scroll-container',
          trigger: ".main-grid",
          start: "50% top",
          end:'top bottom',
          markers:true
        },
        x: 1000,
        ease: "none"
      });


      gsap.from(".horizontal-scroll", {
        scrollTrigger: {
          scroller: '.scroll-container',
          trigger: ".main-grid",
          start: "top top",
          end:'top bottom',
        },
        y: 1000,
        ease: "none"
      });

      ScrollTrigger.addEventListener("refresh", () => locoScroll.update()); //locomotive-scroll

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

.scroll-container  {
  overflow:hidden;
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}

.pin-wrap{
  display: flex;
  flex-direction: row;
}
</style>

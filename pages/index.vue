<template>
  <div class="scroll-container">
    <MainGrid class="main-grid" />
    <Three />
    <ContactGrid />

    <div class="horizontal-scroll">
      <div class="pin-wrap">
        <SecondGrid />
        <ThirdGrid />
      </div>
    </div>

    <div class="fake-section fake-section-1"></div>
    <div class="fake-section fake-section-2"></div>
    <div class="fake-section fake-section-3"></div>
    <div class="fake-section fake-section-4"></div>
    <div class="fake-section fake-section-5"></div>
    <div class="fake-section fake-section-6"></div>
    <div class="fake-section fake-section-7"></div>
    <div class="fake-section fake-section-8"></div>
    <div class="fake-section fake-section-9"></div>
    <div class="fake-section fake-section-10"></div>
  </div>
</template>

<script>
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
export default {
  name: 'IndexPage',
  mounted() {
    window.addEventListener('load', () => {
      setTimeout(()=>{
        document.querySelectorAll('.fake-section').forEach(section => {
          section.style.height = '100vh'
        })

        gsap.registerPlugin(ScrollTrigger)

        const tl = gsap.timeline({
          scrollTrigger: {
            markers: false,
            start: 'top top',
            end: 'bottom bottom',
            scrub: 2,
          },
        })
        const cameraParams = {
          y: -0.3,
          z: 1.1,
        }
        const invertEase = 'Power4.easeOut'
        const invertEase2 = 'Power4.easeIn'

        let nameYAnimation = -50
        const mediaQuery = window.matchMedia('(max-width: 768px)')
        if (mediaQuery.matches) {
          nameYAnimation = 50
        }
        gsap.set('.horizontal-scroll', { yPercent: '-100', display: 'grid' })
        gsap.set('.webgl', { y: '-100vh', display: 'inline' })
        tl.to('.name',{ yPercent: nameYAnimation, ease: 'none', duration: 6 },0)
        .to('.available-container',{ opacity: '0', ease: 'Power4.easeIn' },0)
        .to('#awwwards svg',{ xPercent: '-200', ease: 'Power4.easeOut', duration:6 },0)
        
        .to('.webgl', { y: '0', ease: 'none', duration: 1.5 }, 0)
        .to(cameraParams,{ y: 0.4, ease: 'none', duration: 3, onUpdate: updateCamera },'>')
        .to(cameraParams,{ z: 1.01, ease: 'none', duration: 3, onUpdate: updateCamera },'<')
        .to('.horizontal-scroll', {yPercent: '0',ease: 'none',delay: -0.2, duration:2},'>')
        .to('.star', {rotation: 180,ease: 'none', duration:2},'<')
        .to('.webgl', { yPercent: '98', ease: 'none', duration:2 }, '<')

        .to('.pin-wrap', {x: '-99.9vw', ease: 'none', delay: 0.5, duration:3,
          onStart: eyeAnimationTrue,
          onReverseComplete: eyeAnimationFalse,
        },'>')
        .to('.star', {rotation: 480, ease: 'none', duration:3},'<-=0.5')
        .to('.webgl', { yPercent: '0', ease: 'none' }, '<')

        // .to(".pin-wrap *", { color: "rgba(0,0,0,1)", ease: invertEase }, "<")
        // .to(".pin-wrap section", { backgroundColor: "rgba(255,255,255,1)", ease: invertEase }, "<")
        // .to(".circle-title", { borderColor: "rgba(0,0,0,1)",backgroundColor: "rgba(0,0,0,1)", ease: invertEase }, "<")
        // .to('.circle-title',{ backgroundColor: 'rgba(0,0,0,1)', ease: 'none' },'<')
        // .to(".title-line", { borderColor: "rgba(0,0,0,1)", ease: invertEase }, "<")
        

        .to('.contact-container', { display: 'grid', ease: 'none' }, '<')
        .to('.main-grid', { display: 'none', ease: 'none' }, '<')


        .to('.horizontal-scroll', { y: '100vh', ease: 'none', duration:2 })
        .from('.contact-container', { y: '33vh', ease: 'none', duration:2 }, '<')

        function updateCamera() {
          $nuxt.$emit('cameraUpdate', cameraParams)
        }

        function eyeAnimationFalse() {
          $nuxt.$emit('eyeAnimationFalse')
        }
        function eyeAnimationTrue() {
          $nuxt.$emit('eyeAnimationTrue')
        }

        tl.to('.projects-ul', { yPercent: 100, duration: 2, ease: 'none' }, 0)

        const services = document.querySelectorAll('.service')
        for (let i = 0; i < services.length; i++) {
          tl.to(
            `.service-${i}`,
            {
              xPercent: '-110',
              yPercent: '-10',
              ease: 'Power4.easeIn',
              duration: 3,
              delay: 0.1 * i,
            },
            0
          )
        }

        const nameLetters = document.querySelectorAll('.name span')
        for (let i = 1; i < nameLetters.length - 2; i++) {
          if (i != 1 && i != 4)
            tl.to(
              `.name-letter-${i}`,
              {
                y: `${i }vh`,
                ease: 'none',
                duration: 3,
              },
              0
            )
      }
      }, 7000)
  })
  }
}
</script>

<style lang="scss">
* {
  @include reset;
}

.main-grid {
  position: fixed;
  overflow: hidden;
}
.horizontal-scroll {
  position: fixed;
  display: none;
}
.webgl {
  display: none;
}

.pin-wrap {
  display: flex;
  flex-direction: row;
}

.sections-container {
  position: fixed;
  z-index: 2;
}

.scroll-container {
  width: 100vw;
  // overflow: hidden;
  // transform: none !important;
}

.fake-section {
  width: 0;
  height: 0;
  // pointer-events: none !important;
}
</style>

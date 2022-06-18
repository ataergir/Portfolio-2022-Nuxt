<template>
<div>
  <section class="section-container section2">
    <div class="grid-container">

      <div class="about-me-container">
        <p>I’m a freelance web designer and developer based in France. I studied graphic design in Art School  before discovering all the creative opportunities web developement can offer me. </p>
        <p>I pay a special  care  to communication as I think it is one of the most important aspect of a valuable work.</p>
      </div>

      <h2 class="points-title design-title">Design
        <div class="title-line"></div>
      </h2>
      

      <div class="design-point design-point-1">
        <div class="point-title-container">
          <div class="circle-title"></div><h4>Timeless</h4>
        </div>
        <p>Inspired by minimalist, brutalist and swiss design I want my designs to be timeless, elegant and content driven.</p>
      </div>

      <div class="icon clock-container">
        <div class="icon-wrap">
          <div class="clock-wrap">
            <span class="hour"></span>
            <span class="minute"></span>
            <span class="second"></span>
          </div>
        </div>
    </div>

      <div class="design-point design-point-2">
        <div class="point-title-container">
          <div class="circle-title"></div><h4>Impactful</h4>
        </div>
        <p>A unique, worth remembering and intuitive experience that immerses the viewer into an esthetic realm.</p>
      </div>

      <div class="icon stars-container">
        <div class="stars-wrap icon-wrap">
          <svg class="star-1 star"
            vector-effect="non-scaling-stroke"
            xmlns:dc="http://purl.org/dc/elements/1.1/"
            xmlns:cc="http://creativecommons.org/ns#"
            xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:svg="http://www.w3.org/2000/svg"
            xmlns="http://www.w3.org/2000/svg"
            version="1.0"
            width="528" height="528" viewBox="-264 -264 528 528">
            <polygon
              points="-36.16358,-87.30662 0,-233.85776 36.16358,-87.30662 165.36241,-165.36241 87.30662,-36.16358 233.85776,0 87.30662,36.16358 165.36241,165.36241 36.16358,87.30662 0,233.85776 -36.16358,87.30662 -165.36241,165.36241 -87.30662,36.16358 -233.85776,0 -87.30662,-36.16358 -165.36241,-165.36241 -36.16358,-87.30662 " />
          </svg>

          <svg class="star-2 star"
            vector-effect="non-scaling-stroke"
            xmlns:dc="http://purl.org/dc/elements/1.1/"
            xmlns:cc="http://creativecommons.org/ns#"
            xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:svg="http://www.w3.org/2000/svg"
            xmlns="http://www.w3.org/2000/svg"
            version="1.0"
            width="528" height="528" viewBox="-264 -264 528 528">
            <polygon
              points="-36.16358,-87.30662 0,-233.85776 36.16358,-87.30662 165.36241,-165.36241 87.30662,-36.16358 233.85776,0 87.30662,36.16358 165.36241,165.36241 36.16358,87.30662 0,233.85776 -36.16358,87.30662 -165.36241,165.36241 -87.30662,36.16358 -233.85776,0 -87.30662,-36.16358 -165.36241,-165.36241 -36.16358,-87.30662 " />
          </svg>

        </div>
      </div>

      <div class="design-point design-point-3">
        <div class="point-title-container">
          <div class="circle-title"></div><h4>Lively</h4>
        </div>
        <p>A playful narrative through graphics and motion makes the visitor wanting to take action and will attract more clients.</p>
      </div>

      <div class="icon circles-illustration-container">
        <div class="icon-wrap circles-illustration-wrap">
            <div class="circle-illustration circle-1"></div>
            <div class="circle-illustration circle-2"></div>
            <div class="circle-illustration circle-3"></div>
            <div class="circle-illustration circle-4"></div>
        </div>
      </div>

      <div class="design-point design-point-4">
        <div class="point-title-container">
          <div class="circle-title"></div><h4>Representative</h4>
        </div>
        <p>Websites represent the brand’s identity and values and thus should be designed with expertise and individuality.</p>
      </div>

      <div class="icon squares-container">
        <div class="squares-wrap icon-wrap">
          <div class="square square-1"></div>
          <div class="square square-2"></div>
        </div>
      </div>

      

    </div>
    
  </section>
</div>
</template>

<script>
export default {
  mounted(){
  let inc = 1000;

  clock()

  function clock() {
    const date = new Date()

    const hours = ((date.getHours() + 11) % 12 + 1)
    const minutes = date.getMinutes()
    const seconds = date.getSeconds()
    
    const hour = hours * 30;
    const minute = minutes * 6;
    const second = seconds * 6;
    
    document.querySelector('.hour').style.transform = `rotate(${hour}deg)`
    document.querySelector('.minute').style.transform = `rotate(${minute}deg)`
    document.querySelector('.second').style.transform = `rotate(${second}deg)`
  }

  setInterval(clock, inc)
  }
}
</script>
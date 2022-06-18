<template>
<div>
  <section class="section-container section3">
    <div class="grid-container">

       
      <h2 class="points-title dev-title">Developement
        <div class="title-line"></div>
      </h2>
            
      <div class="dev-point dev-point-1">
        <div class="point-title-container">
          <div class="circle-title"></div>
          <h4>Performant</h4>
        </div>
        <p>Everyone needs a fast loading, smooth and optimized web experience.</p>
      </div>

      <div class="icon dev-point-1-icon">
        <div class="icon-wrap dev-point-1-wrap">

        </div>
      </div>

      <div class="dev-point dev-point-2">
        <div class="point-title-container">
          <div class="circle-title"></div>
          <h4>Responsive</h4>
        </div>
        <p>Today, a vast majority of traffic comes from smartphones and it is therefore essential to adapt the website to any screen size.</p>
      </div>

      <div class="icon dev-point-2-icon">
        <div class="icon-wrap dev-point-2-wrap">
          <div class="desktop screen"></div>
          <div class="mobile screen"></div>
        </div>
      </div>

      <div class="dev-point dev-point-3">
        <div class="point-title-container">
          <div class="circle-title"></div>
          <h4>Clean code</h4>  
        </div>
        <p>My code is written in a short and efficient way so that it can always be modified and updated, while still being at the most performant state.</p>
      </div>

      <div class="icon dev-point-3-icon">
        <div class="icon-wrap dev-point-3-wrap">
          <div class="line line1"></div>
          <div class="line line2"></div>
          <div class="line line3"></div>
          <div class="line line4"></div>
        </div>
      </div>


      <div class="icon dev-point-4-icon">
        <div class="icon-wrap dev-point-4-wrap">
          
        </div>
      </div>

    </div>
  </section>
</div>
</template>

<script>
export default {
  mounted(){
    $nuxt.$emit('third-grid-loaded')
  }
}
</script>
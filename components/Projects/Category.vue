<template>
  <div class="category-container" :style="style">
      <p class="category-text">{{ category }}</p>
  </div>
</template>

<script>
export default {
    props: [
        'category'
    ],
    computed: {
      style () {
          let bgColor = '#E1F1FF'
          let textColor = 'black'
          let opacity = '1'

          switch(this.category){
            case 'Design': 
                bgColor = '#D9FFCC'
                textColor = 'black'
                break;
            case 'Art Direction': 
                bgColor = '#E1F1FF'
                textColor = 'black'
                break;
            case 'Developement': 
                bgColor = '#FFFACF'
                textColor = 'black'
                break;
            case 'WebGL': 
                bgColor = '255,255,0'
                textColor = 'black'
                break;
            case 'Front End': 
                bgColor = '0,255,120'
                textColor = 'black'
                break;
          }

        return 'background-color: ' + bgColor + '; opacity: ' + opacity + '; color: ' + textColor;
      }
    },
}
</script>

<style>
.category-container{
    border-radius: 9999vw;
    width:min-content;
    height:min-content;
    margin-bottom:0.2rem;
}
.category-text{
    opacity:1;
    padding:0.4rem 1rem;
    white-space: nowrap;
}
</style>
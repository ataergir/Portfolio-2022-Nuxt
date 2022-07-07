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
                bgColor = '#E1F1FF'
                textColor = 'black'
                break;
            case 'Art Direction': 
                bgColor = '#FFD6A5'
                textColor = 'black'
                break;
            case 'Development': 
                bgColor = '#FDFFB6'
                textColor = 'black'
                break;
            case 'WebGL': 
                bgColor = '#CAFFBF'
                textColor = 'black'
                break;
            case 'Motion Design': 
                bgColor = '#fec5bb'
                textColor = 'black'
                break;
            case 'Digital art': 
                bgColor = '#0000ff'
                textColor = 'white'
                break;
            case 'Photography': 
                bgColor = '#00dd00'
                textColor = 'white'
                break;
            case 'Experimental': 
                bgColor = '#CCA9DD'
                textColor = 'black'
                break;
            // case 'Front End': 
            //     bgColor = 'rgb(0,255,120)'
            //     textColor = 'black'
            //     break;
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
    font-size: 1.8rem;
}
</style>
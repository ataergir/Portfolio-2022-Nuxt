<template>
  <div>
    <ul class="projects-ul">
      <li v-for="(project, index) in this.projects" :key="index" class="project" :class="[`project-${index}`]" v-on:click="toggle(index)">

        <div class="outside-li hover-effect"
        :class="[`outside-li-${index}`]">
          <p class="date">{{ project.date }}</p>
        <p class="title">{{ project.title }}</p>
        </div>
        

        <div class="inside-li max-height-closed"
        :class="[`inside-li-${index}`]">

          <div class="margin"></div>

          <div class="img-container" v-if="project.images">
            <div class="img-bg" v-for="(img, index) in project.images" :key="index" :style="{ backgroundImage: `url(img/${img})`}"></div>
          </div>
          <div class="project-categories">
            <ProjectsCategory v-for="(category, index) in project.categories" :key="index" :category='category' class="project-category"/>
          </div>
          

          <p v-if="project.description" class="project-description">{{ project.description }}</p>
          
          <div class="margin"></div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import * as data from '~/static/json/projects.json';
import { gsap } from "gsap"
  export default {
    data() {
      return {
        projects: null,
        toggle(i) {
          const outsideLi = document.querySelector(`.outside-li-${i}`)

          if(outsideLi.classList.contains('open')){
            outsideLi.classList.remove('open')
            gsap.to(`.inside-li-${i}`, {height: "0", opacity:'0', duration: 1, ease:'Power4.easeOut'})
          } else {
            outsideLi.classList.add('open')
            gsap.to(`.inside-li-${i}`, {height: "auto", duration: 1, ease:'Power4.easeOut'})
            gsap.to(`.inside-li-${i}`, {opacity: "1", duration: 2, ease:'Power4.easeOut'})
          }
          
          const outsideLis = document.querySelectorAll('.outside-li')
          for(let y = 0; y < outsideLis.length ; y++){
            if(y != i){
              outsideLis[y].classList.remove('open')
              gsap.to(`.inside-li-${y}`, {height: "0", opacity:"0", duration: 1, ease:'Power4.easOut'})
            }
          }
        },
      }
    },
    mounted() {
      this.projects = data.default
    }
  }
</script>

<style lang="scss" scoped>
// $container-width:calc(100vw - 2 * $margin);
// $projects-width:calc($container-width * 0.66666);
// $single-column: calc(($projects-width / 8) - 7 * $column-gutter);
.projects-ul{
    li {
      .outside-li{
        border-top: 2px solid rgba(0,0,0,1);
        cursor: pointer;

        display: grid;
        grid-template-columns: repeat(8, 1fr);
        grid-template-rows: 1fr;
        grid-column-gap: $column-gutter;
        grid-row-gap: 0px;

        .date { 
          grid-area: 1 / 1 / 2 / 2;
          text-align: center;
          font-size:2.5rem;
        }

        

        .title {
          grid-area: 1 / 2 / 2 / 8;
          text-transform: lowercase;
          font-size: 3rem;
          transition: color 0.3s ease-out;
        }

        &:hover{
          .title{
            color:#999;
          }
        }
        }

        .inside-li{
          height:0;
          overflow:hidden;
          opacity:0;
          .project-description{
            text-align:justify;
          }
          .project-categories {
            display: flex;
            gap:0.2rem;
            flex-wrap: wrap;
          }
          .img-container{
            display:flex;
            flex-direction:column;
            justify-content: center;
            gap:1rem;
            margin-bottom:1rem;

            width:100%;
            height:auto;
            .img-bg{
              width:100%;
              height:33vh;
              border-radius:0.5rem;
              background-size: 100% auto;
            }
          }
          .margin{
            height:1rem;
            // background-color:blue;
          }
      }

      .open{
        &:hover{
          .title{
            color:#000;
          }
        }
      }

    }
}
</style>
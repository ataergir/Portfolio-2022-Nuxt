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

          <div class="img-container-square" v-if="project.squareImages">
            <div class="img-bg-square" v-for="(img, index) in project.squareImages" :key="index" :style="{ backgroundImage: `url(img/${img})`}"></div>
          </div>

          <div class="img-container-square" v-if="project.videos">
            <video v-for="(video, index) in project.videos" autoplay muted loop :key="index" :src="require(`~/static/video/${video}`)"></video>
          </div>

          <div class="text-categories-container">
            <div class="project-categories">
              <ProjectsCategory v-for="(category, index) in project.categories" :key="index" :category='category' class="project-category"/>
            </div>
            

            <p v-if="project.description" class="project-description">{{ project.description }}</p>

            <div class="project-links">
              <a v-if="project.link" :href='project.link' target="_blank" rel="noopener noreferrer">see live ➝</a>
              <a v-if="project.repo" :href='project.repo' target="_blank" rel="noopener noreferrer">see repo ➝</a>
              <a v-if="project.photography" :href='project.photography' target="_blank" rel="noopener noreferrer">see photos ➝</a>
              <a v-if="project.art" :href='project.art' target="_blank" rel="noopener noreferrer">see digital art ➝</a>
            </div>
            
          </div>
          
          
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
              gsap.to(`.inside-li-${y}`, {height: "0", opacity:"0", duration: 1, ease:'Power1.easeOut'})
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
.projects-ul{
    min-width:100%;
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
          font-size:3rem;
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
          display:flex;
          flex-direction: row;
          gap:1rem;
          align-content: right;

          @media screen and (min-width: 320px) {
            flex-direction: column;
            margin: 0 1rem;
          }
          @media screen and (min-width: 720px) {
            flex-direction: row;
            margin: 0 0;
          }

          .text-categories-container{
            display: flex;
            flex-direction: column;
            gap: 1rem;
            padding-top: 1rem;
          }
          .project-description{
            text-align:justify;
            font-size:1.8rem;
            max-width: 40rem;
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

            width:auto;
            height:auto;
            .img-bg{
              border-radius:0.5rem;
              background-size: 100% auto;
              @media screen and (min-width: 320px) {
                // width:calc(100vw - 2rem);
                // height:calc((100vw - 2rem) * (9/16)) ;
                width:calc(70vw);
                height:calc((70vw) * (9/16)) ;
              }
              @media screen and (min-width: 720px) {
                width:30vw;
                height:16.88vw;
              }
            }
          }

          .img-container-square{
            display:flex;
            flex-direction:row;
            justify-content: space-around;
            gap:1rem;
            margin-bottom:1rem;
            width:auto;
            height:auto;
            .img-bg-square{
              background-size: 100% auto;
              @media screen and (min-width: 320px) {
                width:calc((80vw - 2rem) * (9/16));
                height:calc((80vw - 2rem) * (9/16));
              }
              @media screen and (min-width: 720px) {
                width:16.88vw;
                height:16.88vw;
              }
            }

            video{
              @media screen and (min-width: 320px) {
                width:calc((80vw - 2rem) * (9/16));
                height:calc((80vw - 2rem) * (9/16));
              }
              @media screen and (min-width: 720px) {
                width:16.88vw;
                height:16.88vw;
              }
            }
          }

          .project-links{
            display:flex;
            flex-direction: column;
            align-self: flex-end;
            align-items: flex-end;

            @media screen and (min-width: 320px) {
              flex-direction: row;
              gap:1rem;
              }
              @media screen and (min-width: 720px) {
                flex-direction: column;
                gap:0;
              }
            a{
              color:black;
              font-size: 1.8rem;
            }
            
          }
          .margin{
            height:1rem;
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
<template>
    <canvas class="webgl"></canvas>
</template>

<script>
import { LoadingManager, PerspectiveCamera, Scene, SphereBufferGeometry, WebGLRenderer, Vector3, TextureLoader, RepeatWrapping } from 'three'
import Stats from 'stats.js'
import { Water } from '~/static/js/Water.js'

export default {
  name: 'Three',
  data() {
    return {
        camera: null,
        scene: null,
        renderer: null,
        mesh: null,
        stats: null,
        loadingManager: null,
        geometry:null,
        sizes : {
            width: null,
            height: null
        },
        cursor : {
            x: null,
            y: null
        },
        waterMesh:null,
        eyeAnimation: false

    }
  },

  methods: {
    init: function() {
        const canvas = document.querySelector('canvas.webgl')

        this.loadingManager = new LoadingManager()
        this.loadingManager.onLoad = () => {
        }

        this.sizes.width = window.innerWidth
        this.sizes.height = window.innerHeight 

        this.camera = new PerspectiveCamera(75, this.sizes.width / this.sizes.height, 0.0001, 100)
        this.camera.position.x = 0
        this.camera.position.y = -0.3
        this.camera.position.z = 1.1
        this.camera.lookAt(0,0,0)

        this.scene = new Scene()


        this.renderer = new WebGLRenderer({
            canvas: canvas,
            alpha:true,
            antialias: false
        })
        this.renderer.setSize(this.sizes.width, this.sizes.height)
        this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

        this.geometry = new SphereBufferGeometry( 1, 64, 64 )
    },
    initStats: function(){
        this.stats = new Stats()
        this.stats.showPanel( 0 ) // 0: fps, 1: ms, 2: mb, 3+: custom
        document.body.appendChild( this.stats.dom )
        this.stats.dom.classList.add('fps')
        this.stats.dom.style.width = '100%'
        this.stats.dom.style.height = '100%'
        this.stats.dom.style.position = 'static'
        document.querySelector('.dev-point-1-wrap').appendChild( this.stats.dom )
        document.querySelector('.fps').removeChild(document.querySelector('.fps').lastChild)


        document.querySelectorAll('.fps canvas').forEach(canvas =>{
            canvas.style.width = '100%'
            canvas.style.height = '60%'
        })

    },
    addEventListeners: function() {
        window.addEventListener('resize', () =>
        {
            this.resize()
        })

        const vectorEye = new Vector3(0,0,0)
        document.body.addEventListener('mousemove',(e)=>{
            if(this.eyeAnimation == true){
                vectorEye.x = 0.0005 * e.clientX
                vectorEye.y = 0.0008 * e.clientY
                this.waterMesh.material.uniforms.eye.value = vectorEye
            }
        })
    },
    initWater: function(){
        this.waterMesh = new Water(
            this.geometry,
            {
                textureWidth: 512,
                textureHeight: 512,
                waterNormals: new TextureLoader().load( '/img/normals.jpg', function ( texture ) {

                    texture.wrapS = texture.wrapT = RepeatWrapping

                } ),
                sunDirection: new Vector3(0.5,0,0),
                sunColor: 0xffffff,
                waterColor: 0xffffff,
                distortionScale: 200,
                fog: undefined,
            }
        )
        this.scene.add(this.waterMesh)
    },
    resize: function() {
        this.sizes.width = window.innerWidth
        this.sizes.height = window.innerHeight 

        this.camera.aspect = this.sizes.width / this.sizes.height
        this.camera.updateProjectionMatrix()

        this.renderer.setSize(this.sizes.width, this.sizes.height)
        this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

        document.querySelector('.name h1').style.fontSize = "8.3vw";
        
    },
    animate: function() {
        this.stats.begin()
        this.waterMesh.material.uniforms[ 'time' ].value += 0.3 / 60.0
        this.camera.lookAt(0,0,0)
        this.renderer.render(this.scene, this.camera)
        requestAnimationFrame(this.animate)
        this.stats.end()
    }
    },
  mounted() {
    this.init()
    this.initWater()
    this.addEventListeners()
    

    this.$nuxt.$on('cameraUpdate', position => {
        this.camera.position.y = position.y
        this.camera.position.z = position.z
    })

    this.$nuxt.$on('eyeAnimationFalse', () => {
        this.eyeAnimation = false
        this.waterMesh.material.uniforms.eye.value = new Vector3()
    })
    this.$nuxt.$on('eyeAnimationTrue', () => {
        this.eyeAnimation = true
    })

    let stats = false
    this.$nuxt.$on('third-grid-loaded', () => {
        if (stats == false){
            this.initStats()
            this.animate()
            stats = true
        }
    })
  }
}
</script>

<style>
canvas.webgl{
    position:fixed;
    width:100vw;
    height:100vh;
    mix-blend-mode: darken;
    pointer-events: none;
}
</style>
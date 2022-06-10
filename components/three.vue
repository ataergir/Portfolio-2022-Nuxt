<template>
    <canvas class="webgl"></canvas>
</template>

<script>
import * as THREE from 'three'
import Stats from 'stats.js'
import { gsap } from "gsap"
import { Water } from '~/static/js/Water.js'
import { EffectComposer } from 'three/examples/jsm/postprocessing/EffectComposer.js'
import { RenderPass } from 'three/examples/jsm/postprocessing/RenderPass.js'
import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass.js'

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
        params : {
            test: null,
            height:0.8
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

        this.loadingManager = new THREE.LoadingManager()
        this.loadingManager.onLoad = () => {
        }

        this.sizes.width = window.innerWidth
        this.sizes.height = window.innerHeight 

        this.camera = new THREE.PerspectiveCamera(75, this.sizes.width / this.sizes.height, 0.0001, 100)
        this.camera.position.x = 0
        this.camera.position.y = -0.3
        this.camera.position.z = 1.1
        this.camera.lookAt(0,0,0)

        // gist.github.com/ayamflow/96a1f554c3f88eef2f9d0024fc42940f
        // let dist = this.camera.position.z
        // let height = this.params.height
        // this.camera.fov = 2 * Math.atan(height / (2 * dist)) * (180 / Math.PI)
        // this.camera.updateProjectionMatrix()

        this.scene = new THREE.Scene()

        this.raycaster = new THREE.Raycaster()
        this.pointer = new THREE.Vector2()

        this.renderer = new THREE.WebGLRenderer({
            canvas: canvas,
            alpha:true,
            // antialias: true
        })
        this.renderer.setSize(this.sizes.width, this.sizes.height)
        this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

        this.geometry = new THREE.SphereBufferGeometry( 1, 64, 64 )
    },
    initStats: function(){
        this.stats = new Stats()
        this.stats.showPanel( 0 ) // 0: fps, 1: ms, 2: mb, 3+: custom
        // document.body.appendChild( this.stats.dom )
    },
    addEventListeners: function() {
        window.addEventListener('resize', () =>
        {
            this.resize()
        })

        document.body.addEventListener('mousemove',(e)=>{
            if(this.eyeAnimation == true){
                this.waterMesh.material.uniforms.eye.value = new THREE.Vector3(0.01 * e.clientX, 0.0005 * e.clientY, 0 )
            }
        })
    },
    initWater: function(){
        this.waterMesh = new Water(
            this.geometry,
            {
                textureWidth: 512,
                textureHeight: 512,
                waterNormals: new THREE.TextureLoader().load( '/img/normals.jpg', function ( texture ) {

                    texture.wrapS = texture.wrapT = THREE.RepeatWrapping

                } ),
                sunDirection: new THREE.Vector3(0.5,0,0),
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
    this.initStats()
    this.initWater()
    this.addEventListeners()
    this.animate()

    this.$nuxt.$on('cameraUpdate', position => {
        this.camera.position.y = position.y
        this.camera.position.z = position.z
    })

    this.$nuxt.$on('eyeAnimationFalse', () => {
        this.eyeAnimation = false
        this.waterMesh.material.uniforms.eye.value = new THREE.Vector3()
    })
    this.$nuxt.$on('eyeAnimationTrue', () => {
        this.eyeAnimation = true
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
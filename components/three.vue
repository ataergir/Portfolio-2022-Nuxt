<template>
    <canvas class="webgl"></canvas>
</template>

<script>
import * as THREE from 'Three'
import Stats from 'stats.js'
import { gsap } from "gsap"

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
        sizes : {
            width: null,
            height: null
        },
        planesMeshes : [],
        planesTrails : [],
        planeIndexCounter : 0,
        animationPlaying: null,

        raycaster:null,
        actualPlaneIndex: 0,

        params : {
        animationTimeOut: 1.5,
        animationTimeIn: 1.8,
        easeOut: "power4.easeInOut",
        easeIn: "power4.easeOut",
        planeHeight: 1.3,
        trailsAmount: 2,
        trailsDuration: 0.1,

        timeBeforeChangingIndex: 1,

        beforeX: 3,
        beforeY: -1,
        actualX: 0,
        actualY: 0,
        afterX: -3,
        afterY:1,

        beforeZRot :  -0.08 * Math.PI,
        actualZRot : -0.08 * Math.PI,
        afterZRot :  0.08 * Math.PI,

        trailsDurationOffestIn: 0.6,
        trailsDurationOffestOut: 3,

        trailsXOffsetBefore: 0.1,
        trailsYOffsetBefore: - 0.1,
        trailsZOffsetBefore: - 0.1,

        trailsXOffsetActual: 0.04,
        trailsYOffsetActual: - 0.06,
        trailsZOffsetActual: - 0.5,


        trailsXOffsetAfter: 0,
        trailsYOffsetAfter: 0,
        trailsZOffsetAfter: 0,


        trailsFade: 0.1,

        opacityOut: 0,
        opacityIn:0.7,

        opacityFadeInTime: 3,
        opacityFadeOutTime : 1,
        opacityFadeInEase: 'power4.easeOut',
        opacityFadeOutEase: 'power4.easeOut',
        },

        cursor : {
            x: null,
            y: null
        },

        pointer : {
            x:null,
            y:null
        }

    }
  },
  methods: {
    init: function() {
        const canvas = document.querySelector('canvas.webgl')
        this.stats = new Stats()
        this.stats.showPanel( 0 ) // 0: fps, 1: ms, 2: mb, 3+: custom
        document.body.appendChild( this.stats.dom )

        this.loadingManager = new THREE.LoadingManager()
        this.loadingManager.onLoad = () => {
            this.animationPlaying = false
        }

        this.sizes.width = window.innerWidth
        this.sizes.height = window.innerHeight

        this.camera = new THREE.PerspectiveCamera(75, this.sizes.width / this.sizes.height, 0.1, 100)
        this.camera.position.x = 0
        this.camera.position.y = 0
        this.camera.position.z = 2

        // gist.github.com/ayamflow/96a1f554c3f88eef2f9d0024fc42940f
        let dist = this.camera.position.z
        let height = this.params.planeHeight
        this.camera.fov = 2 * Math.atan(height / (2 * dist)) * (180 / Math.PI)
        this.camera.updateProjectionMatrix()

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

        this.addEventListeners()
    },
    addEventListeners: function() {
        window.addEventListener('resize', () =>
        {
            this.resize()
        })

        const section = document.querySelector('.main-grid')
        section.addEventListener('mousedown', () => {
            console.log('mousedown section1')
            if (this.animationPlaying == false){
                this.animationPlaying = true
                this.nextPlane()
            }
        })

        section.addEventListener('mousemove', (e) => {
            this.cursor.x = e.clientX / this.sizes.width - 0.5
            this.cursor.y = - e.clientY / this.sizes.height + 0.5

            this.pointer.x = ( e.clientX / window.innerWidth ) * 2 - 1;
	        this.pointer.y = - ( e.clientY / window.innerHeight ) * 2 + 1;

            // this.cursor.x = e.clientX / this.sizes.width
            // this.cursor.y = - e.clientY / this.sizes.height 
            // gsap.to(this.camera.position,{
            //     x: 0.5 - this.cursor.x * 0.7,
            //     y: -(0.2 + this.cursor.y * 0.2),
            //     ease:'Power4.easeOut',
            //     duration:0.2
            // })

            gsap.to(this.planesMeshes[this.actualPlaneIndex].rotation, {
            y: this.cursor.x * 0.7,
            x: -this.cursor.y * 0.3 - 0.1,
            duration: 1 , 
            ease: 'Power4.easeOut' })

            let counter = 1
            this.planesTrails[this.actualPlaneIndex].forEach(trail =>{
                gsap.to(trail.rotation,{
                    y: this.cursor.x * 0.7,
                    x: -this.cursor.y * 0.3 - 0.1,
                    duration: 1,
                    ease: 'Power4.easeOut'})

                gsap.to(trail.position,{
                    z:  (this.cursor.x - 0.7) * 0.2 * counter,
                    duration: 1,
                    ease: 'Power4.easeOut'})

                counter++
            })

        })
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
        this.camera.lookAt(0,0,0)

        this.raycaster.setFromCamera( this.pointer, this.camera )
        const intersects = this.raycaster.intersectObjects( this.scene.children )

        if(intersects.length){
            this.animationPlaying = false
            document.body.style.cursor='pointer'
        }else {
            this.animationPlaying = true
            document.body.style.cursor='default'
        }

        this.renderer.render(this.scene, this.camera)
        requestAnimationFrame(this.animate)
        this.stats.end()
    },
    loadTextures: function() {
        const imgTextures = []
        const planeGeometry = new THREE.PlaneBufferGeometry(0.6,1,16,16)
        const textureLoader = new THREE.TextureLoader(this.loadingManager)
        imgTextures.push(textureLoader.load('/img/1.jpg'))
        imgTextures.push(textureLoader.load('/img/2.jpg'))
        imgTextures.push(textureLoader.load('/img/3.jpg'))
        imgTextures.push(textureLoader.load('/img/4.jpg'))
        imgTextures.push(textureLoader.load('/img/5.jpg'))

        for(let i = 0 ; i < imgTextures.length ; i++){
            const uniforms = {
                uTime: {
                value: 0
                },
                uTexture: {
                value: imgTextures[i]
                },
                uOffset: {
                value: new THREE.Vector2(0.0, 0.0)
                },
                uAlpha: {
                value: 0
                }
            }
            const material = new THREE.ShaderMaterial({
                uniforms: uniforms,
                vertexShader: `
                    uniform vec2 uOffset;
            
                    varying vec2 vUv;
            
                    vec3 deformationCurve(vec3 position, vec2 uv, vec2 offset) {
                    float M_PI = 3.1415926535897932384626433832795;
                    position.x = position.x + (sin(uv.y * M_PI) * offset.x);
                    position.y = position.y + (sin(uv.x * M_PI) * offset.y);
                    return position;
                    }
            
                    void main() {
                    vUv = uv;
                    vec3 newPosition = position;
                    newPosition = deformationCurve(position,uv,uOffset);
                    gl_Position = projectionMatrix * modelViewMatrix * vec4( newPosition, 1.0 );
                    }
                `,
                fragmentShader: `
                    uniform sampler2D uTexture;
                    uniform float uAlpha;
                    uniform vec2 uOffset;
            
                    varying vec2 vUv;
            
                    void main() {
                    vec3 color = texture2D(uTexture,vUv).rgb;
                    gl_FragColor = vec4(color,uAlpha);
                    }
                `,
                transparent: true
            })

            const mesh = new THREE.Mesh(planeGeometry, material)

            //first mesh center
            // if(this.planesMeshes.length == 0)
            if(false){
                mesh.position.x = 0
                mesh.position.y = 0
                mesh.rotation.z = Math.PI * 0
            } else {
                mesh.position.x = this.params.beforeX
                mesh.position.y = this.params.beforeY
                mesh.rotation.z = - Math.PI * 0.1
            }
            this.planesMeshes.push(mesh)
            this.scene.add(mesh)

            const trails = []
            for (let i = 0; i < this.params.trailsAmount; i++) {
                let trailsPlane = mesh.clone()
                trails.push(trailsPlane)
                this.scene.add(trailsPlane)
                trailsPlane.position.z = -0.01
            }
            this.planesTrails.push(trails)
        }
    },
    nextPlane: function() {
        let nextPlane = this.planeIndexCounter + 1
        nextPlane == this.planesMeshes.length  ? nextPlane = 0 : ''

        this.planeToLeft(this.planeIndexCounter)
        this.planeToCenter(nextPlane)
        this.actualPlaneIndex = nextPlane

        this.trailsFollowToLeft(this.planeIndexCounter, nextPlane)
        this.trailsFollowToCenter(nextPlane)

        this.opacityFade(this.planeIndexCounter, 'out')
        this.opacityFade(nextPlane, 'in')

        setTimeout(()=>{
            this.planeIndexCounter = nextPlane
            this.animationPlaying = false
        }, this.params.timeBeforeChangingIndex * 1000)
    },
    planeToLeft: function(planeIndex){
        gsap.to(this.planesMeshes[planeIndex].position, {
            x: this.params.afterX, 
            y: this.params.afterY, 
            duration: this.params.animationTimeOut , 
            ease: this.params.easeOut})

        gsap.to(this.planesMeshes[planeIndex].rotation, {
            z: this.params.afterZRot, 
            duration: this.params.animationTimeOut , 
            ease: this.params.easeOut})
    },
    planeToCenter: function(planeIndex){
        gsap.to(this.planesMeshes[planeIndex].position, {
            x: this.params.actualX, y: this.params.actualY, 
            duration: this.params.animationTimeIn , 
            ease: this.params.easeIn})
        gsap.to(this.planesMeshes[planeIndex].rotation, {
            z: this.params.actualZRot, 
            duration: this.params.animationTimeIn , 
            ease: this.params.easeIn})
    },
    trailsFollowToLeft: function(planeIndex, nextPlaneIndex){
        const backToInitialPosition = (planeIndex, nextPlaneIndex) => {
            console.log('backToIntialPosition', planeIndex,'  ',  nextPlaneIndex)

            gsap.to(this.planesMeshes[planeIndex].position, {
                x: this.params.beforeX, 
                y: this.params.beforeY, 
                duration: 0 , 
                ease: 'none'})

            gsap.to(this.planesMeshes[planeIndex].rotation, {
                z: this.params.beforeZRot, 
                duration: 0 , 
                ease: 'none'})

            let trailsCounterBack = 1
            this.planesTrails[planeIndex].forEach(trail =>{
                gsap.to(trail.position,{
                    x: this.params.beforeX + this.params.trailsXOffsetBefore, 
                    y: this.params.beforeY + this.params.trailsYOffsetBefore,
                    z: trailsCounterBack * this.params.trailsZOffsetBefore,
                    duration: 0,
                    ease:'none'})
                    
                gsap.to(trail.rotation,{
                    z: this.params.beforeZRot,
                    duration: 0.1,
                    ease:'none'})

                trailsCounterBack ++ 
            })
        }

        let trailsCounterOut = 1
        this.planesTrails[planeIndex].forEach(trail =>{
            const duration = this.params.animationTimeOut + this.params.trailsDuration * (trailsCounterOut * this.params.trailsDurationOffestOut)

            if(trailsCounterOut == this.params.trailsAmount){
                gsap.to(trail.position,{
                    x: this.params.afterX + trailsCounterOut * this.params.trailsXOffsetAfter, 
                    y: this.params.afterY + trailsCounterOut * this.params.trailsYOffsetAfter,
                    z: trailsCounterOut * this.params.trailsZOffsetAfter, 
                    duration: duration,
                    onComplete:function(){
                        backToInitialPosition(planeIndex, nextPlaneIndex)
                        }
                    })
            } else {
                gsap.to(trail.position,{
                    x: this.params.afterX + trailsCounterOut * this.params.trailsXOffsetAfter, 
                    y: this.params.afterY + trailsCounterOut * this.params.trailsYOffsetAfter,
                    z: trailsCounterOut * this.params.trailsZOffsetAfter, 
                    duration: duration})
            }
            
            
            gsap.to(trail.rotation,{
                z: this.params.afterZRot,
                duration: duration})

            trailsCounterOut ++
        })
    },
    trailsFollowToCenter: function(planeIndex){
        let trailsCounterIn = 1
        this.planesTrails[planeIndex].forEach(trail =>{
            const duration = this.params.animationTimeIn + this.params.trailsDuration * (trailsCounterIn * this.params.trailsDurationOffestIn)
            gsap.to(trail.position,{
                x: this.params.actualX + trailsCounterIn * this.params.trailsXOffsetActual, 
                y: this.params.actualY + trailsCounterIn * this.params.trailsYOffsetActual,
                z: (trailsCounterIn * 0.2) * this.params.trailsZOffsetActual,
                duration: duration,
                ease:this.params.easeIn
            })
            gsap.to(trail.rotation,{
                z: this.params.actualZRot, 
                duration: duration,
                ease:this.params.easeIn
            })
            trailsCounterIn ++
        })
    },
    opacityFade: function(index, InOrOut){
        console.log('opacity fade ', index, '  ', InOrOut)
        let opacityValue = null
        let duration = null
        let ease = null
        if(InOrOut == 'in'){
            duration = this.params.opacityFadeInTime
            ease = this.params.opacityFadeInEase
            opacityValue = this.params.opacityIn
        } else if (InOrOut == 'out') {
            duration = this.params.opacityFadeOutTime
            ease = this.params.opacityFadeOutEase
            opacityValue = this.params.opacityOut
        } else {
            console.log('opacity fade error !')
        }

        gsap.to(this.planesMeshes[index].material.uniforms.uAlpha,{
            value:opacityValue,
            duration:duration,
            ease:ease
        })

        let trailsCounter = 1

        this.planesTrails[index].forEach(trail =>{
            gsap.to(trail.material.uniforms.uAlpha,{
                value: opacityValue - trailsCounter * this.params.trailsFade,
                duration:duration,
                ease:ease
            })
            trailsCounter ++ 
        })

    }
  },
  mounted() {
      this.init()
      this.loadTextures()
      this.animate()
      this.$nuxt.$on('nextPlane', ()=>{ this.nextPlane() })
  }
}
</script>
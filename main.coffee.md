Renderer
========

    require "./globals"
    require "./lib/obj_renderer"

    util = require "util"
    util.applyStylesheet(require("./style"))

    map = require("./map")()
    
    core = require "core"
    
    t = 0
    dt = 1 / 60

    container = document.createElement "div"
    document.body.appendChild container

    container.appendChild renderer.domElement
    renderer.setSize 800, 600 #window.innerWidth, window.innerHeight

    camera.position.set 0, 100, 200
    
    characters = {}

    init = ->
      addLights()

      map.generateGrid 10, 10, (mapCubes) ->
        console.log mapCubes

      map.populateCharacters (c) ->
        console.log c
        characters = c

    animate = ->
      requestAnimationFrame animate
      
      render()

    animateCharacters = (t) ->
      if beamSword = characters.beam_sword
        animationName = beamSword.userData.animations.grow.wrap (t / 0.25).floor()
        beamSword.children.forEach (component) ->
          component.visible = false
  
        beamSword.children.forEach (c) ->
          if animationName is c.name
            c.visible = true
         
    addLights = ->
      ambient = new THREE.AmbientLight 0x101030
      scene.add ambient

      directionalLight = new THREE.DirectionalLight 0xffeedd
      directionalLight.position.set 0, 0, 10
      scene.add directionalLight

    render = ->
      #animateCharacters(t)
    
      camera.lookAt scene.position

      renderer.render scene, camera
      t += dt

    init()
    animate()

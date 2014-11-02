Renderer
========

    require "./globals"
    require "./lib/obj_renderer"

    util = require "util"
    util.applyStylesheet(require("./style"))

    map = require("./map")()

    container = document.createElement "div"
    document.body.appendChild container

    container.appendChild renderer.domElement
    renderer.setSize 800, 600 #window.innerWidth, window.innerHeight

    camera.position.set 0, 100, 200
    
    characters = {}
    lastAnimated = +new Date()

    init = ->
      addLights()

      map.generateGrid 10, 10, (mapCubes) ->
        console.log mapCubes

      map.populateCharacters (c) ->
        characters = c

    animate = ->
      requestAnimationFrame animate
      
      render()

    animateCharacters = ->
      now = +new Date()
      
      if now - lastAnimated > 1000
        lastAnimated = now
    
        beamSword = characters.beam_sword
        beamSword.animationIndex ?= 0
        beamSword.children.forEach (component) ->
          component.visible = false
        
        debugger
        beamSword.userData.animations.grow.forEach (growFrame) ->
          if animations.grow[c.animationIndex] is growFrame.name
            growFrame.visible = true             

    addLights = ->
      ambient = new THREE.AmbientLight 0x101030
      scene.add ambient

      directionalLight = new THREE.DirectionalLight 0xffeedd
      directionalLight.position.set 0, 0, 10
      scene.add directionalLight

    render = ->
      animateCharacters()
    
      camera.lookAt scene.position

      renderer.render scene, camera

    init()
    animate()

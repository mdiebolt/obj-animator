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
    
    characters = []
    framesElapsed = 0

    init = ->
      addLights()

      map.generateGrid 10, 10, (mapCubes) ->
        console.log mapCubes

      map.populateCharacters (c) ->
        characters = c

    animate = ->
      framesElapsed += 1
      requestAnimationFrame animate
      
      characters.forEach (c) ->
        debugger
      
      render()

    addLights = ->
      ambient = new THREE.AmbientLight 0x101030
      scene.add ambient

      directionalLight = new THREE.DirectionalLight 0xffeedd
      directionalLight.position.set 0, 0, 10
      scene.add directionalLight

    render = ->
      camera.lookAt scene.position

      renderer.render scene, camera

    init()
    animate()

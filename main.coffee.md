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

      map.populateCharacters (c) ->
        characters = c

    animate = ->
      requestAnimationFrame animate

      render()

    addLights = ->
      ambient = new THREE.AmbientLight 0x101030
      scene.add ambient

      directionalLight = new THREE.DirectionalLight 0xffeedd
      directionalLight.position.set 0, 0, 10
      scene.add directionalLight

    renderCharacters = ->
      ;

    render = ->
      scene.clear()
      
      map.generateGrid 10, 10, (mapCubes) ->
        console.log mapCubes   
        
      renderCharacters()
      #animateCharacters(t)

      camera.lookAt scene.position

      renderer.render scene, camera
      t += dt

    init()
    animate()

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
    mapGeometry = []

    init = ->
      addLights()

      map.generateGrid 10, 10, (mapCubes) ->
        mapGeometry = mapCubes

      map.populateCharacters (c) ->
        console.log c
        characters = c

        setTimeout ->
          x = 0
          z = 0
          for name, actions of characters
            idle = actions.idle[0]
            idle.position.setX(x)
            idle.position.setZ(z)
            scene.add(idle)
            x += 10
            z += 10
        , 500

    animate = ->
      requestAnimationFrame animate

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
      t += dt

    init()
    animate()

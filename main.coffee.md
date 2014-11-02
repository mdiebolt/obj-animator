Renderer
========

    require "core"
    require "./globals"
    require "./lib/obj_renderer"

    util = require "util"
    util.applyStylesheet require("./style")

    Map = require "./map"

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

      Map.generateGrid 10, 10, (mapCubes) ->
        console.log mapCubes

      Map.populateItems (modelData) ->
        console.log "items", modelData
      
      Map.populateTerrain (modelData) ->
        console.log "terrain", modelData
  
      Map.populateCharacters (modelData) ->
        console.log "characters", modelData
        characters = modelData.characters

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
        , 1500

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

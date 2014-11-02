Renderer
========

    require "core"
    require "./globals"
    require "./lib/obj_renderer"

    util = require "util"
    util.applyStylesheet require("./style")

    Loader = require "./loader"
    gameLoop = require "./game_loop"
    
    cubeMesh = ->
      geometry = new THREE.BoxGeometry(CUBE_SIZE, CUBE_SIZE, CUBE_SIZE)

      material = new THREE.MeshBasicMaterial
        color: 0xfffff
        wireframe: true

      return new THREE.Mesh geometry, material

    cube = cubeMesh()

    addCubeAt = (position) ->
      clone = cube.clone()
      clone.position.set position.x, position.y, position.z
      scene.add clone

    characters = {}

    CUBE_SIZE = 10
    cubePositions = [0...width].map (x) ->
      [0...depth].map (z) ->
        new THREE.Vector3(x * CUBE_SIZE, -CUBE_SIZE / 2, z * CUBE_SIZE)
 
    Loader.fromObj("items", modelData.items).onLoad (modelData) ->
      console.log "items", modelData
 
    Loader.fromMesh
      name: "floor"
      type: "terrain"
      mesh: cube

    Loader.fromObj("terrain", modelData.terrain).onLoad (modelData) ->
      console.log "terrain", modelData

    Loader.fromObj("characters", modelData.characters).onLoad cb (modelData) ->
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
          gameLoop.start()
      , 1500

    gameLoop.update (dt) ->
      console.log dt

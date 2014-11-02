Main
====

    require "core"
    require "./lib/obj_renderer"

    util = require "util"
    util.applyStylesheet require("./style")

    Loader = require "./loader"
    gameLoop = require "./game_loop"
    modelData = require "./models" 
    
    scene = null 
    
    CUBE_SIZE = 10
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

    cubePositions = [0...10].map (x) ->
      [0...10].map (z) ->
        new THREE.Vector3(x * CUBE_SIZE, -CUBE_SIZE / 2, z * CUBE_SIZE)
 
    Loader.fromObj("items", modelData.items).onLoad (loadedData) ->
      console.log "items", loadedData 
 
    Loader.fromMesh
      name: "floor"
      type: "terrain"
      mesh: cube

    Loader.fromObj("terrain", modelData.terrain).onLoad (loadedData) ->
      console.log "terrain", loadedData

    Loader.fromObj("characters", modelData.characters).onLoad (loadedData) ->
      console.log "characters", loadedData
      characters = loadedData.characters

      setTimeout ->
        scene = gameLoop.start()
        
        x = 0
        z = 0
        
        for name, actions of characters
          idle = actions.idle[0]
          idle.position.setX(x)
          idle.position.setZ(z)
        
          x += 10
          z += 10
          
          scene.add(idle)
      , 1500

    gameLoop.update (t) ->
      console.log t

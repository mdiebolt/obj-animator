Main
====

    gameLoop = require "./game_loop"
    
    Loader = require "./loader"
    modelData = require "./models" 
    
    scene = null 
    
    CUBE_SIZE = 10

    cachedModels = {}

    Loader.fromObj("items", modelData.items)
    Loader.fromObj("terrain", modelData.terrain)

    manager = Loader.fromObj("characters", modelData.characters)

    cubeMesh = ->
      geometry = new THREE.BoxGeometry(CUBE_SIZE, CUBE_SIZE, CUBE_SIZE)

      material = new THREE.MeshBasicMaterial
        color: 0xfffff
        wireframe: true

      if cachedModels.terrain?.floor?.idle?
        cube = cachedModels.terrain.floor.idle
      else
        container = new THREE.Object3D()
        mesh = new THREE.Mesh geometry, material
        container.add(mesh)
        
        cachedModels.terrain ||= {}
        cachedModels.terrain.floor ||= {}
        cachedModels.terrain.floor.idle = [container] 
        
        cube = container

      cube
    
    addCharacters = (scene) ->
      x = 0
      z = 0        
      
      for name, actions of cachedModels.characters
        idle = actions.idle[0]
        
        idle.position.setX(x)
        idle.position.setZ(z)
      
        x += 10 
        z += 10
        
        scene.add idle    
  
    addMapCubes = (scene) ->
      [0...10].forEach (x) ->
        [0...10].forEach (z) ->
          {cx, cy, cz} = new THREE.Vector3(x * CUBE_SIZE, -CUBE_SIZE / 2, z * CUBE_SIZE)
          
          clone = cubeMesh().clone()
          clone.position.set(cx, cy, cz)       
      
          scene.add clone

    manager.onLoad (loadedData) ->
      # TODO: merge here instead of overwrite 
      cachedModels = loadedData 
      console.log cachedModels

      setTimeout ->
        scene = gameLoop.start()
                
        addMapCubes scene
        addCharacters scene
      , 1000

    gameLoop.update (t) ->
      console.log t

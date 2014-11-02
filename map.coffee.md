Map
===

Generate a simple map, populating it with a cube floor and characters.

    Loader = require "./loader"
    modelData = require "./models"

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

Create a basic floor of dimension `size`

    exports.generateGrid = (width, depth, cb) ->        
      [0...width].forEach (x) ->
        [0...depth].forEach (z) ->
          addCubeAt new THREE.Vector3(x * CUBE_SIZE, -CUBE_SIZE / 2, z * CUBE_SIZE)

    exports.populateItems = (cb) ->
      Loader.fromObj("items", modelData.items).onLoad cb
    
    exports.populateTerrain = (cb) ->
      Loader.fromMesh
        name: "floor" 
        type: "terrain"
        mesh: cube

      Loader.fromObj("terrain", modelData.terrain).onLoad cb

Load all the characters. Provide a callback that receives an array
of the characters when they're done loading. 

    exports.populateCharacters = (cb) ->
      Loader.fromObj("characters", modelData.characters).onLoad cb

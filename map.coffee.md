Map
===

Generate a simple map, populating it with a cube floor and characters.

    Loader = require "./loader"
    modelData = require "./characters"

    CUBE_SIZE = 10
    mapCubes = []

    cubeMesh = ->
      geometry = new THREE.BoxGeometry(CUBE_SIZE, CUBE_SIZE, CUBE_SIZE)

      material = new THREE.MeshBasicMaterial
        color: 0xfffff
        wireframe: true

      return new THREE.Mesh geometry, material    

    addCube = (position) ->
      cube = cubeMesh()
      cube.position.set position.x, position.y, position.z
      mapCubes.push cube
      scene.add cube

Create a basic floor of dimension `size`

    module.exports = ->

      generateGrid: (width, depth, cb) ->
        Loader.fromMesh
          name: "floor" 
          type: "terrain"
          mesh: cubeMesh()
        
        [0...width].forEach (x) ->
          [0...depth].forEach (z) ->
            addCube new THREE.Vector3(x * CUBE_SIZE, -CUBE_SIZE / 2, z * CUBE_SIZE)

        cb mapCubes

      populateItems: (cb) ->
        Loader.fromObj("items", modelData.items).onLoad cb
        
      populateTerrain: (cb) ->
        Loader.fromObj("terrain", modelData.terrain).onLoad cb

Load all the characters. Provide a callback that receives an array
of the characters when they're done loading. 

      populateCharacters: (cb) ->
        Loader.fromObj("characters", modelData.characters).onLoad cb

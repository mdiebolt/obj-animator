Map
===

Generate a simple map, populating it with a cube floor and characters.

    load = require "./loader"
    characterData = require "./characters"

    CUBE_SIZE = 10
    mapCubes = []

    addCube = (position) ->
      geometry = new THREE.BoxGeometry(CUBE_SIZE, CUBE_SIZE, CUBE_SIZE)
      material = new THREE.MeshBasicMaterial
        color: 0xfffff
        wireframe: true

      cube = new THREE.Mesh geometry, material
      cube.position.set position.x, position.y, position.z
      mapCubes.push cube
      scene.add cube

Create a basic floor of dimension `size`

    module.exports = ->

      generateGrid: (width, depth, cb) ->
        [0...width].forEach (x) ->
          [0...depth].forEach (z) ->
            addCube new THREE.Vector3(x * CUBE_SIZE, -CUBE_SIZE / 2, z * CUBE_SIZE)

        cb? mapCubes

Load all the characters. Provide a callback that receives an array
of the characters when they're done loading.

      populateCharacters: (cb) ->
        load(characterData).onLoad cb

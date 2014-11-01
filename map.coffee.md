Map
===

Generate a simple map, populating it with a cube floor and characters.

    load = require "./loader"

    {Vector3} = THREE

    CUBE_SIZE = 10

    addCube = (position) ->
      geometry = new THREE.BoxGeometry(CUBE_SIZE, CUBE_SIZE, CUBE_SIZE)
      material = new THREE.MeshBasicMaterial
        color: 0xfffff
        wireframe: true

      cube = new THREE.Mesh geometry, material
      cube.position.set position.x, position.y, position.z
      scene.add cube

Create a basic floor of dimension `size`

    module.exports = ->

      generateGrid: (size) ->
        [0...size].forEach (x) ->
          [0...size].forEach (z) ->
            addCube new Vector3(x * CUBE_SIZE, -5, z * CUBE_SIZE)

      populateCharacters: ->
        load [{
          name: "bartender"
          position: new Vector3(0, 0, 0)
        }, {
          name: "robo_sheriff"
          position: new Vector3(10, 0, 0)
        }, {
          name: "cactus"
          position: new Vector3(20, 0, 0)
        }, {
          name: "arrow"
          position: new Vector3(30, 0, 0)
        }, {
          name: "beam_sword"
          position: new Vector3(40, 0, 0)
        }, {
          name: "branding_iron"
          position: new Vector3(50, 0, 0)
        }, {
          name: "character"
          position: new Vector3(60, 0, 0)
        }, {
          name: "gun"
          position: new Vector3(70, 0, 0)
        }, {
          name: "hoverboard"
          position: new Vector3(80, 0, 0)
        }, {
          name: "jetpack_bandit"
          position: new Vector3(90, 0, 0)
        }]
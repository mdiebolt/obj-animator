Main
====

    core = require "core"

    Loader = require "./loader"
    modelData = require "./models"

    GameObject = require "./game_object"

    t = 0
    cachedModels = {}
    spreadsheetAttributes = {}

    addedToScene = false

    Loader.fromObj "items", modelData.items
    Loader.fromObj "terrain", modelData.terrain
    Loader.fromObj "characters", modelData.characters

    core.Loader.get()

    bartender = null
    roboSheriff = null

    addCharacters = (scene) ->
      x = 0
      z = 0

      keyValues cachedModels.characters, (name, actions) ->
        if idle = actions.idle[0]
          idle.position.set x, 5, z

          x += 10
          z += 10

          scene.add idle

    addItems = (scene) ->
      x = 90
      z = 0

      keyValues cachedModels.items, (name, actions) ->
        if idle = actions.idle[0]
          idle.position.set x, 5, z

          x -= 10
          z += 10

          scene.add idle

    updateCharacters = (scene, t, dt) ->
      spreadsheetAttributes.characters.forEach (character) ->
        if character.name is "Bartender"
          attrs = extend character, 
            cachedModels: cachedModels
            obj3D: new THREE.Object3D
            
          bartender ||= GameObject attrs
          bartender.move(0, 0.1)
        
        if character.name is "Robo Sheriff"
          attrs = extend character,
            cachedModels: cachedModels
            obj3D: new THREE.Object3D
            
          roboSheriff ||= GameObject attrs 
    
      roboSheriff?.setAnimation "idle", t

    $.when(Loader.finished(), core.Loader.get())
    .then (modelData, spreadsheetData) ->
      console.log modelData
      console.log spreadsheetData

      extend cachedModels, modelData
      extend spreadsheetAttributes, spreadsheetData

      core.init {}, (scene, t, dt) ->
        # Need this hack to prevent adding stuff to the scene each frame
        # adding to the scene each frame resets the model position
        unless addedToScene
          addCharacters scene
          addItems scene

          addedToScene = true

        updateCharacters scene, t, dt

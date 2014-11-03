Main
====

    core = require "core"

    Loader = require "./loader"
    modelData = require "./models"

    cachedModels = {}

    Loader.fromObj "items", modelData.items
    Loader.fromObj "terrain", modelData.terrain
    Loader.fromObj "characters", modelData.characters
    
    core.Loader.get()

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

    $.when(Loader.finished(), core.Loader.get()).then (modelData, spreadsheetData) ->
      console.log modelData
      console.log spreadsheetData
    
      extend cachedModels, modelData

      core.init {}, (scene, t, dt) ->
        addCharacters scene
        addItems scene

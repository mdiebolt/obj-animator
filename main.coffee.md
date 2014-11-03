Main
====

    core = require "core"

    Loader = require "./loader"
    modelData = require "./models"
    
    GameObject = require "./game_object"

    cachedModels = {}
    spreadsheetAttributes = {}

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

    updateCharacters = ->
      spreadsheetAttributes.Characters.forEach (character) ->
        if character.name is "Bartender"
          attrs = extend character, {cachedModels: cachedModels}
          bartender = GameObject(character)
          bartender.move(1, 1)

    $.when(Loader.finished(), core.Loader.get())
    .then (modelData, spreadsheetData) ->
      console.log modelData
      console.log spreadsheetData
    
      extend cachedModels, modelData
      extend spreadsheetAttributes, spreadsheetData
      
      core.init {}, (scene, t, dt) ->
        addCharacters scene
        addItems scene
        
        updateCharacters()

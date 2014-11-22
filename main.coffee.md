Main
====

    TacticsCore = require "tactics-core"

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

    TacticsCore.Loader.get()

    bartender = null
    roboSheriff = null
    roboSheriff2 = null
    
    characters = []

    addCharacters = (scene) ->
      roboSheriff = GameObject
        name: "Robo Sheriff"
        cachedModels: cachedModels

      characters.push roboSheriff
      scene.add roboSheriff.I.obj3D

      roboSheriff2 = GameObject
        name: "Robo Sheriff"
        cachedModels: cachedModels
        tOffset: 0.125
        position:
          x: 20
          y: 0
          z: 20

      characters.push roboSheriff2
      scene.add roboSheriff2.I.obj3D      

    $.when(Loader.finished(), TacticsCore.Loader.get())
    .then (modelData, spreadsheetData) ->
      console.log modelData
      console.log spreadsheetData

      extend cachedModels, modelData
      extend spreadsheetAttributes, spreadsheetData

      activeCharacter = null
      theScene = null

      TacticsCore.init
        data: {}
        update: (scene, t, dt) ->
          theScene = scene
          # Need this hack to prevent adding stuff to the scene each frame
          # adding to the scene each frame resets the model position
          unless addedToScene
            addCharacters scene
  
            addedToScene = true

          characters.invoke "update", t, dt
        clickObjectsFn: ->

          if activeCharacter
            theScene.children
          else
            characters.map (character) -> character.I.obj3D
        click: (results) ->
          if results[0]
            console.log results[0]

            {object} = results[0]

            console.log object

            if character = object.userData.character
              activeCharacter = character
            else
              # Move to location
              activeCharacter?.I.position.copy(object.position).setY(0)

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
    roboSheriff2 = null

    addCharacters = (scene) ->
      roboSheriff = GameObject
        name: "Robo Sheriff"
        cachedModels: cachedModels

      scene.add roboSheriff.I.obj3D

      roboSheriff2 = GameObject
        name: "Robo Sheriff"
        cachedModels: cachedModels
        position:
          x: 20
          y: 0
          z: 20

      scene.add roboSheriff2.I.obj3D

    updateCharacters = (scene, t, dt) ->
      roboSheriff.setAnimation("idle", t)
      roboSheriff2.setAnimation("idle", t + 0.125)

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

          addedToScene = true

        updateCharacters scene, t, dt

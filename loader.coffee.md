Loader
======

Loads voxel models from .obj files

Pass model data to the callback when the loading manager finishes

    manager = new THREE.LoadingManager (cb) ->
      cb? models

    texture = new THREE.Texture()

The models hash keeps our 3D models in memory.
Its structure looks like this.
Object3D is the THREE.js scene primitive

bartender:
  idle: [Object3D, Object3D, Object3D, Object3D]
  walk: [Object3D, ...]
cactus:
  idle: [...]

Object3D
  name: "bartender_idle_0"

Object3D
  name: "bartender_idle_1"

    models = {}

Base path to our game's S3 bucket

    BUCKET_PATH = "https://s3.amazonaws.com/distri-tactics"

We're sharing the same palette across all of our models.
Load up one from an arbitrary model we use.

    do ->
      loader = new THREE.ImageLoader(manager)
      loader.crossOrigin = true

      loader.load "#{BUCKET_PATH}/bartender.png", (image) ->
        texture.image = image
        texture.needsUpdate = true

Load a model by name, passing in an optional position.

    module.exports = (modelData) ->
      for name, actions of modelData
        models[name] ||= {}

        for actionName, fileNames of actions
          models[name][actionName] ||= []

          fileNames.forEach (file) ->
            loader = new THREE.OBJLoader(manager)
            loader.crossOrigin = true

            actionFrames = models[name][actionName]
            loader.load "#{BUCKET_PATH}/#{file}.obj", (obj3D) ->
              obj3D.name = file
              obj3D.children[0].material.map = texture

              actionFrames.push obj3D

      return manager

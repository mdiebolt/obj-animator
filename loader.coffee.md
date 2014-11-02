Loader
======

Loads voxel models from .obj files

Pass model data to the callback when the loading manager finishes

    manager = new THREE.LoadingManager (loadingComplete) ->
      loadingComplete? models

    texture = new THREE.Texture()

The models hash keeps our 3D models in memory.
Its structure looks like this. 

# characters:
#   bartender:
#     idle: [Object3D, Object3D, Object3D, Object3D]
#   walk: [Object3D, ...]
# terrain:
#   cactus: 
#     idle: [...] 
# items:
#   gun: 
#     idle: [...]
#
# Object3D is the THREE.js scene primitive
# Object3D
#   name: "bartender_idle_0"
#   children: [Mesh]
#
# Object3D
#   name: "bartender_idle_1"
#   children: [Mesh]

    models =
      characters: {}
      terrain: {}
      items: {}

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

    exports.fromObj = (namespace, modelData) ->
      cache = models[namespace]
      for name, actions of modelData
        cache[name] ||= {}
        for actionName, fileNames of actions
          cache[name][actionName] ||= []

          fileNames.forEach (file) ->
            loader = new THREE.OBJLoader(manager)
            loader.crossOrigin = true

            actionFrames = cache[name][actionName]
            loader.load "#{BUCKET_PATH}/#{file}.obj", (obj3D) ->
              obj3D.name = file
              obj3D.children[0].material.map = texture

              actionFrames.push obj3D

      return manager

    exports.fromMesh = (data={}) ->
      {mesh, name, type} = data

      obj3D = new THREE.Object3D()
      obj3D.name = name
      obj3D.add mesh

      models[type][name] =
        idle: [obj3D]

      return manager

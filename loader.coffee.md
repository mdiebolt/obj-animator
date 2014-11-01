Loader
======

Loads voxel models from .obj files

Pass in the characters collection when the loading manager finishes

    manager = new THREE.LoadingManager (cb) ->
      cb? characters
    
    texture = new THREE.Texture()
    characters = []

Base path to our game's S3 bucket

    BUCKET_PATH = "https://s3.amazonaws.com/distri-tactics"

Let us know how far along things are when loading a .obj model.

    onProgress = (xhr) ->
      if xhr.lengthComputable
        percentComplete = xhr.loaded / xhr.total * 100
        console.log "#{Math.round(percentComplete, 2)}% downloaded"

    onError = (xhr) ->
      console.error xhr

We're sharing the same palette across all of our models.
Load up one from an arbitrary model we use.

    do ->
      loader = new THREE.ImageLoader(manager)
      loader.crossOrigin = true

      loader.load "#{BUCKET_PATH}/bartender.png?doot2", (image) ->
        texture.image = image
        texture.needsUpdate = true

Load a model by name, passing in an optional position.

    module.exports = (objects) ->
      objects.forEach (attrs) ->
        position = attrs.position
  
        parent = new THREE.Object3d()
        parent.position.set position.x, position.y, position.z
  
        loader = new THREE.OBJLoader(manager)
        loader.crossOrigin = true
        loader.load "#{BUCKET_PATH}/#{attrs.name}.obj?doot2", (defaultModel) ->
          parent.name = attrs.name
          defaultModel.traverse (child) ->
            if child instanceof THREE.Mesh
              child.material.map = texture
                            
              loader.load "#{BUCKET_PATH}/#{attrs.name}_test.obj?doot2", (testModel) ->
                testModel.traverse (child) ->
                  if child instanceof THREE.Mesh
                    child.material.map = texture
                            
                    parent.add testModel
        
                , onProgress
                , onError              
              
              characters.push parent
              scene.add parent
              
              object.visible = false if parent.name is "bartender"
  
          , onProgress
          , onError
                  
      return manager

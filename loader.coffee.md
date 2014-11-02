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

        parent = new THREE.Object3D()
        parent.position.set position.x, position.y, position.z

        loader = new THREE.OBJLoader(manager)
        loader.crossOrigin = true
        loader.load "#{BUCKET_PATH}/#{attrs.name}.obj", (defaultModel) ->
          parent.name = attrs.name
          defaultModel.traverse (child) ->
            if child instanceof THREE.Mesh
              child.material.map = texture

              # TODO: default this in the character base class
              (attrs.animations || []).forEach (animation) ->
                for animationName, frames of animation
                  frames.forEach (frame) ->
                    loader.load "#{BUCKET_PATH}/#{frame}.obj", (frameModel) ->
                      frameModel.name = "#{animationName}_#{frame}"
                      frameModel.visible = false
                    
                      frameModel.traverse (c) ->
                        if c instanceof THREE.Mesh
                          c.material.map = texture

                          parent.add frameModel

              parent.add defaultModel
              characters.push parent
              scene.add parent

      return manager

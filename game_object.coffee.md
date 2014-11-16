Game Object
===========

Manage character data and its state

    module.exports = (I={}) ->
      defaults I,
        tOffset: 0
        active: true
        state: "idle"
        obj3D: new THREE.Object3D
        position: new THREE.Vector3(I.position?.x ? 0, I.position?.y ? 0, I.position?.z ? 0)

      I.obj3D.position.copy I.position

      self =
        I: I
        move: (byX, byZ) ->
          keyValues I.cachedModels.characters, (name, actions) ->
            if name is I.name.toLowerCase()
              if idle = actions.idle[0]
                x = idle.position.x + byX
                z = idle.position.z + byZ

                idle.position.setX x
                idle.position.setZ z

        setAnimation: (name, time=0) ->
          characterName = I.name.dasherize().underscore()
          state = I.cachedModels.characters[characterName][name]

          frame = state.wrap (time / 0.25).floor()

          if I.obj3D.children[0]
            I.obj3D.remove I.obj3D.children[0]

          # Need our own mesh copy
          mesh = frame.children[0]
          ourMesh = new THREE.Mesh(mesh.geometry, mesh.material)
          ourMesh.userData = self

          I.obj3D.add ourMesh

        update: (t, dt) ->
          # Update position
          # Update animations
          self.setAnimation("idle", t + I.tOffset)
          # etc.
          I.obj3D.position.copy I.position

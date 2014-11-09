Game Object
===========

Manage character data and its state

    module.exports = (I={}) ->
      defaults I,
        active: true
        state: "idle"

      self =
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
          I.obj3D.geometry = frame
          
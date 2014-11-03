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
                debugger
                x = idle.position.x + byX
                z = idle.position.z + byZ
                
                idle.position.set x, 5, z
                
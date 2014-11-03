Game Object
===========

Manage character data and its state

    module.exports = (I={}) ->
      defaults I,
        active: true
        state: "idle"
      
      self = 
        move: (x, z) ->
          keyValues I.cachedModels.characters, (name, actions) ->
            if name is I.name
              if idle = actions.idle[0]
                x = idle.position.x
                z = idle.position.z
                
                idle.position.setX x
                idle.position.setZ z
                
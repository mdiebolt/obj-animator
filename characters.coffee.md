Models
======

Model data for each character and their actions (animation states).

    module.exports =
      characters:
        bartender:
          idle: ["bartender"]
          wink: ["bartender_wink_0"]
        robo_sheriff:
          idle: ["robo_sheriff"]
        character:
          idle: ["character"]
        jetpack_bandit:
          idle: ["jetpack_bandit"]
      terrain:
        cactus:
          idle: ["cactus"]
      items:
        arrow:
          idle: ["arrow"]
        beam_sword:
          idle: ["beam_sword_idle_0"]
          grow: [
            "beam_sword_grow_0"
            "beam_sword_grow_1"
            "beam_sword_grow_2"
            "beam_sword_grow_3"
          ]
        branding_iron:
          idle: ["branding_iron"]
        gun:
          idle: ["gun"]
        hoverboard:
          idle: ["hoverboard"]

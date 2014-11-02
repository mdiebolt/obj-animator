Game Loop
=========

    require "core"

    lights = require "./lights"
    util = require "util"

    t = 0
    dt = 1 / 60
    started = false

    aspectRatio = window.innerWidth / window.innerHeight

    renderer = new THREE.WebGLRenderer()
    scene = new THREE.Scene()
    camera = new THREE.PerspectiveCamera(45, aspectRatio, 1, 2000)

    do ->
      util.applyStylesheet require("./style")
      camera.position.set 0, 100, 200

      scene.add lights.ambient()
      scene.add lights.directional()

      renderer.setSize window.innerWidth, window.innerHeight
      
      camera.aspect	= window.innerWidth / window.innerHeight
      camera.updateProjectionMatrix()
      
      document.body.appendChild renderer.domElement

    animate = ->
      requestAnimationFrame animate

      render()

    render = ->
      camera.lookAt scene.position

      renderer.render scene, camera
      t += dt

    exports.start = ->
      started = true
      animate()

      scene

    exports.update = (updateFn) ->
      updateFn(t) if started

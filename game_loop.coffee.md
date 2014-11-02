Game Loop
=========

    lights = require "./lights"

    t = 0
    dt = 1 / 60
    started = false

    aspectRatio = 800 / 600

    renderer = new THREE.WebGLRenderer()
    scene = new THREE.Scene()
    camera = new THREE.PerspectiveCamera(45, aspectRatio, 1, 2000)

    do ->
      camera.position.set 0, 100, 200
      
      scene.add lights.ambient()
      scene.add lights.directional()
      
      renderer.setSize 800, 600 #window.innerWidth, window.innerHeight
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
Globals
=======

Put all these gross guys in here.

    t = 0
    dt = 1 / 60
    started = false

    aspectRatio = 800 / 600

    renderer = new THREE.WebGLRenderer()
    scene = new THREE.Scene()
    camera = new THREE.PerspectiveCamera(45, aspectRatio, 1, 2000)

    document.body.appendChild renderer.domElement
    renderer.setSize 800, 600 #window.innerWidth, window.innerHeight

    camera.position.set 0, 100, 200

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
      
    exports.update = (updateFn) ->
      updateFn(t) if started
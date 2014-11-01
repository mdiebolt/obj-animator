Renderer
========

    require "./globals"
    require "./lib/obj_renderer"

    util = require "util"
    util.applyStylesheet(require("./style"))

    map = require("./map")()

    particles = []

    container = document.createElement "div"
    document.body.appendChild container

    container.appendChild renderer.domElement
    renderer.setSize window.innerWidth, window.innerHeight

    camera.position.set 0, 100, 200

    init = ->
      addLights()

      map.generateGrid(10)
      map.populateCharacters()

    animate = ->
      requestAnimationFrame animate
      render()

    addLights = ->
      ambient = new THREE.AmbientLight 0x101030
      scene.add ambient

      directionalLight = new THREE.DirectionalLight 0xffeedd
      directionalLight.position.set 0, 0, 10
      scene.add directionalLight

    render = ->
      camera.lookAt scene.position

      renderer.render scene, camera

    init()
    animate()

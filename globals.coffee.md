Globals
=======

Put all these gross guys in here.

    aspectRatio = 800 / 600

    window.renderer = new THREE.WebGLRenderer()
    window.scene = new THREE.Scene()
    window.camera = new THREE.PerspectiveCamera(45, aspectRatio, 1, 2000)


    THREE.Object3D.prototype.clear = ->
      @children.forEach (child) ->
        scene.remove(child)

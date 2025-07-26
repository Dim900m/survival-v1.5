<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Improved 3D RPG Survival Prototype</title>
  <style>
    body { margin: 0; overflow: hidden; background: #222; color: #eee; font-family: monospace; }
    #info, #inventory {
      position: absolute; z-index: 10; background: rgba(0,0,0,0.6);
      padding: 10px; border-radius: 6px;
    }
    #info { top: 10px; left: 10px; }
    #inventory {
      top: 50px; left: 10px; width: 220px; max-height: 300px; overflow-y: auto;
      display: none;
    }
    #inventory h2 { margin: 0 0 8px; font-size: 1.1em; }
    #inventory ul { margin: 0; padding-left: 1.2em; }
    #inventory li { margin-bottom: 4px; }
  </style>
</head>
<body>
  <div id="info">
    <p>W/S: Move forward/backward · A/D: Strafe · Arrows: Look · I: Inventory</p>
    <p>Health: <span id="health">100</span> · Hunger: <span id="hunger">100</span></p>
  </div>
  <div id="inventory">
    <h2>Inventory</h2>
    <ul id="inv-list"><li><i>Empty</i></li></ul>
    <small>Press 1,2,3 to add test items.</small>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/three@0.152.2/build/three.min.js"></script>
  <script>
    class InputHandler {
      constructor() {
        this.keys = new Set();
        window.addEventListener('keydown', e => this.onKey(e, true));
        window.addEventListener('keyup',   e => this.onKey(e, false));
      }
      onKey(e, down) {
        const key = e.key.toLowerCase();
        if (['arrowup','arrowdown','arrowleft','arrowright','i'].includes(key)) e.preventDefault();
        down ? this.keys.add(key) : this.keys.delete(key);
      }
      isPressed(key) { return this.keys.has(key); }
    }

    class Inventory {
      constructor(el) {
        this.container = el;
        this.list = el.querySelector('#inv-list');
        this.items = {};
      }
      toggle() { this.container.style.display = this.container.style.display === 'none' ? 'block' : 'none'; }
      add(name, count = 1) {
        this.items[name] = (this.items[name] || 0) + count;
        this.render();
      }
      render() {
        this.list.innerHTML = '';
        if (!Object.keys(this.items).length) {
          this.list.innerHTML = '<li><i>Empty</i></li>'; return;
        }
        for (const [name, qty] of Object.entries(this.items)) {
          const li = document.createElement('li');
          li.textContent = `${name} x${qty}`;
          this.list.appendChild(li);
        }
      }
    }

    class Player {
      constructor(scene) {
        const geom = new THREE.CylinderGeometry(0.5, 0.5, 2, 16);
        const mat  = new THREE.MeshPhongMaterial({ color: 0x44aa44 });
        this.mesh = new THREE.Mesh(geom, mat);
        this.mesh.position.y = 1;
        scene.add(this.mesh);
        this.health = 100;
        this.hunger = 100;
      }
      updateStatusUI() {
        document.getElementById('health').textContent = this.health;
        document.getElementById('hunger').textContent = this.hunger;
      }
      drain(delta) {
        // hunger drains 1 per second
        this.hunger = Math.max(0, this.hunger - delta);
        if (this.hunger === 0) this.health = Math.max(0, this.health - delta);
        this.updateStatusUI();
      }
    }

    class Game {
      constructor() {
        this.input = new InputHandler();
        this.inventory = new Inventory(document.getElementById('inventory'));
        this.initThree();
        this.player = new Player(this.scene);
        this.setupTestItems();
        this.lastTime = performance.now();
        this.animate();
        this.setupResize();
      }

      initThree() {
        this.scene = new THREE.Scene();
        this.camera = new THREE.PerspectiveCamera(75, innerWidth/innerHeight, 0.1, 1000);
        this.renderer = new THREE.WebGLRenderer({ antialias: true });
        this.renderer.setSize(innerWidth, innerHeight);
        document.body.appendChild(this.renderer.domElement);

        // Ground
        const ground = new THREE.Mesh(
          new THREE.PlaneGeometry(100,100),
          new THREE.MeshPhongMaterial({ color: 0x556633 })
        );
        ground.rotation.x = -Math.PI/2;
        this.scene.add(ground);

        // Light
        const light = new THREE.DirectionalLight(0xffffff, 1);
        light.position.set(10,10,10);
        this.scene.add(light);

        // Camera angles
        this.yaw = 0; this.pitch = 0;
        this.maxPitch = Math.PI/3;
        this.camDist = 10;
      }

      setupTestItems() {
        window.addEventListener('keydown', e => {
          if (['1','2','3'].includes(e.key) && !['input','textarea'].includes(e.target.tagName.toLowerCase())) {
            const mapping = { '1': ['Wood',5], '2': ['Stone',3], '3': ['Food',2] };
            const [name, qty] = mapping[e.key];
            this.inventory.add(name, qty);
          }
          if (e.key.toLowerCase() === 'i') this.inventory.toggle();
        });
      }

      movePlayer(delta) {
        const speed = 5;
        let f = 0, r = 0;
        if (this.input.isPressed('w')) f =  1;
        if (this.input.isPressed('s')) f = -1;
        if (this.input.isPressed('a')) r = -1;
        if (this.input.isPressed('d')) r =  1;
        const len = Math.hypot(f, r) || 1;
        f/=len; r/=len;
        const sinY = Math.sin(this.yaw), cosY = Math.cos(this.yaw);
        this.player.mesh.position.x += (r*cosY - f*sinY)*speed*delta;
        this.player.mesh.position.z += (r*sinY + f*cosY)*speed*delta;
      }

      updateCamera() {
        const x = this.player.mesh.position.x + this.camDist * Math.sin(this.yaw)*Math.cos(this.pitch);
        const y = this.player.mesh.position.y + 5 + this.camDist * Math.sin(this.pitch);
        const z = this.player.mesh.position.z + this.camDist * Math.cos(this.yaw)*Math.cos(this.pitch);
        this.camera.position.set(x,y,z);
        this.camera.lookAt(
          this.player.mesh.position.x,
          this.player.mesh.position.y + 1,
          this.player.mesh.position.z
        );
      }

      handleCamera(delta) {
        const rs = 2;
        if (this.input.isPressed('arrowleft'))  this.yaw   -= rs*delta;
        if (this.input.isPressed('arrowright')) this.yaw   += rs*delta;
        if (this.input.isPressed('arrowup'))    this.pitch = Math.min(this.maxPitch, this.pitch+rs*delta);
        if (this.input.isPressed('arrowdown'))  this.pitch = Math.max(-this.maxPitch, this.pitch-rs*delta);
      }

      animate() {
        requestAnimationFrame(() => this.animate());
        const now = performance.now(); const delta = (now - this.lastTime)/1000; this.lastTime = now;
        this.movePlayer(delta);
        this.handleCamera(delta);
        this.updateCamera();
        this.player.drain(delta);
        this.renderer.render(this.scene, this.camera);
      }

      setupResize() {
        window.addEventListener('resize', () => {
          this.camera.aspect = innerWidth/innerHeight;
          this.camera.updateProjectionMatrix();
          this.renderer.setSize(innerWidth, innerHeight);
        });
      }
    }

    // Start the game
    new Game();
  </script>
</body>
</html>

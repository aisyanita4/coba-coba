<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Visualisasi Isotop Silikon</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      font-family: Arial, sans-serif;
      background-color: #111;
      color: #fff;
    }
    #legend {
      position: absolute;
      top: 10px;
      left: 10px;
      background: rgba(0,0,0,0.6);
      padding: 10px;
      border-radius: 8px;
      font-size: 14px;
    }
    .item {
      display: flex;
      align-items: center;
      margin-bottom: 4px;
    }
    .box {
      width: 16px;
      height: 16px;
      margin-right: 8px;
      border-radius: 4px;
    }
  </style>
</head>
<body>
  <div id="legend">
    <div class="item"><div class="box" style="background-color:#ff6b6b"></div>Silikon-28</div>
    <div class="item"><div class="box" style="background-color:#4ecdc4"></div>Silikon-29</div>
    <div class="item"><div class="box" style="background-color:#ffe66d"></div>Silikon-30</div>
  </div>

  <!-- Three.js -->
  <script src="https://cdn.jsdelivr.net/npm/three@0.146.0/build/three.min.js"></script>

  <script>
    let scene, camera, renderer, particles = [];

    function init() {
      scene = new THREE.Scene();

      camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      camera.position.z = 50;

      renderer = new THREE.WebGLRenderer({ antialias: true });
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setPixelRatio(window.devicePixelRatio);
      renderer.outputColorSpace = THREE.SRGBColorSpace;
      renderer.toneMapping = THREE.ACESFilmicToneMapping;
      renderer.toneMappingExposure = 1;
      document.body.appendChild(renderer.domElement);

      // Warna isotop
      const colors = {
        Si28: 0xff6b6b,
        Si29: 0x4ecdc4,
        Si30: 0xffe66d
      };

      // Buat partikel acak untuk tiap isotop
      for (let i = 0; i < 300; i++) {
        let isotope;
        let rand = Math.random();
        if (rand < 0.92) isotope = "Si28"; // ~92%
        else if (rand < 0.97) isotope = "Si29"; // ~5%
        else isotope = "Si30"; // ~3%

        const geometry = new THREE.SphereGeometry(0.5, 12, 12);
        const material = new THREE.MeshStandardMaterial({
          color: colors[isotope],
          roughness: 0.4,
          metalness: 0.1
        });
        const sphere = new THREE.Mesh(geometry, material);

        sphere.position.set(
          (Math.random() - 0.5) * 60,
          (Math.random() - 0.5) * 60,
          (Math.random() - 0.5) * 60
        );

        scene.add(sphere);
        particles.push(sphere);
      }

      // Cahaya
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
      scene.add(ambientLight);

      const pointLight = new THREE.PointLight(0xffffff, 1);
      pointLight.position.set(25, 50, 25);
      scene.add(pointLight);

      window.addEventListener("resize", onWindowResize);
    }

    function onWindowResize() {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    }

    function animate() {
      requestAnimationFrame(animate);

      // Rotasi partikel
      particles.forEach(p => {
        p.rotation.x += 0.01;
        p.rotation.y += 0.01;
      });

      scene.rotation.y += 0.001;

      renderer.render(scene, camera);
    }

    init();
    animate();
  </script>
</body>
</html>

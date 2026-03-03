<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crystal Lattice Constructor</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
    <style>
        body { margin: 0; overflow: hidden; background-color: #f3f4f6; font-family: sans-serif; }
        #canvas-container { width: 100%; height: 60vh; position: relative; border-radius: 1rem; overflow: hidden; }
        canvas { display: block; }
        .info-card { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(4px); }
        input[type="range"] { accent-color: #2563eb; }
        .toggle-checkbox:checked { right: 0; border-color: #68D391; }
        .toggle-checkbox:checked + .toggle-label { background-color: #68D391; }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex flex-col p-2 md:p-6">

    <div class="max-w-5xl mx-auto w-full bg-white rounded-2xl shadow-xl overflow-hidden flex flex-col">
        <!-- Header -->
        <div class="p-4 md:p-5 border-b border-gray-100 flex justify-between items-center">
            <div>
                <h1 class="text-xl md:text-2xl font-bold text-gray-800">Crystal Lattice Constructor</h1>
                <p class="text-gray-500 text-xs md:text-sm">Visualize solid-state physics structures using primitive vectors</p>
            </div>
            <div class="hidden md:block text-right text-[10px] text-gray-400 font-mono">
                v2.3: Conventional Unit Cell
            </div>
        </div>

        <!-- 3D Viewport Area -->
        <div id="canvas-container" class="bg-slate-50 relative">
            <div class="absolute top-4 left-4 z-10 flex gap-2">
                <div class="bg-white/80 px-3 py-1 rounded-full text-[10px] font-bold text-gray-600 shadow-sm flex items-center gap-1 uppercase tracking-tighter">
                    <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path></svg>
                    Orbit / Zoom
                </div>
            </div>
            
            <!-- Statistics Overlay -->
            <div class="absolute bottom-4 left-4 right-4 z-10 grid grid-cols-3 gap-2 md:gap-4">
                <div class="info-card p-2 md:p-4 rounded-xl shadow-md border border-gray-100">
                    <span class="text-[9px] uppercase tracking-wider text-gray-400 block font-bold">Type</span>
                    <span id="stat-type" class="text-sm md:text-lg font-bold text-blue-600">BCC</span>
                </div>
                <div class="info-card p-2 md:p-4 rounded-xl shadow-md border border-gray-100">
                    <span class="text-[9px] uppercase tracking-wider text-gray-400 block font-bold">Basis Vectors</span>
                    <span id="stat-basis" class="text-sm md:text-lg font-bold text-gray-800">3</span>
                </div>
                <div class="info-card p-2 md:p-4 rounded-xl shadow-md border border-gray-100">
                    <span class="text-[9px] uppercase tracking-wider text-gray-400 block font-bold">Visible Atoms</span>
                    <span id="stat-atoms" class="text-sm md:text-lg font-bold text-gray-800">343</span>
                </div>
            </div>
        </div>

        <!-- Controls -->
        <div class="p-4 md:p-5 space-y-4">
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4 items-end">
                <!-- Lattice Selector -->
                <div class="space-y-2 md:col-span-1">
                    <label class="block text-xs font-bold text-gray-500 uppercase tracking-wide">Lattice Type</label>
                    <select id="lattice-select" class="w-full p-2 bg-gray-50 border border-gray-200 rounded-lg text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none transition-all font-semibold">
                        <option value="SC">Simple Cubic (SC)</option>
                        <option value="BCC" selected>Body-Centered (BCC)</option>
                        <option value="FCC">Face-Centered (FCC)</option>
                    </select>
                </div>

                <!-- Extent Slider -->
                <div class="space-y-2 md:col-span-1">
                    <div class="flex justify-between">
                        <label class="block text-xs font-bold text-gray-500 uppercase tracking-wide">Extent</label>
                        <span id="extent-value" class="text-xs font-bold text-blue-600 bg-blue-50 px-2 rounded">3</span>
                    </div>
                    <input type="range" id="extent-slider" min="1" max="5" value="3" class="w-full h-1.5 bg-gray-200 rounded-lg appearance-none cursor-pointer">
                </div>

                <!-- Atom Size Slider -->
                <div class="space-y-2 md:col-span-1">
                    <div class="flex justify-between">
                        <label class="block text-xs font-bold text-gray-500 uppercase tracking-wide">Atom Radius</label>
                        <span id="size-value" class="text-xs font-bold text-blue-600 bg-blue-50 px-2 rounded">0.12</span>
                    </div>
                    <input type="range" id="size-slider" min="0.05" max="0.5" step="0.01" value="0.12" class="w-full h-1.5 bg-gray-200 rounded-lg appearance-none cursor-pointer">
                </div>

                <!-- Toggles -->
                <div class="flex flex-col gap-2 md:col-span-1 justify-center pb-1">
                    <label class="flex items-center cursor-pointer">
                        <div class="relative">
                            <input type="checkbox" id="show-axes" class="sr-only" checked>
                            <div class="block bg-gray-200 w-8 h-5 rounded-full transition-colors toggle-bg"></div>
                            <div class="dot absolute left-1 top-1 bg-white w-3 h-3 rounded-full transition-transform transform translate-x-0 toggle-dot"></div>
                        </div>
                        <div class="ml-2 text-xs font-bold text-gray-600 uppercase">Show XYZ Axes</div>
                    </label>
                </div>
            </div>

            <!-- Vector Display -->
            <div class="p-3 bg-slate-900 rounded-xl text-blue-300 font-mono text-[10px] md:text-xs overflow-x-auto border border-slate-700 shadow-inner">
                <div id="vector-formula" class="grid grid-cols-3 gap-2">
                    <!-- Formulas will be injected here -->
                </div>
            </div>
        </div>
    </div>

    <style>
        input:checked ~ .toggle-bg { background-color: #3b82f6; }
        input:checked ~ .dot { transform: translateX(100%); }
    </style>

    <script>
        let scene, camera, renderer, controls;
        let atomsGroup, vectorsGroup, gridGroup, axesGroup;
        const a = 2.0; 

        const LATTICES = {
            SC: {
                getVectors: (a) => [new THREE.Vector3(a, 0, 0), new THREE.Vector3(0, a, 0), new THREE.Vector3(0, 0, a)],
                formula: ["a₁ = a x̂", "a₂ = a ŷ", "a₃ = a ẑ"]
            },
            BCC: {
                getVectors: (a) => [
                    new THREE.Vector3(-a/2, a/2, a/2),
                    new THREE.Vector3(a/2, -a/2, a/2),
                    new THREE.Vector3(a/2, a/2, -a/2)
                ],
                formula: ["a₁ = a/2 (-x̂+ŷ+ẑ)", "a₂ = a/2 (x̂-ŷ+ẑ)", "a₃ = a/2 (x̂+ŷ-ẑ)"]
            },
            FCC: {
                getVectors: (a) => [
                    new THREE.Vector3(0, a/2, a/2),
                    new THREE.Vector3(a/2, 0, a/2),
                    new THREE.Vector3(a/2, a/2, 0)
                ],
                formula: ["a₁ = a/2 (ŷ+ẑ)", "a₂ = a/2 (ẑ+x̂)", "a₃ = a/2 (x̂+ŷ)"]
            }
        };

        // Helper to create text labels for the axes and vectors
        function createTextSprite(text, color, scale = 1.5) {
            const canvas = document.createElement('canvas');
            canvas.width = 128; 
            canvas.height = 128;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = color;
            ctx.font = 'bold 70px sans-serif'; 
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText(text, 64, 64);
            const texture = new THREE.CanvasTexture(canvas);
            const spriteMat = new THREE.SpriteMaterial({ map: texture, depthTest: false });
            const sprite = new THREE.Sprite(spriteMat);
            sprite.scale.set(scale, scale, scale);
            return sprite;
        }

        function init() {
            const container = document.getElementById('canvas-container');
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0xf1f5f9);

            camera = new THREE.PerspectiveCamera(50, container.clientWidth / container.clientHeight, 0.1, 1000);
            camera.position.set(10, 8, 14);

            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(container.clientWidth, container.clientHeight);
            renderer.setPixelRatio(window.devicePixelRatio);
            container.appendChild(renderer.domElement);

            controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true;

            scene.add(new THREE.AmbientLight(0xffffff, 0.7));
            const dirLight = new THREE.DirectionalLight(0xffffff, 0.6);
            dirLight.position.set(10, 20, 10);
            scene.add(dirLight);

            atomsGroup = new THREE.Group();
            vectorsGroup = new THREE.Group();
            gridGroup = new THREE.Group();
            axesGroup = new THREE.Group();
            
            scene.add(atomsGroup, vectorsGroup, gridGroup, axesGroup);

            // --- Setup Cartesian Axes (XYZ) ---
            const axisLength = 6;
            const helper = new THREE.AxesHelper(axisLength);
            axesGroup.add(helper);

            // XYZ Labels
            const xLabel = createTextSprite('X', '#ef4444'); xLabel.position.set(axisLength + 0.5, 0, 0); axesGroup.add(xLabel);
            const yLabel = createTextSprite('Y', '#22c55e'); yLabel.position.set(0, axisLength + 0.5, 0); axesGroup.add(yLabel);
            const zLabel = createTextSprite('Z', '#3b82f6'); zLabel.position.set(0, 0, axisLength + 0.5); axesGroup.add(zLabel);

            // Negative dashed lines for XYZ
            const dashMat = new THREE.LineDashedMaterial({ color: 0x9ca3af, dashSize: 0.2, gapSize: 0.2 });
            const addDashed = (p1, p2) => {
                const geom = new THREE.BufferGeometry().setFromPoints([p1, p2]);
                const line = new THREE.Line(geom, dashMat);
                line.computeLineDistances();
                axesGroup.add(line);
            };
            addDashed(new THREE.Vector3(0,0,0), new THREE.Vector3(-axisLength,0,0));
            addDashed(new THREE.Vector3(0,0,0), new THREE.Vector3(0,-axisLength,0));
            addDashed(new THREE.Vector3(0,0,0), new THREE.Vector3(0,0,-axisLength));

            updateSimulation();
            window.addEventListener('resize', onWindowResize);
            animate();
        }

        function onWindowResize() {
            const container = document.getElementById('canvas-container');
            camera.aspect = container.clientWidth / container.clientHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(container.clientWidth, container.clientHeight);
        }

        function updateSimulation() {
            const typeKey = document.getElementById('lattice-select').value;
            const extent = parseInt(document.getElementById('extent-slider').value);
            const atomRadius = parseFloat(document.getElementById('size-slider').value);
            const showAxes = document.getElementById('show-axes').checked;
            
            const lattice = LATTICES[typeKey];
            const vectors = lattice.getVectors(a);

            // Update Visibility of XYZ Axes
            axesGroup.visible = showAxes;

            // Cleanup
            while(atomsGroup.children.length > 0) atomsGroup.remove(atomsGroup.children[0]);
            while(vectorsGroup.children.length > 0) vectorsGroup.remove(vectorsGroup.children[0]);
            while(gridGroup.children.length > 0) gridGroup.remove(gridGroup.children[0]);

            const sphereGeometry = new THREE.SphereGeometry(atomRadius, 16, 16);
            const atomMat = new THREE.MeshPhongMaterial({ color: 0x1e40af, shininess: 80 });
            const originMat = new THREE.MeshPhongMaterial({ color: 0xef4444, shininess: 100 });

            let atomCount = 0;
            for (let n1 = -extent; n1 <= extent; n1++) {
                for (let n2 = -extent; n2 <= extent; n2++) {
                    for (let n3 = -extent; n3 <= extent; n3++) {
                        const pos = new THREE.Vector3()
                            .addScaledVector(vectors[0], n1)
                            .addScaledVector(vectors[1], n2)
                            .addScaledVector(vectors[2], n3);

                        const isOrigin = n1 === 0 && n2 === 0 && n3 === 0;
                        const atom = new THREE.Mesh(sphereGeometry, isOrigin ? originMat : atomMat);
                        atom.position.copy(pos);
                        atomsGroup.add(atom);
                        atomCount++;
                    }
                }
            }

            // Primitive Vectors (Cyan, Fuchsia, Amber)
            const vectorColorsHex = [0x06b6d4, 0xd946ef, 0xf59e0b];
            const vectorColorsStr = ['#06b6d4', '#d946ef', '#f59e0b']; 
            const vectorLabelsText = ['a₁', 'a₂', 'a₃'];

            vectors.forEach((v, i) => {
                const arrow = new THREE.ArrowHelper(v.clone().normalize(), new THREE.Vector3(0,0,0), v.length(), vectorColorsHex[i], 0.4, 0.2);
                vectorsGroup.add(arrow);

                const labelSprite = createTextSprite(vectorLabelsText[i], vectorColorsStr[i], 1.2);
                labelSprite.position.copy(v).multiplyScalar(1.25);
                vectorsGroup.add(labelSprite);
            });

            // --- CONVENTIONAL UNIT CELL GRID BOX ---
            // Create a box of size exactly 'a' (the lattice constant)
            const boxSize = a;
            const boxGeom = new THREE.BoxGeometry(boxSize, boxSize, boxSize);
            
            // Translate the geometry so that its bottom-left-back corner sits exactly at (0,0,0)
            // This perfectly aligns the box boundaries with the atoms for SC, BCC, and FCC
            boxGeom.translate(boxSize / 2, boxSize / 2, boxSize / 2);
            
            const edges = new THREE.EdgesGeometry(boxGeom);
            const line = new THREE.LineSegments(edges, new THREE.LineBasicMaterial({ 
                color: 0x334155, // Dark slate gray for better visibility
                transparent: true, 
                opacity: 0.6 
            }));
            gridGroup.add(line);

            // UI Update
            document.getElementById('stat-type').innerText = typeKey;
            document.getElementById('stat-atoms').innerText = atomCount;
            document.getElementById('extent-value').innerText = extent;
            document.getElementById('size-value').innerText = atomRadius.toFixed(2);
            document.getElementById('vector-formula').innerHTML = lattice.formula.map((f, i) => {
                const textClasses = ['text-cyan-400', 'text-fuchsia-400', 'text-amber-400'];
                return `<div class="bg-slate-800/50 p-1 px-2 rounded text-center ${textClasses[i]} font-bold">${f}</div>`;
            }).join('');
        }

        function animate() {
            requestAnimationFrame(animate);
            controls.update();
            renderer.render(scene, camera);
        }

        // Listeners
        document.getElementById('lattice-select').addEventListener('change', updateSimulation);
        document.getElementById('extent-slider').addEventListener('input', updateSimulation);
        document.getElementById('size-slider').addEventListener('input', updateSimulation);
        document.getElementById('show-axes').addEventListener('change', updateSimulation);

        window.onload = init;
    </script>
</body>
</html>



# Three.js Implementation Guide For LLMs

## 1. Core Understanding Framework

This document provides a structured approach for Large Language Models (LLMs) to understand, reason about, and implement Three.js animations and 3D graphics in web development projects. It establishes a systematic decision-making framework across various implementation contexts.

### 1.1 Library Definition

Three.js is a JavaScript library that creates and displays 3D graphics in a web browser using WebGL. It provides a high-level abstraction over WebGL, making 3D graphics more accessible to web developers. Version 162 is the current stable release as of May 2025.

### 1.2 Implementation Context Parameters

When implementing Three.js, reason through these parameters sequentially:

1. **Hardware Target:**
   - LOW = Low-end devices, minimal GPU capabilities, mobile phones
   - MEDIUM = Average devices, integrated graphics, mid-range laptops
   - HIGH = High-end devices, dedicated GPUs, gaming computers

2. **Platform Context:**
   - SERVER-RENDERED = PHP, Laravel, or other server-rendered frameworks with client-side JS
   - MODERN-FRAMEWORK = React, Vue, Astro frameworks
   - STATIC = Plain HTML/JS implementation

3. **Scene Complexity Matrix:**
   - SIMPLE = Basic 3D objects, minimal lighting, static scenes (Level 1)
   - INTERMEDIATE = Multiple objects, animations, basic interactions (Level 2)
   - ADVANCED = Complex models, physics, particles, post-processing (Level 3)

4. **Hosting Environment Constraints:**
   - SHARED_HOSTING = Limited resources, bandwidth concerns, asset size critical
   - DEDICATED = More resources available, optimization recommended
   - CLOUD = Scalable resources, standard optimization

## 2. Library Technical Framework

### 2.1 Architectural Model

Three.js employs a modular, object-oriented architecture organized around core components for building and rendering 3D scenes. The library follows a scene graph pattern where objects form a hierarchical tree structure.

#### 2.1.1 Core Components:

```
Scene → Container for all 3D objects, lights, and cameras
Camera → Defines the viewpoint (PerspectiveCamera, OrthographicCamera)
Renderer → Draws the scene (WebGLRenderer, CSS3DRenderer)
Mesh → 3D object (geometry + material)
Geometry → Defines the shape (BoxGeometry, SphereGeometry, custom)
Material → Defines the appearance (MeshBasicMaterial, MeshStandardMaterial)
Light → Illuminates objects (DirectionalLight, PointLight, AmbientLight)
Controls → Manages user interactions (OrbitControls, TrackballControls)
Loader → Imports 3D models and assets (GLTFLoader, TextureLoader)
```

### 2.2 Feature-Decision Matrix

When implementing Three.js, apply this decision matrix to determine which features to utilize:

| Context Parameter | Feature Selection | Implementation Approach |
|-------------------|------------------|-------------------------|
| LOW + SHARED_HOSTING | Basic renderer, simple geometries | Minimize draw calls, low-poly models, avoid post-processing |
| MEDIUM + SHARED_HOSTING | Standard renderer, optimized models | Balance quality and performance, moderate draw calls |
| HIGH + ANY_HOSTING | Advanced renderer, complex scenes | Enable post-processing, shadows, high-poly models |
| ANY + SERVER-RENDERED | Load detection | Initialize Three.js after DOM is ready |
| ANY + MODERN-FRAMEWORK | Component lifecycle hooks | Initialize in mount/effect hooks, handle cleanup |

### 2.3 Module Selection Tree

Follow this decision tree to select appropriate modules:

1. **Basic 3D Scene Required Only?**
   - YES → Import { Scene, PerspectiveCamera, WebGLRenderer, Mesh } only
   - NO → Continue to step 2

2. **Lighting and Materials Needed?**
   - YES → Add { DirectionalLight, AmbientLight, MeshStandardMaterial }
   - NO → Use MeshBasicMaterial (no lighting required)

3. **User Interaction Required?**
   - YES → Add { OrbitControls } or appropriate controls
   - NO → Skip controls

4. **Loading External 3D Models?**
   - YES → Add appropriate loaders { GLTFLoader, FBXLoader, etc. }
   - NO → Use built-in geometries

5. **Advanced Visual Effects?**
   - YES → Add { EffectComposer, post-processing modules }
   - NO → Skip post-processing

### 2.4 Performance Analysis Dataset

Three.js performance characteristics can be modeled through these quantifiable metrics:

```json
{
  "performance_metrics": {
    "fps_capability": {
      "low_complexity": "60fps on medium-end devices, 30-60fps on low-end",
      "medium_complexity": "60fps on high-end, 30-40fps on medium, <30fps on low-end",
      "high_complexity": "60fps on high-end with GPU, <30fps on medium, not recommended for low-end"
    },
    "object_thresholds": {
      "safe_threshold": "50-100 simple objects or 10K-50K triangles",
      "medium_threshold": "100-500 objects or 50K-200K triangles",
      "upper_threshold": "500-1000 objects or 200K-1M triangles (with optimization)"
    },
    "bundle_size": {
      "core_import": "~150KB (minified core library)",
      "typical_import": "200-300KB (core + common modules)",
      "full_library": "500KB+ (all features + common loaders)",
      "asset_consideration": "3D models often 1-10MB+ (must be optimized)"
    }
  }
}
```

### 2.5 Library Selection Algorithm

When determining whether to use Three.js versus alternatives, apply this algorithm:

1. **Input:** Project requirements, target devices, graphics complexity
2. **Process:**
   ```
   IF primary_requirement == '2D_ANIMATIONS' THEN
     recommended_library = "GSAP or Anime.js"
   ELSE IF primary_requirement == 'SIMPLE_UI_TRANSITIONS' THEN
     recommended_library = "CSS animations or Motion One"
   ELSE IF primary_requirement == '3D_GRAPHICS' THEN
     IF scene_complexity == HIGH AND performance_critical == TRUE THEN
       recommended_library = "CUSTOM_WEBGL or THREE.JS_WITH_OPTIMIZATION"
     ELSE IF target_audience == "MOBILE_PRIMARILY" AND optimization_resources == LIMITED THEN
       recommended_library = "THREE.JS_WITH_STRICT_OPTIMIZATION or CONSIDER_2D_ALTERNATIVE"
     ELSE
       recommended_library = "THREE.JS"
   ELSE IF primary_requirement == '2D_GAMES' THEN
     recommended_library = "PIXI.JS or CANVAS_API"
   ELSE IF primary_requirement == '3D_GAMES' THEN
     recommended_library = "THREE.JS or BABYLON.JS"
   END IF
   ```
3. **Output:** Recommended graphics library

Rationale: Three.js is ideal for 3D graphics on the web, but may be overkill for 2D-only animations or simple UI transitions. Performance considerations are critical, especially for mobile devices.

## 3. Implementation Process Model

### 3.1 Installation Decision Tree

```
START
├── Is the project using a build system with NPM/Yarn?
│   ├── YES
│   │   └── INSTALL METHOD: npm install three
│   └── NO
│       ├── Is project on shared hosting with bandwidth concerns?
│       │   ├── YES
│       │   │   └── INSTALL METHOD: CDN with version lock
│       │   │       └── <script src="https://cdn.jsdelivr.net/npm/three@0.162.0/build/three.min.js"></script>
│       │   └── NO
│       │       └── INSTALL METHOD: Download and self-host
│       │           └── Download from https://github.com/mrdoob/three.js/releases
└── END
```

### 3.2 Framework Integration Models

Each framework requires specific integration patterns. LLMs should follow these implementation templates:

#### 3.2.1 React Implementation Template

```javascript
// Step 1: Import required modules from Three.js
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { useEffect, useRef, useState } from 'react';

// Step 2: Create component with Three.js logic
function ThreeScene() {
  // Step 2.1: Create reference to DOM element
  const mountRef = useRef(null);
  // Step 2.2: Create state for any animation properties (optional)
  const [isAnimating, setIsAnimating] = useState(true);
  
  // Step 2.3: Initialize scene in useEffect hook
  useEffect(() => {
    // Step 2.4: Create Three.js scene, camera, renderer
    const width = mountRef.current.clientWidth;
    const height = mountRef.current.clientHeight;
    
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
    camera.position.z = 5;
    
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(width, height);
    renderer.setPixelRatio(window.devicePixelRatio);
    mountRef.current.appendChild(renderer.domElement);
    
    // Step 2.5: Add meshes, lights, etc.
    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);
    
    // Add lighting
    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(0, 1, 2);
    scene.add(light);
    
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);
    
    // Step 2.6: Add controls if needed
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    
    // Step 2.7: Animation loop
    let animationId;
    const animate = () => {
      animationId = requestAnimationFrame(animate);
      
      if (isAnimating) {
        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;
      }
      
      controls.update(); // Only needed if controls.enableDamping = true
      renderer.render(scene, camera);
    };
    
    animate();
    
    // Step 2.8: Handle window resize
    const handleResize = () => {
      const width = mountRef.current.clientWidth;
      const height = mountRef.current.clientHeight;
      
      camera.aspect = width / height;
      camera.updateProjectionMatrix();
      renderer.setSize(width, height);
    };
    
    window.addEventListener('resize', handleResize);
    
    // Step 2.9: Cleanup function
    return () => {
      window.removeEventListener('resize', handleResize);
      mountRef.current.removeChild(renderer.domElement);
      cancelAnimationFrame(animationId);
      scene.dispose();
      geometry.dispose();
      material.dispose();
    };
  }, [isAnimating]); // Only re-run if animation state changes
  
  // Step 3: Return container div with ref
  return (
    <div>
      <div 
        ref={mountRef} 
        style={{ width: '100%', height: '400px' }}
      />
      <button onClick={() => setIsAnimating(!isAnimating)}>
        {isAnimating ? 'Pause' : 'Play'}
      </button>
    </div>
  );
}

export default ThreeScene;
```

#### 3.2.2 Vue Implementation Template

```javascript
<template>
  <!-- Step 1: Create container element with ref -->
  <div>
    <div ref="threeContainer" class="three-container"></div>
    <button @click="toggleAnimation">{{ isAnimating ? 'Pause' : 'Play' }}</button>
  </div>
</template>

<script>
// Step 2: Import Three.js modules
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

export default {
  name: 'ThreeScene',
  
  // Step 3: Setup component data
  data() {
    return {
      scene: null,
      camera: null,
      renderer: null,
      cube: null,
      controls: null,
      animationId: null,
      isAnimating: true
    };
  },
  
  // Step 4: Initialize Three.js in mounted lifecycle hook
  mounted() {
    this.initThree();
    window.addEventListener('resize', this.handleResize);
  },
  
  // Step 5: Clean up in beforeUnmount
  beforeUnmount() {
    window.removeEventListener('resize', this.handleResize);
    this.cleanup();
  },
  
  methods: {
    // Step 6: Initialize Three.js scene
    initThree() {
      const container = this.$refs.threeContainer;
      const width = container.clientWidth;
      const height = container.clientHeight;
      
      // Create scene
      this.scene = new THREE.Scene();
      
      // Create camera
      this.camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
      this.camera.position.z = 5;
      
      // Create renderer
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(width, height);
      this.renderer.setPixelRatio(window.devicePixelRatio);
      container.appendChild(this.renderer.domElement);
      
      // Add mesh
      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
      this.cube = new THREE.Mesh(geometry, material);
      this.scene.add(this.cube);
      
      // Add lighting
      const light = new THREE.DirectionalLight(0xffffff, 1);
      light.position.set(0, 1, 2);
      this.scene.add(light);
      
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
      this.scene.add(ambientLight);
      
      // Add controls
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.enableDamping = true;
      
      // Start animation loop
      this.animate();
    },
    
    // Step 7: Animation loop
    animate() {
      this.animationId = requestAnimationFrame(this.animate);
      
      if (this.isAnimating) {
        this.cube.rotation.x += 0.01;
        this.cube.rotation.y += 0.01;
      }
      
      this.controls.update();
      this.renderer.render(this.scene, this.camera);
    },
    
    // Step 8: Handle window resize
    handleResize() {
      const container = this.$refs.threeContainer;
      const width = container.clientWidth;
      const height = container.clientHeight;
      
      this.camera.aspect = width / height;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(width, height);
    },
    
    // Step 9: Toggle animation state
    toggleAnimation() {
      this.isAnimating = !this.isAnimating;
    },
    
    // Step 10: Cleanup resources
    cleanup() {
      cancelAnimationFrame(this.animationId);
      
      // Dispose geometries and materials
      this.cube.geometry.dispose();
      this.cube.material.dispose();
      
      // Remove renderer from DOM
      const container = this.$refs.threeContainer;
      container.removeChild(this.renderer.domElement);
      
      // Dispose renderer
      this.renderer.dispose();
    }
  }
};
</script>

<style scoped>
.three-container {
  width: 100%;
  height: 400px;
}
</style>
```

#### 3.2.3 Server-Rendered (PHP/Laravel) Implementation Template

```php
<!-- Step 1: Create unique container element -->
<div id="three-container-<?php echo $uniqueId; ?>" class="three-scene" style="width: 100%; height: 400px;"></div>

<!-- Step 2: Include Three.js from CDN -->
<script src="https://cdn.jsdelivr.net/npm/three@0.162.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.162.0/examples/js/controls/OrbitControls.js"></script>

<!-- Step 3: Initialize Three.js after DOM loads -->
<script>
  document.addEventListener('DOMContentLoaded', () => {
    // Step 3.1: Get container and dimensions
    const containerId = 'three-container-<?php echo $uniqueId; ?>';
    const container = document.getElementById(containerId);
    const width = container.clientWidth;
    const height = container.clientHeight;
    
    // Step 3.2: Create Three.js scene
    const scene = new THREE.Scene();
    
    // Step 3.3: Create camera
    const camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
    camera.position.z = 5;
    
    // Step 3.4: Create renderer
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(width, height);
    renderer.setPixelRatio(window.devicePixelRatio);
    container.appendChild(renderer.domElement);
    
    // Step 3.5: Add mesh
    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);
    
    // Step 3.6: Add lighting
    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(0, 1, 2);
    scene.add(light);
    
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);
    
    // Step 3.7: Add controls
    const controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    
    // Step 3.8: Animation loop
    let isAnimating = true;
    
    function animate() {
      requestAnimationFrame(animate);
      
      if (isAnimating) {
        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;
      }
      
      controls.update();
      renderer.render(scene, camera);
    }
    
    animate();
    
    // Step 3.9: Handle window resize
    function handleResize() {
      const width = container.clientWidth;
      const height = container.clientHeight;
      
      camera.aspect = width / height;
      camera.updateProjectionMatrix();
      renderer.setSize(width, height);
    }
    
    window.addEventListener('resize', handleResize);
    
    // Step 3.10: Add toggle button (optional)
    const button = document.createElement('button');
    button.textContent = 'Pause';
    button.style.position = 'absolute';
    button.style.bottom = '10px';
    button.style.left = '10px';
    container.appendChild(button);
    
    button.addEventListener('click', () => {
      isAnimating = !isAnimating;
      button.textContent = isAnimating ? 'Pause' : 'Play';
    });
  });
</script>
```

#### 3.2.4 Astro Implementation Template

```astro
---
// Step 1: Set up component properties
const { id = 'three-scene', height = '400px' } = Astro.props;
---

<!-- Step 2: Create container element -->
<div id={id} class="three-scene" style={`width: 100%; height: ${height};`}></div>

<!-- Step 3: Add client-side script -->
<script>
  // Step 3.1: Import Three.js modules
  import * as THREE from 'three';
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
  
  // Step 3.2: Initialize scene after DOM is ready
  document.addEventListener('DOMContentLoaded', () => {
    const sceneElements = document.querySelectorAll('.three-scene');
    
    // Initialize each Three.js scene on the page
    sceneElements.forEach(container => {
      initThreeScene(container);
    });
    
    function initThreeScene(container) {
      // Get dimensions
      const width = container.clientWidth;
      const height = container.clientHeight;
      
      // Create scene
      const scene = new THREE.Scene();
      
      // Create camera
      const camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
      camera.position.z = 5;
      
      // Create renderer
      const renderer = new THREE.WebGLRenderer({ antialias: true });
      renderer.setSize(width, height);
      renderer.setPixelRatio(window.devicePixelRatio);
      container.appendChild(renderer.domElement);
      
      // Add mesh
      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
      const cube = new THREE.Mesh(geometry, material);
      scene.add(cube);
      
      // Add lighting
      const light = new THREE.DirectionalLight(0xffffff, 1);
      light.position.set(0, 1, 2);
      scene.add(light);
      
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
      scene.add(ambientLight);
      
      // Add controls
      const controls = new OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true;
      
      // Animation state
      let isAnimating = true;
      
      // Animation loop
      function animate() {
        requestAnimationFrame(animate);
        
        if (isAnimating) {
          cube.rotation.x += 0.01;
          cube.rotation.y += 0.01;
        }
        
        controls.update();
        renderer.render(scene, camera);
      }
      
      animate();
      
      // Handle window resize
      function handleResize() {
        const width = container.clientWidth;
        const height = container.clientHeight;
        
        camera.aspect = width / height;
        camera.updateProjectionMatrix();
        renderer.setSize(width, height);
      }
      
      window.addEventListener('resize', handleResize);
      
      // Clean up on page navigation or component removal
      document.addEventListener('astro:before-swap', () => {
        window.removeEventListener('resize', handleResize);
        renderer.dispose();
        geometry.dispose();
        material.dispose();
      });
      
      // Add toggle button
      const button = document.createElement('button');
      button.textContent = 'Pause';
      button.style.position = 'absolute';
      button.style.bottom = '10px';
      button.style.left = '10px';
      container.appendChild(button);
      
      button.addEventListener('click', () => {
        isAnimating = !isAnimating;
        button.textContent = isAnimating ? 'Pause' : 'Play';
      });
    }
  });
</script>
```

### 3.3 Optimization Algorithm for Performance Tuning

LLMs should follow this deterministic algorithm when implementing performance optimizations:

```javascript
function determineOptimizationStrategy(context) {
  // Initialize optimization array
  const optimizations = [];
  
  // Step 1: Always use renderer optimization
  optimizations.push({
    type: "RENDERER_OPTIMIZATION",
    implementation: "Configure WebGLRenderer for optimal performance",
    example: `
      // Create renderer with appropriate settings
      const renderer = new THREE.WebGLRenderer({ 
        antialias: context.hardwareTarget !== "LOW", // Disable for low-end
        powerPreference: "high-performance",
        precision: context.hardwareTarget === "LOW" ? "mediump" : "highp"
      });
      
      // Set pixel ratio (limit for low-end devices)
      renderer.setPixelRatio(
        context.hardwareTarget === "LOW" 
          ? Math.min(1, window.devicePixelRatio) 
          : window.devicePixelRatio
      );
      
      // Only enable shadows on medium+ hardware
      renderer.shadowMap.enabled = context.hardwareTarget !== "LOW";
      if (renderer.shadowMap.enabled) {
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
      }
    `
  });
  
  // Step 2: Apply geometry optimizations
  optimizations.push({
    type: "GEOMETRY_OPTIMIZATION",
    implementation: "Optimize geometries and meshes",
    example: `
      // Use BufferGeometry for all geometries
      const geometry = new THREE.BoxBufferGeometry(1, 1, 1);
      
      // Merge geometries when possible
      if (context.sceneComplexity === "ADVANCED" && hasMultipleSimilarObjects) {
        const mergedGeometry = BufferGeometryUtils.mergeBufferGeometries(geometries);
        const mergedMesh = new THREE.Mesh(mergedGeometry, material);
        scene.add(mergedMesh);
      }
      
      // For static objects, disable matrix updates
      if (!isAnimated) {
        mesh.matrixAutoUpdate = false;
        mesh.updateMatrix();
      }
    `
  });
  
  // Step 3: Analyze hardware target and adjust accordingly
  if (context.hardwareTarget === "LOW") {
    optimizations.push({
      type: "LOW_END_OPTIMIZATIONS",
      implementation: "Apply aggressive optimizations for low-end devices",
      techniques: [
        {
          name: "Reduce Polygon Count",
          example: `
            // Use lower-poly geometries
            const sphereGeometry = new THREE.SphereGeometry(1, 16, 12); // Reduced segments
            
            // Simplify loaded models
            const simplifyModifier = new SimplifyModifier();
            const simplified = simplifyModifier.modify(geometry, Math.floor(geometry.attributes.position.count * 0.5));
          `
        },
        {
          name: "Disable Effects",
          example: `
            // No post-processing
            // No reflections
            // Simpler materials
            const material = new THREE.MeshLambertMaterial({ color: 0xff0000 }); // Instead of MeshStandardMaterial
          `
        },
        {
          name: "Reduce Draw Calls",
          example: `
            // Implement level-of-detail (LOD)
            const lod = new THREE.LOD();
            
            const highDetailGeometry = new THREE.SphereGeometry(1, 32, 24);
            const mediumDetailGeometry = new THREE.SphereGeometry(1, 16, 12);
            const lowDetailGeometry = new THREE.SphereGeometry(1, 8, 6);
            
            lod.addLevel(new THREE.Mesh(highDetailGeometry, material), 0);    // High detail at close distance
            lod.addLevel(new THREE.Mesh(mediumDetailGeometry, material), 10); // Medium detail at medium distance
            lod.addLevel(new THREE.Mesh(lowDetailGeometry, material), 50);    // Low detail at far distance
            
            scene.add(lod);
          `
        }
      ]
    });
  }
  
  // Step 4: Add texture and material optimizations
  optimizations.push({
    type: "TEXTURE_OPTIMIZATION",
    implementation: "Optimize textures and materials",
    example: `
      // Resize textures based on device capability
      const textureLoader = new THREE.TextureLoader();
      const texturePath = context.hardwareTarget === "LOW" 
        ? 'texture-512.jpg'  // Smaller texture for low-end
        : (context.hardwareTarget === "MEDIUM" 
            ? 'texture-1024.jpg'  // Medium texture
            : 'texture-2048.jpg'  // Full size for high-end
          );
      
      const texture = textureLoader.load(texturePath);
      
      // Ensure proper texture settings
      texture.generateMipmaps = true;
      texture.minFilter = THREE.LinearMipmapLinearFilter;
      texture.magFilter = THREE.LinearFilter;
      
      // Use appropriate material based on hardware
      const material = context.hardwareTarget === "LOW"
        ? new THREE.MeshLambertMaterial({ map: texture }) // Simpler lighting model
        : new THREE.MeshStandardMaterial({    // More complex, physically-based
            map: texture,
            roughness: 0.7,
            metalness: 0.2
          });
    `
  });
  
  // Step 5: Add asset loading optimizations
  optimizations.push({
    type: "ASSET_LOADING_OPTIMIZATION",
    implementation: "Optimize asset loading and processing",
    example: `
      // Use a loading manager to track progress
      const loadingManager = new THREE.LoadingManager();
      loadingManager.onProgress = (url, loaded, total) => {
        console.log(\`Loading: \${Math.round(loaded / total * 100)}%\`);
      };
      
      // Implement asynchronous loading
      async function loadAssets() {
        // Show loading indicator
        showLoader();
        
        // Load high-priority assets first
        await loadCriticalAssets();
        
        // Initial scene can now be displayed
        hideLoader();
        showScene();
        
        // Load remaining assets in background
        loadRemainingAssets().then(() => {
          // Update scene with all assets
          updateSceneWithFullAssets();
        });
      }
      
      // Implement DRACO compression for models if appropriate
      const dracoLoader = new DRACOLoader();
      dracoLoader.setDecoderPath('draco/');
      
      const gltfLoader = new GLTFLoader();
      gltfLoader.setDRACOLoader(dracoLoader);
    `
  });
  
  // Step 6: Add rendering optimizations
  optimizations.push({
    type: "RENDERING_OPTIMIZATION",
    implementation: "Optimize the rendering process",
    example: `
      // Use frustum culling
      const frustum = new THREE.Frustum();
      
      function updateScene() {
        // Update frustum
        camera.updateMatrix();
        camera.updateMatrixWorld();
        frustum.setFromProjectionMatrix(
          new THREE.Matrix4().multiplyMatrices(
            camera.projectionMatrix, 
            camera.matrixWorldInverse
          )
        );
        
        // Check if objects are in frustum
        objects.forEach(object => {
          if (!object.boundingSphere) {
            object.geometry.computeBoundingSphere();
            object.boundingSphere = object.geometry.boundingSphere.clone();
          }
          
          // Update bounding sphere position
          const objectWorldPosition = new THREE.Vector3();
          object.getWorldPosition(objectWorldPosition);
          object.boundingSphere.center.copy(objectWorldPosition);
          
          // Toggle visibility based on frustum
          object.visible = frustum.intersectsSphere(object.boundingSphere);
        });
      }
      
      // Implement adaptive frame rates if needed
      let frameRateTarget = context.hardwareTarget === "LOW" ? 30 : 60;
      let lastTime = 0;
      const frameInterval = 1000 / frameRateTarget;
      
      function animateWithThrottle(time) {
        requestAnimationFrame(animateWithThrottle);
        
        const delta = time - lastTime;
        if (delta < frameInterval) return;
        
        // Adjust for time elapsed (for consistent animation speed)
        const timeScale = delta / frameInterval;
        
        // Update animations with timeScale
        updateScene();
        renderer.render(scene, camera);
        
        lastTime = time;
      }
      
      // Start animation loop
      requestAnimationFrame(animateWithThrottle);
    `
  });
  
  return optimizations;
}
```

### 3.4 Device Capability Detection

Use this code to determine device capability and adjust scene complexity accordingly:

```javascript
function detectDeviceCapability() {
  // Create performance detection object
  const deviceCapability = {
    tier: "UNKNOWN",
    canRun3D: false,
    maxTextureSize: 1024,
    recommendedPolygons: 0,
    supportsWebGL2: false,
    antialiasing: false
  };
  
  // Step 1: Check for WebGL support
  try {
    const canvas = document.createElement('canvas');
    const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl');
    
    if (!gl) {
      deviceCapability.tier = "UNSUPPORTED";
      deviceCapability.canRun3D = false;
      return deviceCapability;
    }
    
    // Step 2: Try WebGL 2
    const gl2 = canvas.getContext('webgl2');
    deviceCapability.supportsWebGL2 = !!gl2;
    
    // Step 3: Check for baseline performance indicators
    const maxTextureSize = gl.getParameter(gl.MAX_TEXTURE_SIZE);
    const maxRenderbufferSize = gl.getParameter(gl.MAX_RENDERBUFFER_SIZE);
    const maxViewportDims = gl.getParameter(gl.MAX_VIEWPORT_DIMS);
    const memory = navigator.deviceMemory || 4; // Default to middle value if not available
    const processors = navigator.hardwareConcurrency || 4;
    const isReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
    
    // Step 4: Check mobile or desktop
    const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
    
    // Step 5: Run quick benchmark (simplified)
    const benchmark = () => {
      const startTime = performance.now();
      
      // Create a temporary larger scene to test performance
      const tempRenderer = new THREE.WebGLRenderer({ canvas });
      const tempScene = new THREE.Scene();
      const tempCamera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000);
      
      // Add several cubes as a stress test
      for (let i = 0; i < 50; i++) {
        const geometry = new THREE.BoxGeometry(1, 1, 1);
        const material = new THREE.MeshBasicMaterial({ color: Math.random() * 0xffffff });
        const mesh = new THREE.Mesh(geometry, material);
        mesh.position.set(
          Math.random() * 10 - 5,
          Math.random() * 10 - 5,
          Math.random() * 10 - 5
        );
        tempScene.add(mesh);
      }
      
      // Render several frames and measure
      const frames = 10;
      const frameStartTime = performance.now();
      
      for (let i = 0; i < frames; i++) {
        tempRenderer.render(tempScene, tempCamera);
      }
      
      const frameEndTime = performance.now();
      const frameDuration = (frameEndTime - frameStartTime) / frames;
      
      // Clean up
      tempRenderer.dispose();
      tempScene.traverse(object => {
        if (object.geometry) object.geometry.dispose();
        if (object.material) object.material.dispose();
      });
      
      return {
        frameDuration,
        fps: 1000 / frameDuration
      };
    };
    
    const benchmarkResult = benchmark();
    
    // Step 6: Determine device tier
    if (isReducedMotion) {
      deviceCapability.tier = "LOW";
      deviceCapability.canRun3D = true;
      deviceCapability.recommendedPolygons = 10000;
      deviceCapability.antialiasing = false;
    } else if (
      isMobile || 
      memory <= 2 || 
      processors <= 2 || 
      maxTextureSize <= 2048 || 
      benchmarkResult.fps < 30
    ) {
      deviceCapability.tier = "LOW";
      deviceCapability.canRun3D = true;
      deviceCapability.recommendedPolygons = 50000;
      deviceCapability.maxTextureSize = Math.min(1024, maxTextureSize);
      deviceCapability.antialiasing = false;
    } else if (
      (memory <= 4 && processors <= 4) || 
      maxTextureSize <= 4096 || 
      benchmarkResult.fps < 50
    ) {
      deviceCapability.tier = "MEDIUM";
      deviceCapability.canRun3D = true;
      deviceCapability.recommendedPolygons = 200000;
      deviceCapability.maxTextureSize = Math.min(2048, maxTextureSize);
      deviceCapability.antialiasing = true;
    } else {
      deviceCapability.tier = "HIGH";
      deviceCapability.canRun3D = true;
      deviceCapability.recommendedPolygons = 1000000;
      deviceCapability.maxTextureSize = maxTextureSize;
      deviceCapability.antialiasing = true;
    }
    
    return deviceCapability;
  } catch (error) {
    console.error("Error detecting device capability:", error);
    deviceCapability.tier = "UNSUPPORTED";
    deviceCapability.canRun3D = false;
    return deviceCapability;
  }
}
```

## 4. Scene Implementation Models

### 4.1 Scene Type Classification System

LLMs should classify Three.js scene requirements using this taxonomy:

```json
{
  "scene_types": {
    "STATIC_MODEL_VIEWER": {
      "description": "Simple display of 3D models with basic interaction",
      "complexity": "SIMPLE",
      "use_case": "Product viewers, 3D asset showcase",
      "key_features": ["OrbitControls", "Environmental lighting", "Model loading"]
    },
    "INTERACTIVE_SCENE": {
      "description": "Scene with dynamic elements responding to user input",
      "complexity": "INTERMEDIATE",
      "use_case": "Interactive demonstrations, configurators",
      "key_features": ["Raycasting", "Dynamic properties", "Event listeners"]
    },
    "ANIMATED_VISUALIZATION": {
      "description": "Data visualization or storytelling with animation",
      "complexity": "INTERMEDIATE",
      "use_case": "Data visualization, animated infographics",
      "key_features": ["Animation system", "Data binding", "Camera transitions"]
    },
    "IMMERSIVE_EXPERIENCE": {
      "description": "Rich environment with multiple interactive elements",
      "complexity": "ADVANCED", 
      "use_case": "Virtual tours, promotional experiences",
      "key_features": ["Post-processing", "Physics", "Audio integration", "Loading management"]
    },
    "GAME_OR_SIMULATION": {
      "description": "Complex logic with game mechanics",
      "complexity": "ADVANCED",
      "use_case": "Web games, physics simulations, training",
      "key_features": ["Physics engine", "Collision detection", "State management", "Input systems"]
    }
  }
}
```

### 4.2 Implementation Templates By Scene Type

#### 4.2.1 STATIC_MODEL_VIEWER Template

```javascript
/**
 * Creates a 3D model viewer with orbit controls
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Scene controller
 */
function createModelViewer(options = {}) {
  // Step 1: Import required modules
  const THREE = window.THREE || {};
  
  // Step 2: Merge default options with provided options
  const config = {
    // Container
    container: options.container || document.getElementById('model-viewer'),
    
    // Model options
    modelPath: options.modelPath || null,
    modelType: options.modelType || 'gltf', // 'gltf', 'obj', 'fbx'
    
    // Display options
    backgroundColor: options.backgroundColor || 0xf5f5f5,
    enableShadows: options.enableShadows !== undefined ? options.enableShadows : true,
    autoRotate: options.autoRotate !== undefined ? options.autoRotate : false,
    
    // Camera options
    cameraPosition: options.cameraPosition || { x: 0, y: 1, z: 5 },
    cameraFOV: options.cameraFOV || 45,
    
    // Lighting options
    environmentMap: options.environmentMap || null,
    lightIntensity: options.lightIntensity || 1,
    
    // Loading options
    showLoadingIndicator: options.showLoadingIndicator !== undefined ? options.showLoadingIndicator : true,
    
    ...options
  };
  
  // Step 3: Create scene objects
  let scene, camera, renderer, controls, model, mixer;
  let clock = new THREE.Clock();
  let loadingManager, loadingScreen;
  
  // Step 4: Initialize scene
  function init() {
    // Step 4.1: Create scene
    scene = new THREE.Scene();
    scene.background = new THREE.Color(config.backgroundColor);
    
    // Step 4.2: Create camera
    const { width, height } = config.container.getBoundingClientRect();
    camera = new THREE.PerspectiveCamera(
      config.cameraFOV,
      width / height,
      0.1,
      1000
    );
    camera.position.set(
      config.cameraPosition.x,
      config.cameraPosition.y,
      config.cameraPosition.z
    );
    
    // Step 4.3: Create renderer
    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(width, height);
    renderer.setPixelRatio(window.devicePixelRatio);
    
    // Configure shadows if enabled
    if (config.enableShadows) {
      renderer.shadowMap.enabled = true;
      renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    }
    
    // Step 4.4: Add renderer to container
    config.container.appendChild(renderer.domElement);
    
    // Step 4.5: Add environment lighting
    setupLighting();
    
    // Step 4.6: Add controls
    controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    controls.minDistance = 1;
    controls.maxDistance = 20;
    controls.autoRotate = config.autoRotate;
    controls.autoRotateSpeed = 1.0;
    
    // Step 4.7: Set up loading manager
    setupLoadingManager();
    
    // Step 4.8: Load model if path provided
    if (config.modelPath) {
      loadModel();
    }
    
    // Step 4.9: Set up window resize handler
    window.addEventListener('resize', handleResize);
    
    // Step 4.10: Start animation loop
    animate();
  }
  
  // Step 5: Set up lighting
  function setupLighting() {
    // Add ambient light
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);
    
    // Add directional light (sun)
    const directionalLight = new THREE.DirectionalLight(0xffffff, config.lightIntensity);
    directionalLight.position.set(1, 2, 3);
    directionalLight.castShadow = config.enableShadows;
    
    // Configure shadow properties
    if (config.enableShadows) {
      directionalLight.shadow.mapSize.width = 1024;
      directionalLight.shadow.mapSize.height = 1024;
      directionalLight.shadow.camera.near = 0.5;
      directionalLight.shadow.camera.far = 50;
      directionalLight.shadow.bias = -0.001;
    }
    
    scene.add(directionalLight);
    
    // Add environment map if provided
    if (config.environmentMap) {
      const envMapLoader = new THREE.CubeTextureLoader();
      const envMap = envMapLoader.load(config.environmentMap);
      scene.environment = envMap;
      scene.background = envMap;
    }
    
    // Add ground for shadows if enabled
    if (config.enableShadows) {
      const groundGeometry = new THREE.PlaneGeometry(100, 100);
      const groundMaterial = new THREE.ShadowMaterial({ opacity: 0.3 });
      const ground = new THREE.Mesh(groundGeometry, groundMaterial);
      ground.rotation.x = -Math.PI / 2;
      ground.position.y = -1;
      ground.receiveShadow = true;
      scene.add(ground);
    }
  }
  
  // Step 6: Set up loading manager
  function setupLoadingManager() {
    loadingManager = new THREE.LoadingManager();
    
    if (config.showLoadingIndicator) {
      // Create loading overlay
      loadingScreen = document.createElement('div');
      loadingScreen.style.position = 'absolute';
      loadingScreen.style.top = '0';
      loadingScreen.style.left = '0';
      loadingScreen.style.width = '100%';
      loadingScreen.style.height = '100%';
      loadingScreen.style.background = 'rgba(0, 0, 0, 0.5)';
      loadingScreen.style.display = 'flex';
      loadingScreen.style.justifyContent = 'center';
      loadingScreen.style.alignItems = 'center';
      loadingScreen.style.zIndex = '1000';
      
      const progressBar = document.createElement('div');
      progressBar.style.width = '50%';
      progressBar.style.height = '10px';
      progressBar.style.background = '#333';
      progressBar.style.borderRadius = '5px';
      
      const progressFill = document.createElement('div');
      progressFill.style.width = '0%';
      progressFill.style.height = '100%';
      progressFill.style.background = '#fff';
      progressFill.style.borderRadius = '5px';
      progressFill.style.transition = 'width 0.3s';
      
      progressBar.appendChild(progressFill);
      loadingScreen.appendChild(progressBar);
      config.container.appendChild(loadingScreen);
      
      // Update loading progress
      loadingManager.onProgress = (url, loaded, total) => {
        const percent = (loaded / total) * 100;
        progressFill.style.width = `${percent}%`;
      };
      
      // Remove loading screen when complete
      loadingManager.onLoad = () => {
        loadingScreen.style.display = 'none';
      };
      
      // Show error if loading fails
      loadingManager.onError = (url) => {
        progressFill.style.background = '#f00';
        console.error(`Error loading: ${url}`);
      };
    }
  }
  
  // Step 7: Load 3D model
  function loadModel() {
    let loader;
    
    // Select appropriate loader based on model type
    switch (config.modelType.toLowerCase()) {
      case 'gltf':
        loader = new THREE.GLTFLoader(loadingManager);
        break;
      case 'obj':
        loader = new THREE.OBJLoader(loadingManager);
        break;
      case 'fbx':
        loader = new THREE.FBXLoader(loadingManager);
        break;
      default:
        console.error(`Unsupported model type: ${config.modelType}`);
        return;
    }
    
    // Load model
    loader.load(
      config.modelPath,
      (loadedModel) => {
        // Process based on model type
        if (config.modelType.toLowerCase() === 'gltf') {
          model = loadedModel.scene;
          
          // Check for animations
          if (loadedModel.animations && loadedModel.animations.length > 0) {
            mixer = new THREE.AnimationMixer(model);
            const animation = loadedModel.animations[0];
            const action = mixer.clipAction(animation);
            action.play();
          }
        } else {
          model = loadedModel;
        }
        
        // Enable shadows on model
        if (config.enableShadows) {
          model.traverse((node) => {
            if (node.isMesh) {
              node.castShadow = true;
              node.receiveShadow = true;
            }
          });
        }
        
        // Center model
        const box = new THREE.Box3().setFromObject(model);
        const center = box.getCenter(new THREE.Vector3());
        const size = box.getSize(new THREE.Vector3());
        
        // Normalize size
        const maxDim = Math.max(size.x, size.y, size.z);
        const scale = 2 / maxDim;
        model.scale.multiplyScalar(scale);
        
        // Adjust position to center
        model.position.x = -center.x * scale;
        model.position.y = -center.y * scale;
        model.position.z = -center.z * scale;
        
        // Add model to scene
        scene.add(model);
        
        // Center camera on model (optional)
        if (config.centerCameraOnLoad) {
          const newBox = new THREE.Box3().setFromObject(model);
          const newCenter = newBox.getCenter(new THREE.Vector3());
          controls.target.copy(newCenter);
          controls.update();
        }
        
        // Trigger onModelLoaded callback if provided
        if (typeof config.onModelLoaded === 'function') {
          config.onModelLoaded(model);
        }
      },
      (progress) => {
        // Progress callback is handled by the loading manager
      },
      (error) => {
        console.error('Error loading model:', error);
        if (typeof config.onModelError === 'function') {
          config.onModelError(error);
        }
      }
    );
  }
  
  // Step 8: Animation loop
  function animate() {
    requestAnimationFrame(animate);
    
    // Update controls
    controls.update();
    
    // Update animation mixer if exists
    if (mixer) {
      mixer.update(clock.getDelta());
    }
    
    // Render scene
    renderer.render(scene, camera);
  }
  
  // Step 9: Handle window resize
  function handleResize() {
    const { width, height } = config.container.getBoundingClientRect();
    
    camera.aspect = width / height;
    camera.updateProjectionMatrix();
    renderer.setSize(width, height);
  }
  
  // Step 10: Initialize and return controller object
  init();
  
  // Return controller with public methods
  return {
    // Scene objects
    getScene: () => scene,
    getCamera: () => camera,
    getRenderer: () => renderer,
    getControls: () => controls,
    getModel: () => model,
    
    // Control methods
    setAutoRotate: (value) => {
      controls.autoRotate = value;
    },
    
    resetCamera: () => {
      camera.position.set(
        config.cameraPosition.x,
        config.cameraPosition.y,
        config.cameraPosition.z
      );
      controls.target.set(0, 0, 0);
      controls.update();
    },
    
    // Snapshot method
    takeScreenshot: (width, height) => {
      const originalSize = {
        width: renderer.domElement.width,
        height: renderer.domElement.height
      };
      
      // Set renderer to desired screenshot size
      if (width && height) {
        renderer.setSize(width, height);
        camera.aspect = width / height;
        camera.updateProjectionMatrix();
      }
      
      // Render the scene
      renderer.render(scene, camera);
      
      // Get the image data
      const dataURL = renderer.domElement.toDataURL('image/png');
      
      // Restore original size
      renderer.setSize(originalSize.width, originalSize.height);
      camera.aspect = originalSize.width / originalSize.height;
      camera.updateProjectionMatrix();
      
      return dataURL;
    },
    
    // Cleanup method
    dispose: () => {
      window.removeEventListener('resize', handleResize);
      
      // Dispose geometries and materials
      scene.traverse((object) => {
        if (object.geometry) {
          object.geometry.dispose();
        }
        
        if (object.material) {
          if (Array.isArray(object.material)) {
            object.material.forEach(material => material.dispose());
          } else {
            object.material.dispose();
          }
        }
      });
      
      // Dispose renderer
      renderer.dispose();
      
      // Remove canvas from container
      if (renderer.domElement && renderer.domElement.parentNode) {
        renderer.domElement.parentNode.removeChild(renderer.domElement);
      }
      
      // Remove loading screen if exists
      if (loadingScreen && loadingScreen.parentNode) {
        loadingScreen.parentNode.removeChild(loadingScreen);
      }
    }
  };
}

// Example usage:
const modelViewer = createModelViewer({
  container: document.getElementById('model-viewer'),
  modelPath: 'models/product.glb',
  modelType: 'gltf',
  backgroundColor: 0xf0f0f0,
  enableShadows: true,
  autoRotate: true,
  onModelLoaded: (model) => {
    console.log('Model loaded successfully', model);
  }
});

// Control methods
document.getElementById('rotate-toggle').addEventListener('click', () => {
  const currentState = modelViewer.getControls().autoRotate;
  modelViewer.setAutoRotate(!currentState);
});

document.getElementById('reset-camera').addEventListener('click', () => {
  modelViewer.resetCamera();
});

document.getElementById('take-screenshot').addEventListener('click', () => {
  const screenshot = modelViewer.takeScreenshot(1920, 1080);
  window.open(screenshot);
});

// Cleanup when no longer needed
window.addEventListener('beforeunload', () => {
  modelViewer.dispose();
});
```

#### 4.2.2 INTERACTIVE_SCENE Template

```javascript
/**
 * Creates an interactive 3D scene with raycasting and events
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Scene controller
 */
function createInteractiveScene(options = {}) {
  // Step 1: Import required modules
  const THREE = window.THREE || {};
  
  // Step 2: Merge default options with provided options
  const config = {
    // Container
    container: options.container || document.getElementById('interactive-scene'),
    
    // Scene options
    backgroundColor: options.backgroundColor || 0x222222,
    
    // Interaction options
    enableSelection: options.enableSelection !== undefined ? options.enableSelection : true,
    highlightColor: options.highlightColor || 0xffff00,
    
    // Camera options
    cameraPosition: options.cameraPosition || { x: 0, y: 5, z: 10 },
    cameraFOV: options.cameraFOV || 50,
    
    // Objects to add automatically (optional)
    initialObjects: options.initialObjects || [],
    
    ...options
  };
  
  // Step 3: Create scene objects
  let scene, camera, renderer, controls;
  let raycaster, mouse;
  let interactiveObjects = [];
  let selectedObject = null;
  let originalMaterials = new Map();
  
  // Step 4: Initialize scene
  function init() {
    // Step 4.1: Create scene
    scene = new THREE.Scene();
    scene.background = new THREE.Color(config.backgroundColor);
    
    // Step 4.2: Create camera
    const { width, height } = config.container.getBoundingClientRect();
    camera = new THREE.PerspectiveCamera(
      config.cameraFOV,
      width / height,
      0.1,
      1000
    );
    camera.position.set(
      config.cameraPosition.x,
      config.cameraPosition.y,
      config.cameraPosition.z
    );
    
    // Step 4.3: Create renderer
    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(width, height);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.shadowMap.enabled = true;
    
    // Step 4.4: Add renderer to container
    config.container.appendChild(renderer.domElement);
    
    // Step 4.5: Set up lighting
    setupLighting();
    
    // Step 4.6: Add controls
    controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    
    // Step 4.7: Initialize raycaster and mouse for interactivity
    raycaster = new THREE.Raycaster();
    mouse = new THREE.Vector2();
    
    // Step 4.8: Add initial objects if provided
    if (config.initialObjects && config.initialObjects.length > 0) {
      config.initialObjects.forEach(obj => addInteractiveObject(obj));
    } else {
      // Add some default objects for demonstration
      addDefaultObjects();
    }
    
    // Step 4.9: Add event listeners
    config.container.addEventListener('mousemove', onMouseMove);
    config.container.addEventListener('click', onClick);
    window.addEventListener('resize', onWindowResize);
    
    // Step 4.10: Start animation loop
    animate();
  }
  
  // Step 5: Set up lighting
  function setupLighting() {
    // Add ambient light
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);
    
    // Add directional light
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    directionalLight.position.set(5, 10, 7.5);
    directionalLight.castShadow = true;
    
    // Configure shadow properties
    directionalLight.shadow.mapSize.width = 1024;
    directionalLight.shadow.mapSize.height = 1024;
    directionalLight.shadow.camera.near = 0.5;
    directionalLight.shadow.camera.far = 50;
    directionalLight.shadow.bias = -0.001;
    
    scene.add(directionalLight);
    
    // Add ground plane to receive shadows
    const groundGeometry = new THREE.PlaneGeometry(100, 100);
    const groundMaterial = new THREE.MeshStandardMaterial({ color: 0x999999 });
    const ground = new THREE.Mesh(groundGeometry, groundMaterial);
    ground.rotation.x = -Math.PI / 2;
    ground.position.y = -2;
    ground.receiveShadow = true;
    scene.add(ground);
  }
  
  // Step 6: Add default objects as examples
  function addDefaultObjects() {
    // Create different geometries
    const geometries = [
      new THREE.BoxGeometry(1, 1, 1),
      new THREE.SphereGeometry(0.7, 32, 32),
      new THREE.ConeGeometry(0.7, 2, 32),
      new THREE.TorusGeometry(0.7, 0.3, 16, 32),
      new THREE.CylinderGeometry(0.7, 0.7, 2, 32)
    ];
    
    // Create different colored materials
    const colors = [
      0xff4444, // red
      0x44ff44, // green
      0x4444ff, // blue
      0xff44ff, // magenta
      0x44ffff  // cyan
    ];
    
    // Create objects and position them
    for (let i = 0; i < geometries.length; i++) {
      const material = new THREE.MeshStandardMaterial({ color: colors[i] });
      const mesh = new THREE.Mesh(geometries[i], material);
      
      // Position in a circle
      const angle = (i / geometries.length) * Math.PI * 2;
      const radius = 5;
      
      mesh.position.x = Math.cos(angle) * radius;
      mesh.position.z = Math.sin(angle) * radius;
      mesh.castShadow = true;
      mesh.receiveShadow = true;
      
      // Add user data for interaction
      mesh.userData = {
        id: `object-${i}`,
        name: `Object ${i+1}`,
        description: `This is an interactive ${mesh.geometry.type}`,
        isInteractive: true
      };
      
      // Add to scene and interactive objects array
      scene.add(mesh);
      interactiveObjects.push(mesh);
    }
  }
  
  // Step 7: Handle mouse movement for hovering
  function onMouseMove(event) {
    // Calculate mouse position in normalized device coordinates
    const rect = config.container.getBoundingClientRect();
    mouse.x = ((event.clientX - rect.left) / rect.width) * 2 - 1;
    mouse.y = -((event.clientY - rect.top) / rect.height) * 2 + 1;
  }
  
  // Step 8: Handle click events for selection
  function onClick(event) {
    if (!config.enableSelection) return;
    
    // Update the ray with the camera and mouse position
    raycaster.setFromCamera(mouse, camera);
    
    // Find intersections
    const intersects = raycaster.intersectObjects(interactiveObjects);
    
    if (intersects.length > 0) {
      const object = intersects[0].object;
      
      // If an object is already selected, restore its original material
      if (selectedObject && selectedObject !== object) {
        restoreOriginalMaterial(selectedObject);
      }
      
      // Select or deselect the clicked object
      if (selectedObject === object) {
        restoreOriginalMaterial(object);
        selectedObject = null;
        
        // Trigger deselect callback
        if (typeof config.onObjectDeselected === 'function') {
          config.onObjectDeselected(object);
        }
      } else {
        // Store original material if not already stored
        if (!originalMaterials.has(object.id)) {
          originalMaterials.set(object.id, object.material);
        }
        
        // Apply highlight material
        if (object.material.clone) {
          const highlightMaterial = object.material.clone();
          highlightMaterial.emissive = new THREE.Color(config.highlightColor);
          highlightMaterial.emissiveIntensity = 0.5;
          object.material = highlightMaterial;
        }
        
        selectedObject = object;
        
        // Trigger select callback
        if (typeof config.onObjectSelected === 'function') {
          config.onObjectSelected(object);
        }
      }
    } else if (selectedObject) {
      // Clicked empty space, deselect
      restoreOriginalMaterial(selectedObject);
      selectedObject = null;
      
      // Trigger deselect callback
      if (typeof config.onObjectDeselected === 'function') {
        config.onObjectDeselected(selectedObject);
      }
    }
  }
  
  // Step 9: Restore original material when deselecting
  function restoreOriginalMaterial(object) {
    if (originalMaterials.has(object.id)) {
      object.material = originalMaterials.get(object.id);
    }
  }
  
  // Step 10: Handle window resize
  function onWindowResize() {
    const { width, height } = config.container.getBoundingClientRect();
    
    camera.aspect = width / height;
    camera.updateProjectionMatrix();
    renderer.setSize(width, height);
  }
  
  // Step 11: Animation loop
  function animate() {
    requestAnimationFrame(animate);
    
    // Update controls
    controls.update();
    
    // Update hover state if not in selection mode
    if (!selectedObject) {
      updateHoverState();
    }
    
    // Animate interactive objects (optional custom animation)
    animateObjects();
    
    // Render scene
    renderer.render(scene, camera);
  }
  
  // Step 12: Update hover highlighting
  function updateHoverState() {
    // Reset previously hovered object
    interactiveObjects.forEach(obj => {
      if (obj !== selectedObject && originalMaterials.has(obj.id)) {
        obj.material = originalMaterials.get(obj.id);
      }
    });
    
    // Update the ray with the camera and mouse position
    raycaster.setFromCamera(mouse, camera);
    
    // Find intersections
    const intersects = raycaster.intersectObjects(interactiveObjects);
    
    if (intersects.length > 0) {
      const object = intersects[0].object;
      
      // Skip if this is the already selected object
      if (object === selectedObject) return;
      
      // Store original material if not already stored
      if (!originalMaterials.has(object.id)) {
        originalMaterials.set(object.id, object.material);
      }
      
      // Apply hover material
      if (object.material.clone) {
        const hoverMaterial = object.material.clone();
        hoverMaterial.emissive = new THREE.Color(config.highlightColor);
        hoverMaterial.emissiveIntensity = 0.25;
        object.material = hoverMaterial;
      }
      
      // Trigger hover callback
      if (typeof config.onObjectHover === 'function') {
        config.onObjectHover(object);
      }
      
      // Update cursor
      config.container.style.cursor = 'pointer';
    } else {
      // Reset cursor
      config.container.style.cursor = 'auto';
    }
  }
  
  // Step 13: Optional animations for objects
  function animateObjects() {
    // Simple rotation animation for interactive objects
    if (config.animateObjects) {
      const time = performance.now() * 0.001; // seconds
      
      interactiveObjects.forEach((obj, index) => {
        const speed = 0.5 + (index * 0.1);
        obj.rotation.y = time * speed;
      });
    }
  }
  
  // Step 14: Add an interactive object to the scene
  function addInteractiveObject(object) {
    if (!object) return null;
    
    // If it's a predefined shape configuration
    if (object.type && object.position) {
      let geometry;
      
      // Create geometry based on type
      switch (object.type.toLowerCase()) {
        case 'box':
          geometry = new THREE.BoxGeometry(
            object.width || 1, 
            object.height || 1, 
            object.depth || 1
          );
          break;
        case 'sphere':
          geometry = new THREE.SphereGeometry(
            object.radius || 1, 
            object.widthSegments || 32, 
            object.heightSegments || 32
          );
          break;
        case 'cylinder':
          geometry = new THREE.CylinderGeometry(
            object.radiusTop || 1,
            object.radiusBottom || 1,
            object.height || 2,
            object.radialSegments || 32
          );
          break;
        default:
          console.warn(`Unknown object type: ${object.type}`);
          return null;
      }
      
      // Create material
      const material = new THREE.MeshStandardMaterial({
        color: object.color || 0xffffff,
        metalness: object.metalness || 0.2,
        roughness: object.roughness || 0.7
      });
      
      // Create mesh
      const mesh = new THREE.Mesh(geometry, material);
      
      // Set position
      mesh.position.set(
        object.position.x || 0,
        object.position.y || 0,
        object.position.z || 0
      );
      
      // Set rotation if provided
      if (object.rotation) {
        mesh.rotation.set(
          object.rotation.x || 0,
          object.rotation.y || 0,
          object.rotation.z || 0
        );
      }
      
      // Set scale if provided
      if (object.scale) {
        mesh.scale.set(
          object.scale.x || 1,
          object.scale.y || 1,
          object.scale.z || 1
        );
      }
      
      // Setup shadow
      mesh.castShadow = true;
      mesh.receiveShadow = true;
      
      // Add user data for interaction
      mesh.userData = {
        id: object.id || `object-${interactiveObjects.length}`,
        name: object.name || `Object ${interactiveObjects.length + 1}`,
        description: object.description || `This is an interactive ${object.type}`,
        isInteractive: true,
        ...object.userData
      };
      
      // Add to scene and interactive objects array
      scene.add(mesh);
      interactiveObjects.push(mesh);
      
      return mesh;
    } else if (object instanceof THREE.Mesh) {
      // It's already a mesh, just add it to the scene
      scene.add(object);
      interactiveObjects.push(object);
      
      // Ensure it has userData
      if (!object.userData.isInteractive) {
        object.userData.isInteractive = true;
      }
      
      return object;
    }
    
    return null;
  }
  
  // Step 15: Initialize and return controller object
  init();
  
  // Return public API
  return {
    // Scene access
    getScene: () => scene,
    getCamera: () => camera,
    getRenderer: () => renderer,
    getControls: () => controls,
    
    // Object management
    getInteractiveObjects: () => [...interactiveObjects],
    getSelectedObject: () => selectedObject,
    
    addObject: (objectConfig) => {
      return addInteractiveObject(objectConfig);
    },
    
    removeObject: (object) => {
      // Remove from scene
      scene.remove(object);
      
      // Remove from interactiveObjects array
      const index = interactiveObjects.indexOf(object);
      if (index !== -1) {
        interactiveObjects.splice(index, 1);
      }
      
      // Clean up material if stored
      if (originalMaterials.has(object.id)) {
        originalMaterials.delete(object.id);
      }
      
      // If removed object was selected, clear selection
      if (selectedObject === object) {
        selectedObject = null;
      }
    },
    
    selectObject: (object) => {
      // Deselect current object if any
      if (selectedObject && selectedObject !== object) {
        restoreOriginalMaterial(selectedObject);
      }
      
      // Select new object
      if (object && interactiveObjects.includes(object)) {
        // Store original material if not already stored
        if (!originalMaterials.has(object.id)) {
          originalMaterials.set(object.id, object.material);
        }
        
        // Apply highlight material
        if (object.material.clone) {
          const highlightMaterial = object.material.clone();
          highlightMaterial.emissive = new THREE.Color(config.highlightColor);
          highlightMaterial.emissiveIntensity = 0.5;
          object.material = highlightMaterial;
        }
        
        selectedObject = object;
        
        return true;
      }
      
      return false;
    },
    
    deselectAll: () => {
      if (selectedObject) {
        restoreOriginalMaterial(selectedObject);
        selectedObject = null;
      }
    },
    
    // Camera controls
    setCameraPosition: (x, y, z) => {
      camera.position.set(x, y, z);
    },
    
    lookAt: (x, y, z) => {
      camera.lookAt(x, y, z);
      
      // Update controls target
      if (controls) {
        controls.target.set(x, y, z);
        controls.update();
      }
    },
    
    // Utility methods
    takeScreenshot: () => {
      renderer.render(scene, camera);
      return renderer.domElement.toDataURL('image/png');
    },
    
    // Clean-up
    dispose: () => {
      // Remove event listeners
      config.container.removeEventListener('mousemove', onMouseMove);
      config.container.removeEventListener('click', onClick);
      window.removeEventListener('resize', onWindowResize);
      
      // Dispose materials and geometries
      interactiveObjects.forEach(obj => {
        if (obj.geometry) obj.geometry.dispose();
        if (obj.material) {
          if (Array.isArray(obj.material)) {
            obj.material.forEach(mat => mat.dispose());
          } else {
            obj.material.dispose();
          }
        }
      });
      
      // Dispose renderer
      renderer.dispose();
      
      // Remove canvas from container
      if (renderer.domElement && renderer.domElement.parentNode) {
        renderer.domElement.parentNode.removeChild(renderer.domElement);
      }
      
      // Clear references
      scene = null;
      camera = null;
      renderer = null;
      controls = null;
      raycaster = null;
      interactiveObjects = [];
      selectedObject = null;
      originalMaterials.clear();
    }
  };
}

// Example usage:
const interactiveScene = createInteractiveScene({
  container: document.getElementById('interactive-scene'),
  enableSelection: true,
  animateObjects: true,
  onObjectSelected: (object) => {
    console.log(`Selected: ${object.userData.name}`);
    document.getElementById('info-panel').innerHTML = `
      <h3>${object.userData.name}</h3>
      <p>${object.userData.description}</p>
    `;
  },
  onObjectDeselected: () => {
    document.getElementById('info-panel').innerHTML = 'Click on an object to see details';
  }
});

// Add custom objects
interactiveScene.addObject({
  type: 'box',
  position: { x: 0, y: 1, z: 0 },
  width: 2,
  height: 2,
  depth: 2,
  color: 0xff9900,
  name: 'Custom Box',
  description: 'This is a custom interactive box'
});

// Clean up when not needed
window.addEventListener('beforeunload', () => {
  interactiveScene.dispose();
});
```

### 4.3 Framework-Specific Loading and Optimization Patterns

#### 4.3.1 React Lazy Loading Pattern

```javascript
// Main App.js component with lazy loading of 3D content

import React, { Suspense, lazy, useState } from 'react';
import './App.css';

// Lazy load the 3D component
const ThreeScene = lazy(() => import('./components/ThreeScene'));

function App() {
  const [show3D, setShow3D] = useState(false);
  
  return (
    <div className="App">
      <header className="App-header">
        <h1>Three.js React Integration</h1>
      </header>
      
      <main>
        <button onClick={() => setShow3D(!show3D)}>
          {show3D ? 'Hide 3D Content' : 'Show 3D Content'}
        </button>
        
        {show3D && (
          <Suspense fallback={<div className="loading">Loading 3D scene...</div>}>
            <ThreeScene />
          </Suspense>
        )}
        
        <div className="content">
          <h2>About This Page</h2>
          <p>This demonstrates efficient loading of Three.js content in React.</p>
        </div>
      </main>
    </div>
  );
}

export default App;
```

#### 4.3.2 Vue Progressive Loading Pattern

```javascript
<template>
  <div class="vue-three-app">
    <button @click="loadThreeContent" v-if="!threeJsLoaded">
      Load 3D Content
    </button>
    
    <div v-if="loading" class="loading">
      Loading... {{ Math.round(loadingProgress * 100) }}%
    </div>
    
    <div v-if="threeJsLoaded" class="three-container" ref="threeContainer"></div>
  </div>
</template>

<script>
export default {
  name: 'ThreeJsApp',
  
  data() {
    return {
      threeJsLoaded: false,
      loading: false,
      loadingProgress: 0,
      scene: null,
      camera: null,
      renderer: null,
      model: null,
      mixer: null,
      clock: null,
      controls: null
    };
  },
  
  methods: {
    async loadThreeContent() {
      this.loading = true;
      
      try {
        // Step 1: Load Three.js core dynamically
        this.loadingProgress = 0.1;
        const THREE = await import('three');
        this.loadingProgress = 0.3;
        
        // Step 2: Load required modules one by one
        const { OrbitControls } = await import('three/examples/jsm/controls/OrbitControls');
        this.loadingProgress = 0.4;
        
        const { GLTFLoader } = await import('three/examples/jsm/loaders/GLTFLoader');
        this.loadingProgress = 0.5;
        
        const { DRACOLoader } = await import('three/examples/jsm/loaders/DRACOLoader');
        this.loadingProgress = 0.6;
        
        // Step 3: Initialize scene
        this.initThreeJs(THREE, OrbitControls);
        this.loadingProgress = 0.7;
        
        // Step 4: Load model progressively
        await this.loadModel(THREE, GLTFLoader, DRACOLoader);
        this.loadingProgress = 1.0;
        
        // Step 5: Start animation loop
        this.animate();
        this.threeJsLoaded = true;
        
      } catch (error) {
        console.error('Error loading Three.js content:', error);
      }
      
      this.loading = false;
    },
    
    initThreeJs(THREE, OrbitControls) {
      // Get container dimensions
      const container = this.$refs.threeContainer;
      const width = container.clientWidth;
      const height = container.clientHeight;
      
      // Create scene
      this.scene = new THREE.Scene();
      this.scene.background = new THREE.Color(0xf0f0f0);
      
      // Create camera
      this.camera = new THREE.PerspectiveCamera(45, width / height, 0.1, 1000);
      this.camera.position.set(0, 5, 10);
      
      // Create renderer
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(width, height);
      this.renderer.shadowMap.enabled = true;
      container.appendChild(this.renderer.domElement);
      
      // Create lights
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
      this.scene.add(ambientLight);
      
      const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
      directionalLight.position.set(5, 10, 7.5);
      directionalLight.castShadow = true;
      this.scene.add(directionalLight);
      
      // Create controls
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.enableDamping = true;
      
      // Setup clock for animations
      this.clock = new THREE.Clock();
      
      // Handle resize
      window.addEventListener('resize', this.onWindowResize);
    },
    
    async loadModel(THREE, GLTFLoader, DRACOLoader) {
      return new Promise((resolve, reject) => {
        // Setup DRACO loader for compression
        const dracoLoader = new DRACOLoader();
        dracoLoader.setDecoderPath('/draco/');
        
        const loader = new GLTFLoader();
        loader.setDRACOLoader(dracoLoader);
        
        // Track loading progress
        loader.load(
          '/models/scene.glb',
          (gltf) => {
            this.model = gltf.scene;
            this.scene.add(this.model);
            
            // Setup animations if available
            if (gltf.animations && gltf.animations.length > 0) {
              this.mixer = new THREE.AnimationMixer(this.model);
              const animation = gltf.animations[0];
              const action = this.mixer.clipAction(animation);
              action.play();
            }
            
            // Enable shadows
            this.model.traverse((child) => {
              if (child.isMesh) {
                child.castShadow = true;
                child.receiveShadow = true;
              }
            });
            
            resolve();
          },
          (xhr) => {
            // Update loading progress
            const additionalProgress = 0.3 * (xhr.loaded / xhr.total);
            this.loadingProgress = 0.7 + additionalProgress;
          },
          (error) => {
            reject(error);
          }
        );
      });
    },
    
    animate() {
      requestAnimationFrame(this.animate);
      
      // Update controls
      this.controls.update();
      
      // Update animation mixer
      if (this.mixer) {
        this.mixer.update(this.clock.getDelta());
      }
      
      // Render scene
      this.renderer.render(this.scene, this.camera);
    },
    
    onWindowResize() {
      if (!this.camera || !this.renderer) return;
      
      const container = this.$refs.threeContainer;
      const width = container.clientWidth;
      const height = container.clientHeight;
      
      this.camera.aspect = width / height;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(width, height);
    }
  },
  
  beforeUnmount() {
    // Clean up resources
    window.removeEventListener('resize', this.onWindowResize);
    
    if (this.renderer && this.renderer.domElement) {
      this.renderer.domElement.remove();
      this.renderer.dispose();
    }
    
    if (this.scene) {
      this.scene.traverse((object) => {
        if (object.geometry) object.geometry.dispose();
        if (object.material) {
          if (Array.isArray(object.material)) {
            object.material.forEach(material => material.dispose());
          } else {
            object.material.dispose();
          }
        }
      });
    }
    
    this.scene = null;
    this.camera = null;
    this.renderer = null;
    this.controls = null;
    this.mixer = null;
    this.clock = null;
  }
};
</script>

<style scoped>
.vue-three-app {
  width: 100%;
  height: 100%;
}

.three-container {
  width: 100%;
  height: 500px;
  position: relative;
}

.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 500px;
  background-color: #f0f0f0;
  font-size: 1.5rem;
}
</style>
```

## 5. Optimization Techniques and Patterns

### 5.1 Performance Optimization Taxonomy

```json
{
  "optimization_categories": {
    "GEOMETRY_OPTIMIZATION": {
      "description": "Techniques to optimize 3D geometries and meshes",
      "techniques": [
        {
          "name": "Polygon Reduction",
          "impact": "HIGH",
          "implementation": "Use simplified geometries or decimation algorithms",
          "use_case": "Any 3D scene, especially for mobile"
        },
        {
          "name": "Geometry Instancing",
          "impact": "HIGH",
          "implementation": "Use InstancedMesh for repeated objects",
          "use_case": "Scenes with many identical objects (trees, particles)"
        },
        {
          "name": "Geometry Merging",
          "impact": "MEDIUM",
          "implementation": "Combine static geometries to reduce draw calls",
          "use_case": "Static scene elements that use the same material"
        },
        {
          "name": "Level of Detail (LOD)",
          "impact": "HIGH",
          "implementation": "Switch between different geometry resolutions based on distance",
          "use_case": "Complex models viewed from varying distances"
        }
      ]
    },
    "MATERIAL_OPTIMIZATION": {
      "description": "Techniques to optimize materials and textures",
      "techniques": [
        {
          "name": "Texture Size Reduction",
          "impact": "HIGH",
          "implementation": "Use appropriately sized textures for device capabilities",
          "use_case": "Any textured scene, essential for mobile"
        },
        {
          "name": "Texture Compression",
          "impact": "MEDIUM",
          "implementation": "Use compressed texture formats (KTX2, etc.)",
          "use_case": "Scenes with many or large textures"
        },
        {
          "name": "Material Simplification",
          "impact": "MEDIUM",
          "implementation": "Use simpler materials when appropriate (Basic vs. Standard)",
          "use_case": "Low-end devices or performance-critical applications"
        },
        {
          "name": "Texture Atlasing",
          "impact": "MEDIUM",
          "implementation": "Combine multiple textures into a single atlas",
          "use_case": "Scenes with many small textures or sprite-based content"
        }
      ]
    },
    "RENDERING_OPTIMIZATION": {
      "description": "Techniques to optimize the rendering process",
      "techniques": [
        {
          "name": "Frustum Culling",
          "impact": "HIGH",
          "implementation": "Skip rendering objects outside the camera view",
          "use_case": "Scenes with many objects spread across the environment"
        },
        {
          "name": "Occlusion Culling",
          "impact": "HIGH",
          "implementation": "Skip rendering objects hidden behind others",
          "use_case": "Complex scenes with many overlapping objects"
        },
        {
          "name": "Reducing Draw Calls",
          "impact": "HIGH",
          "implementation": "Combine meshes, use instancing, share materials",
          "use_case": "Any complex scene, crucial for high object counts"
        },
        {
          "name": "Frame Rate Throttling",
          "impact": "MEDIUM",
          "implementation": "Adapt animation frame rate based on device capability",
          "use_case": "Mobile or battery-sensitive applications"
        }
      ]
    },
    "LOADING_OPTIMIZATION": {
      "description": "Techniques to optimize asset loading and initialization",
      "techniques": [
        {
          "name": "Progressive Loading",
          "impact": "HIGH",
          "implementation": "Load essential content first, then enhance gradually",
          "use_case": "Scenes with large models or many assets"
        },
        {
          "name": "Asset Compression",
          "impact": "HIGH",
          "implementation": "Use DRACO compression for models, optimized textures",
          "use_case": "Any scene with significant download size"
        },
        {
          "name": "Lazy Loading",
          "impact": "MEDIUM",
          "implementation": "Only load assets when needed or about to be visible",
          "use_case": "Large environments where not all content is visible at once"
        },
        {
          "name": "Asset Preloading",
          "impact": "MEDIUM",
          "implementation": "Preload critical assets before they're needed",
          "use_case": "Interactive experiences where sudden loading would disrupt flow"
        }
      ]
    },
    "SHADER_OPTIMIZATION": {
      "description": "Techniques to optimize custom shaders and effects",
      "techniques": [
        {
          "name": "Shader Complexity Reduction",
          "impact": "HIGH",
          "implementation": "Simplify shader calculations, especially in fragments",
          "use_case": "Custom shaders, post-processing effects"
        },
        {
          "name": "Shader Feature Toggling",
          "impact": "MEDIUM",
          "implementation": "Enable/disable shader features based on device capability",
          "use_case": "Advanced visual effects that can be gracefully degraded"
        },
        {
          "name": "Baking Effects",
          "impact": "MEDIUM",
          "implementation": "Pre-compute complex effects into textures",
          "use_case": "Static lighting, ambient occlusion, complex patterns"
        },
        {
          "name": "PostProcessing Stack Optimization",
          "impact": "HIGH",
          "implementation": "Reduce or simplify post-processing effects for low-end devices",
          "use_case": "Scenes using EffectComposer with multiple passes"
        }
      ]
    }
  }
}
```

### 5.2 Low-End Device Optimization Patterns

```javascript
/**
 * Creates a device-adaptive Three.js setup with performance optimizations
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Optimized scene controller
 */
function createAdaptiveThreeJsScene(options = {}) {
  // Step 1: Initialize with defaults
  const config = {
    container: options.container || document.getElementById('scene-container'),
    defaultTier: options.defaultTier || 'MEDIUM',
    ...options
  };
  
  // Step 2: Detect device capabilities
  const deviceCapability = detectDeviceCapability();
  console.log('Device capability detected:', deviceCapability);
  
  // Step 3: Merge detected capabilities with config
  const adaptiveConfig = {
    ...config,
    deviceTier: deviceCapability.tier || config.defaultTier
  };
  
  // Step 4: Initialize core objects with adaptive settings
  let scene, camera, renderer, controls;
  
  function init() {
    // Create scene
    scene = new THREE.Scene();
    
    // Create camera with adaptive settings
    const { width, height } = adaptiveConfig.container.getBoundingClientRect();
    camera = new THREE.PerspectiveCamera(60, width / height, 0.1, 
      adaptiveConfig.deviceTier === 'LOW' ? 100 : 1000 // Reduce far plane on low-end
    );
    camera.position.set(0, 5, 10);
    
    // Create renderer with adaptive settings
    renderer = new THREE.WebGLRenderer({
      antialias: adaptiveConfig.deviceTier !== 'LOW', // Disable antialiasing on low-end
      powerPreference: 'high-performance',
      precision: adaptiveConfig.deviceTier === 'LOW' ? 'mediump' : 'highp'
    });
    
    renderer.setSize(width, height);
    renderer.setPixelRatio(getAdaptivePixelRatio());
    
    // Configure shadows based on device tier
    renderer.shadowMap.enabled = adaptiveConfig.deviceTier !== 'LOW';
    if (renderer.shadowMap.enabled) {
      renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    }
    
    // Add renderer to container
    adaptiveConfig.container.appendChild(renderer.domElement);
    
    // Create controls with adaptive settings
    controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    
    // Setup scene based on device capability
    setupAdaptiveLighting();
    setupAdaptiveEnvironment();
    
    // Handle window resize
    window.addEventListener('resize', onWindowResize);
  }
  
  // Step 5: Setup adaptive lighting based on device capability
  function setupAdaptiveLighting() {
    // Add ambient light - keep this for all tiers
    const ambientIntensity = adaptiveConfig.deviceTier === 'LOW' ? 0.6 : 0.4;
    const ambientLight = new THREE.AmbientLight(0xffffff, ambientIntensity);
    scene.add(ambientLight);
    
    // Add directional light with adaptive settings
    const dirLightIntensity = adaptiveConfig.deviceTier === 'LOW' ? 0.8 : 1.0;
    const directionalLight = new THREE.DirectionalLight(0xffffff, dirLightIntensity);
    directionalLight.position.set(5, 10, 7.5);
    
    // Only enable shadows on medium and high tiers
    if (adaptiveConfig.deviceTier !== 'LOW') {
      directionalLight.castShadow = true;
      
      // Adaptive shadow quality
      const shadowMapSize = adaptiveConfig.deviceTier === 'HIGH' ? 2048 : 1024;
      directionalLight.shadow.mapSize.width = shadowMapSize;
      directionalLight.shadow.mapSize.height = shadowMapSize;
      
      directionalLight.shadow.camera.near = 0.5;
      directionalLight.shadow.camera.far = 50;
      directionalLight.shadow.bias = -0.001;
    }
    
    scene.add(directionalLight);
    
    // Add additional lights for higher tiers
    if (adaptiveConfig.deviceTier === 'HIGH') {
      // Add subtle rim light for high-end devices
      const backLight = new THREE.DirectionalLight(0xffffff, 0.3);
      backLight.position.set(-5, 8, -10);
      scene.add(backLight);
    }
  }
  
  // Step 6: Setup adaptive environment based on device capability
  function setupAdaptiveEnvironment() {
    // Add a ground plane
    const groundGeometry = new THREE.PlaneGeometry(100, 100);
    
    // Use simpler material for low-end devices
    let groundMaterial;
    
    if (adaptiveConfig.deviceTier === 'LOW') {
      groundMaterial = new THREE.MeshLambertMaterial({ color: 0x999999 });
    } else {
      groundMaterial = new THREE.MeshStandardMaterial({ 
        color: 0x999999,
        roughness: 0.8,
        metalness: 0.2
      });
    }
    
    const ground = new THREE.Mesh(groundGeometry, groundMaterial);
    ground.rotation.x = -Math.PI / 2;
    ground.position.y = -2;
    
    // Only enable shadows on medium and high tiers
    ground.receiveShadow = adaptiveConfig.deviceTier !== 'LOW';
    
    scene.add(ground);
    
    // Add background based on device tier
    if (adaptiveConfig.deviceTier === 'LOW') {
      // Solid color background for low-end
      scene.background = new THREE.Color(0xeeeeee);
    } else if (adaptiveConfig.deviceTier === 'MEDIUM') {
      // Simple gradient for medium tier
      const canvas = document.createElement('canvas');
      canvas.width = 2;
      canvas.height = 512;
      const context = canvas.getContext('2d');
      const gradient = context.createLinearGradient(0, 0, 0, 512);
      gradient.addColorStop(0, '#87CEFA');
      gradient.addColorStop(1, '#E0F7FA');
      context.fillStyle = gradient;
      context.fillRect(0, 0, 2, 512);
      
      const texture = new THREE.CanvasTexture(canvas);
      texture.minFilter = THREE.LinearFilter;
      scene.background = texture;
    } else {
      // HDRI background for high-end (if specified)
      if (adaptiveConfig.hdriBackground) {
        const hdriLoader = new THREE.RGBELoader();
        hdriLoader.load(adaptiveConfig.hdriBackground, (texture) => {
          texture.mapping = THREE.EquirectangularReflectionMapping;
          scene.background = texture;
          scene.environment = texture;
        });
      } else {
        // Default gradient for high-end
        const canvas = document.createElement('canvas');
        canvas.width = 2;
        canvas.height = 512;
        const context = canvas.getContext('2d');
        const gradient = context.createLinearGradient(0, 0, 0, 512);
        gradient.addColorStop(0, '#4A90E2');
        gradient.addColorStop(1, '#87CEFA');
        context.fillStyle = gradient;
        context.fillRect(0, 0, 2, 512);
        
        const texture = new THREE.CanvasTexture(canvas);
        texture.minFilter = THREE.LinearFilter;
        scene.background = texture;
      }
    }
  }
  
  // Step 7: Create adaptive materials based on device capability
  function createAdaptiveMaterial(options = {}) {
    const materialOptions = {
      color: options.color || 0xffffff,
      map: options.map || null,
      ...options
    };
    
    let material;
    
    // Choose appropriate material type based on device tier
    if (adaptiveConfig.deviceTier === 'LOW') {
      // Use Lambert material for low-end devices (faster, simpler lighting)
      material = new THREE.MeshLambertMaterial(materialOptions);
    } else if (adaptiveConfig.deviceTier === 'MEDIUM') {
      // Use Phong material for medium tier (good balance)
      material = new THREE.MeshPhongMaterial({
        ...materialOptions,
        shininess: options.shininess || 30
      });
    } else {
      // Use Standard material for high-end (PBR lighting)
      material = new THREE.MeshStandardMaterial({
        ...materialOptions,
        roughness: options.roughness !== undefined ? options.roughness : 0.7,
        metalness: options.metalness !== undefined ? options.metalness : 0.2
      });
    }
    
    return material;
  }
  
  // Step 8: Create adaptive geometries based on device capability
  function createAdaptiveGeometry(type, options = {}) {
    let geometry;
    const deviceTier = adaptiveConfig.deviceTier;
    
    // Adjust detail level based on device tier
    const getSegmentCount = (base) => {
      switch (deviceTier) {
        case 'LOW': return Math.max(4, Math.floor(base * 0.5));
        case 'MEDIUM': return base;
        case 'HIGH': return Math.floor(base * 1.5);
        default: return base;
      }
    };
    
    // Create geometry based on type and adjusted detail
    switch (type.toLowerCase()) {
      case 'box':
        geometry = new THREE.BoxGeometry(
          options.width || 1,
          options.height || 1,
          options.depth || 1
        );
        break;
        
      case 'sphere':
        const widthSegments = getSegmentCount(options.widthSegments || 16);
        const heightSegments = getSegmentCount(options.heightSegments || 12);
        
        geometry = new THREE.SphereGeometry(
          options.radius || 1,
          widthSegments,
          heightSegments
        );
        break;
        
      case 'cylinder':
        const radialSegments = getSegmentCount(options.radialSegments || 16);
        
        geometry = new THREE.CylinderGeometry(
          options.radiusTop || 1,
          options.radiusBottom || 1,
          options.height || 2,
          radialSegments
        );
        break;
        
      case 'torus':
        const tubularSegments = getSegmentCount(options.tubularSegments || 32);
        const tubeSegments = getSegmentCount(options.tubeSegments || 16);
        
        geometry = new THREE.TorusGeometry(
          options.radius || 1,
          options.tube || 0.4,
          tubeSegments,
          tubularSegments
        );
        break;
        
      default:
        console.warn(`Unknown geometry type: ${type}`);
        geometry = new THREE.BoxGeometry(1, 1, 1);
    }
    
    return geometry;
  }
  
  // Step 9: Adaptive model loading with LOD
  function loadModelWithLOD(path, options = {}) {
    return new Promise((resolve, reject) => {
      const loadingManager = new THREE.LoadingManager();
      
      // Show loading progress if specified
      if (options.onProgress) {
        loadingManager.onProgress = options.onProgress;
      }
      
      // Determine if we should use Draco compression based on config
      const useDraco = options.useDraco !== undefined 
        ? options.useDraco 
        : adaptiveConfig.deviceTier !== 'LOW'; // Not on low-end by default
      
      // Setup appropriate loader
      const loader = new THREE.GLTFLoader(loadingManager);
      
      if (useDraco) {
        const dracoLoader = new THREE.DRACOLoader();
        dracoLoader.setDecoderPath(options.dracoPath || '/draco/');
        loader.setDRACOLoader(dracoLoader);
      }
      
      // Determine which model variant to load based on device tier
      let modelVariant = path;
      
      if (options.lodPaths && options.lodPaths[adaptiveConfig.deviceTier.toLowerCase()]) {
        modelVariant = options.lodPaths[adaptiveConfig.deviceTier.toLowerCase()];
      }
      
      // Load the appropriate model
      loader.load(
        modelVariant,
        (gltf) => {
          const model = gltf.scene;
          
          // Apply shadows based on device capability
          if (adaptiveConfig.deviceTier !== 'LOW') {
            model.traverse((node) => {
              if (node.isMesh) {
                node.castShadow = true;
                node.receiveShadow = true;
                
                // Optionally simplify materials on low-end/medium devices
                if (adaptiveConfig.deviceTier !== 'HIGH' && options.simplifyMaterials) {
                  // Replace PBR materials with simpler ones
                  if (node.material && node.material.type === 'MeshStandardMaterial') {
                    const simpleMaterial = createAdaptiveMaterial({
                      color: node.material.color,
                      map: node.material.map
                    });
                    node.material.dispose();
                    node.material = simpleMaterial;
                  }
                }
              }
            });
          }
          
          resolve(model);
        },
        // Progress callback
        options.onProgress || (() => {}),
        // Error callback
        reject
      );
    });
  }
  
  // Step 10: Get adaptive pixel ratio based on device capability
  function getAdaptivePixelRatio() {
    const devicePixelRatio = window.devicePixelRatio || 1;
    
    switch (adaptiveConfig.deviceTier) {
      case 'LOW':
        return Math.min(1, devicePixelRatio); // Cap at 1
      case 'MEDIUM':
        return Math.min(2, devicePixelRatio); // Cap at 2
      case 'HIGH':
        return devicePixelRatio; // Use full pixel ratio
      default:
        return devicePixelRatio;
    }
  }
  
  // Step 11: Handle window resize event
  function onWindowResize() {
    const { width, height } = adaptiveConfig.container.getBoundingClientRect();
    
    camera.aspect = width / height;
    camera.updateProjectionMatrix();
    renderer.setSize(width, height);
  }
  
  // Step 12: Animation loop with adaptive frame rate
  let lastTime = 0;
  let frameRateTarget = adaptiveConfig.deviceTier === 'LOW' ? 30 : 60;
  let frameInterval = 1000 / frameRateTarget;
  
  function animate(time) {
    requestAnimationFrame(animate);
    
    // Throttle frame rate for low-end devices
    if (adaptiveConfig.deviceTier === 'LOW') {
      const delta = time - lastTime;
      if (delta < frameInterval) return;
      lastTime = time;
    }
    
    // Update controls
    controls.update();
    
    // Render scene
    renderer.render(scene, camera);
  }
  
  // Step 13: Initialize and start
  init();
  animate(0);
  
  // Return public API
  return {
    // Core objects
    getScene: () => scene,
    getCamera: () => camera,
    getRenderer: () => renderer,
    getControls: () => controls,
    
    // Adaptive creation methods
    createMaterial: createAdaptiveMaterial,
    createGeometry: createAdaptiveGeometry,
    loadModel: loadModelWithLOD,
    
    // Device info
    getDeviceCapability: () => deviceCapability,
    getDeviceTier: () => adaptiveConfig.deviceTier,
    
    // Control methods
    setFrameRate: (fps) => {
      frameRateTarget = fps;
      frameInterval = 1000 / frameRateTarget;
    },
    
    // Cleanup
    dispose: () => {
      window.removeEventListener('resize', onWindowResize);
      
      // Dispose materials and geometries
      scene.traverse((object) => {
        if (object.geometry) {
          object.geometry.dispose();
        }
        
        if (object.material) {
          if (Array.isArray(object.material)) {
            object.material.forEach(material => material.dispose());
          } else {
            object.material.dispose();
          }
        }
      });
      
      // Dispose renderer
      renderer.dispose();
      
      // Remove canvas from container
      if (renderer.domElement && renderer.domElement.parentNode) {
        renderer.domElement.parentNode.removeChild(renderer.domElement);
      }
    }
  };
}

// Example usage:
const threeScene = createAdaptiveThreeJsScene({
  container: document.getElementById('scene-container'),
  hdriBackground: '/path/to/environment.hdr' // Only used for HIGH tier
});

// Create adaptive objects
const material = threeScene.createMaterial({
  color: 0x3399ff,
  roughness: 0.5,
  metalness: 0.3
});

const geometry = threeScene.createGeometry('sphere', {
  radius: 1.5,
  widthSegments: 32,
  heightSegments: 24
});

const mesh = new THREE.Mesh(geometry, material);
threeScene.getScene().add(mesh);

// Load model with LOD
threeScene.loadModel('/path/to/model.glb', {
  lodPaths: {
    low: '/path/to/model_low.glb', // Lower poly version
    medium: '/path/to/model_medium.glb',
    high: '/path/to/model.glb' // Original high-quality
  },
  simplifyMaterials: true,
  onProgress: (url, loaded, total) => {
    console.log(`Loading: ${Math.round(loaded / total * 100)}%`);
  }
}).then(model => {
  threeScene.getScene().add(model);
});

// Dispose when finished
window.addEventListener('beforeunload', () => {
  threeScene.dispose();
});
```

### 5.3 Memory Management Patterns

```javascript
/**
 * Advanced memory management system for Three.js applications
 * Helps prevent memory leaks and optimize memory usage
 */
class ThreeJsMemoryManager {
  constructor() {
    this.geometryCache = new Map();
    this.materialCache = new Map();
    this.textureCache = new Map();
    
    this.disposables = new Set();
    this.disposalQueue = [];
    
    this.memoryUsageHistory = [];
    this.lastMemoryCheck = performance.now();
    this.memoryCheckInterval = 5000; // Check every 5 seconds
    
    // Start memory monitoring if performance memory API is available
    if (window.performance && window.performance.memory) {
      this.startMemoryMonitoring();
    }
  }
  
  /**
   * Register a disposable object for later cleanup
   * @param {Object} object - Object with disposable resources
   * @param {String} type - Type of object for categorization
   */
  register(object, type = 'unspecified') {
    if (!object) return;
    
    // Add to disposables set with metadata
    this.disposables.add({
      object,
      type,
      addedAt: performance.now()
    });
    
    // For complex objects, traverse them to find all disposable resources
    if (object.traverse && typeof object.traverse === 'function') {
      let geometries = new Set();
      let materials = new Set();
      let textures = new Set();
      
      object.traverse(child => {
        if (child.geometry) {
          geometries.add(child.geometry);
        }
        
        if (child.material) {
          if (Array.isArray(child.material)) {
            child.material.forEach(mat => {
              materials.add(mat);
              this.collectTexturesFromMaterial(mat, textures);
            });
          } else {
            materials.add(child.material);
            this.collectTexturesFromMaterial(child.material, textures);
          }
        }
      });
      
      // Add individual resources to disposables
      geometries.forEach(geometry => this.register(geometry, 'geometry'));
      materials.forEach(material => this.register(material, 'material'));
      textures.forEach(texture => this.register(texture, 'texture'));
    }
  }
  
  /**
   * Collect all textures from a material
   * @param {THREE.Material} material - The material to collect textures from
   * @param {Set} textureSet - Set to collect textures
   */
  collectTexturesFromMaterial(material, textureSet) {
    const textureProperties = [
      'map', 'normalMap', 'bumpMap', 'displacementMap', 'roughnessMap',
      'metalnessMap', 'alphaMap', 'aoMap', 'emissiveMap', 'envMap',
      'lightMap', 'specularMap'
    ];
    
    textureProperties.forEach(prop => {
      if (material[prop]) {
        textureSet.add(material[prop]);
      }
    });
  }
  
  /**
   * Create or retrieve a cached geometry
   * @param {String} type - Geometry type
   * @param {Object} params - Geometry parameters
   * @returns {THREE.BufferGeometry} The geometry
   */
  createGeometry(type, params = {}) {
    const key = this.generateGeometryKey(type, params);
    
    if (this.geometryCache.has(key)) {
      return this.geometryCache.get(key);
    }
    
    let geometry;
    switch (type) {
      case 'box':
        geometry = new THREE.BoxGeometry(
          params.width || 1,
          params.height || 1,
          params.depth || 1,
          params.widthSegments || 1,
          params.heightSegments || 1,
          params.depthSegments || 1
        );
        break;
      case 'sphere':
        geometry = new THREE.SphereGeometry(
          params.radius || 1,
          params.widthSegments || 32,
          params.heightSegments || 16
        );
        break;
      case 'plane':
        geometry = new THREE.PlaneGeometry(
          params.width || 1,
          params.height || 1,
          params.widthSegments || 1,
          params.heightSegments || 1
        );
        break;
      // Add other geometry types as needed
      default:
        console.warn(`Unknown geometry type: ${type}`);
        return null;
    }
    
    // Store in cache
    this.geometryCache.set(key, geometry);
    return geometry;
  }
  
  /**
   * Generate a unique key for geometry caching
   * @param {String} type - Geometry type
   * @param {Object} params - Geometry parameters
   * @returns {String} Cache key
   */
  generateGeometryKey(type, params) {
    return `${type}_${JSON.stringify(params)}`;
  }
  
  /**
   * Create or retrieve a cached material
   * @param {String} type - Material type
   * @param {Object} params - Material parameters
   * @returns {THREE.Material} The material
   */
  createMaterial(type, params = {}) {
    const key = this.generateMaterialKey(type, params);
    
    if (this.materialCache.has(key)) {
      return this.materialCache.get(key);
    }
    
    let material;
    switch (type) {
      case 'basic':
        material = new THREE.MeshBasicMaterial(params);
        break;
      case 'standard':
        material = new THREE.MeshStandardMaterial(params);
        break;
      case 'phong':
        material = new THREE.MeshPhongMaterial(params);
        break;
      case 'lambert':
        material = new THREE.MeshLambertMaterial(params);
        break;
      // Add other material types as needed
      default:
        console.warn(`Unknown material type: ${type}`);
        return null;
    }
    
    // Store in cache
    this.materialCache.set(key, material);
    return material;
  }
  
  /**
   * Generate a unique key for material caching
   * @param {String} type - Material type
   * @param {Object} params - Material parameters
   * @returns {String} Cache key
   */
  generateMaterialKey(type, params) {
    // Create a params copy and handle textures specially
    const paramsCopy = { ...params };
    const textureProperties = [
      'map', 'normalMap', 'bumpMap', 'displacementMap', 'roughnessMap',
      'metalnessMap', 'alphaMap', 'aoMap', 'emissiveMap', 'envMap',
      'lightMap', 'specularMap'
    ];
    
    // Replace texture objects with their URLs or unique identifiers
    textureProperties.forEach(prop => {
      if (paramsCopy[prop] && paramsCopy[prop].isTexture) {
        paramsCopy[prop] = paramsCopy[prop].uuid || 'texture';
      }
    });
    
    return `${type}_${JSON.stringify(paramsCopy)}`;
  }
  
  /**
   * Create or retrieve a cached texture
   * @param {String} url - Texture URL
   * @param {Object} params - Texture parameters
   * @returns {THREE.Texture} The texture
   */
  createTexture(url, params = {}) {
    if (this.textureCache.has(url)) {
      return this.textureCache.get(url);
    }
    
    const textureLoader = new THREE.TextureLoader();
    const texture = textureLoader.load(url);
    
    // Apply parameters
    if (params.repeat) {
      texture.repeat.set(params.repeat.x || 1, params.repeat.y || 1);
    }
    
    if (params.offset) {
      texture.offset.set(params.offset.x || 0, params.offset.y || 0);
    }
    
    if (params.wrapS) {
      texture.wrapS = params.wrapS;
    }
    
    if (params.wrapT) {
      texture.wrapT = params.wrapT;
    }
    
    if (params.anisotropy) {
      texture.anisotropy = params.anisotropy;
    }
    
    // Store in cache
    this.textureCache.set(url, texture);
    return texture;
  }
  
  /**
   * Queue an object for disposal
   * @param {Object} object - Object to dispose
   */
  queueForDisposal(object) {
    if (!object) return;
    
    this.disposalQueue.push(object);
    
    // Process queue if it gets large
    if (this.disposalQueue.length > 50) {
      this.processDisposalQueue();
    }
  }
  
  /**
   * Process the disposal queue
   */
  processDisposalQueue() {
    while (this.disposalQueue.length > 0) {
      const object = this.disposalQueue.pop();
      this.disposeObject(object);
    }
  }
  
  /**
   * Dispose a specific object
   * @param {Object} object - Object to dispose
   */
  disposeObject(object) {
    if (!object) return;
    
    // Handle different object types
    if (object.dispose && typeof object.dispose === 'function') {
      object.dispose();
    }
    
    // Remove from disposables set
    this.disposables.forEach(disposable => {
      if (disposable.object === object) {
        this.disposables.delete(disposable);
      }
    });
    
    // Remove from caches
    this.geometryCache.forEach((geometry, key) => {
      if (geometry === object) {
        this.geometryCache.delete(key);
      }
    });
    
    this.materialCache.forEach((material, key) => {
      if (material === object) {
        this.materialCache.delete(key);
      }
    });
    
    this.textureCache.forEach((texture, key) => {
      if (texture === object) {
        this.textureCache.delete(key);
      }
    });
  }
  
  /**
   * Dispose a complex object (scene, mesh, group, etc.)
   * @param {Object} object - Complex object with children
   */
  disposeComplexObject(object) {
    if (!object) return;
    
    if (object.traverse && typeof object.traverse === 'function') {
      const geometries = new Set();
      const materials = new Set();
      const textures = new Set();
      
      // Collect all disposable resources
      object.traverse(child => {
        if (child.geometry) {
          geometries.add(child.geometry);
        }
        
        if (child.material) {
          if (Array.isArray(child.material)) {
            child.material.forEach(mat => {
              materials.add(mat);
              this.collectTexturesFromMaterial(mat, textures);
            });
          } else {
            materials.add(child.material);
            this.collectTexturesFromMaterial(child.material, textures);
          }
        }
      });
      
      // Dispose all collected resources
      geometries.forEach(geometry => this.disposeObject(geometry));
      materials.forEach(material => this.disposeObject(material));
      textures.forEach(texture => this.disposeObject(texture));
    }
    
    // Remove references for GC
    if (object.parent) {
      object.parent.remove(object);
    }
  }
  
  /**
   * Start monitoring memory usage
   */
  startMemoryMonitoring() {
    if (!window.performance || !window.performance.memory) {
      console.warn('Memory monitoring not supported in this browser');
      return;
    }
    
    const checkMemoryUsage = () => {
      const now = performance.now();
      
      // Only check periodically
      if (now - this.lastMemoryCheck > this.memoryCheckInterval) {
        this.lastMemoryCheck = now;
        
        const memoryInfo = {
          timestamp: now,
          usedJSHeapSize: window.performance.memory.usedJSHeapSize,
          totalJSHeapSize: window.performance.memory.totalJSHeapSize
        };
        
        this.memoryUsageHistory.push(memoryInfo);
        
        // Limit history size
        if (this.memoryUsageHistory.length > 100) {
          this.memoryUsageHistory.shift();
        }
        
        // Check if memory usage is getting high
        if (memoryInfo.usedJSHeapSize / memoryInfo.totalJSHeapSize > 0.8) {
          console.warn('Memory usage is high, consider cleaning up unused resources');
        }
      }
      
      requestAnimationFrame(checkMemoryUsage);
    };
    
    requestAnimationFrame(checkMemoryUsage);
  }
  
  /**
   * Clean up all resources
   */
  disposeAll() {
    // Process any pending disposals
    this.processDisposalQueue();
    
    // Dispose all cached resources
    this.geometryCache.forEach(geometry => {
      geometry.dispose();
    });
    this.geometryCache.clear();
    
    this.materialCache.forEach(material => {
      material.dispose();
    });
    this.materialCache.clear();
    
    this.textureCache.forEach(texture => {
      texture.dispose();
    });
    this.textureCache.clear();
    
    // Dispose all registered disposables
    this.disposables.forEach(disposable => {
      if (disposable.object && disposable.object.dispose) {
        disposable.object.dispose();
      }
    });
    this.disposables.clear();
  }
  
  /**
   * Get memory usage statistics
   * @returns {Object} Memory statistics
   */
  getMemoryStats() {
    if (!window.performance || !window.performance.memory) {
      return {
        supported: false
      };
    }
    
    return {
      supported: true,
      current: {
        usedJSHeapSize: window.performance.memory.usedJSHeapSize,
        totalJSHeapSize: window.performance.memory.totalJSHeapSize,
        usageRatio: window.performance.memory.usedJSHeapSize / window.performance.memory.totalJSHeapSize
      },
      history: this.memoryUsageHistory,
      cacheStats: {
        geometries: this.geometryCache.size,
        materials: this.materialCache.size,
        textures: this.textureCache.size
      },
      registeredDisposables: this.disposables.size
    };
  }
}

// Example usage:
const memoryManager = new ThreeJsMemoryManager();

// Create cached geometries and materials
const boxGeometry = memoryManager.createGeometry('box', { width: 1, height: 1, depth: 1 });
const sphereGeometry = memoryManager.createGeometry('sphere', { radius: 0.5 });

const redMaterial = memoryManager.createMaterial('standard', { color: 0xff0000 });
const blueMaterial = memoryManager.createMaterial('standard', { color: 0x0000ff });

// Create mesh
const mesh = new THREE.Mesh(boxGeometry, redMaterial);
scene.add(mesh);

// Register complex object for later disposal
memoryManager.register(mesh, 'mesh');

// Later when done with an object
memoryManager.disposeComplexObject(mesh);

// When shutting down the app
window.addEventListener('beforeunload', () => {
  memoryManager.disposeAll();
});
```

## 6. Model Loading and Asset Management

### 6.1 Model Loading Framework

```javascript
/**
 * Creates a comprehensive model loading system for Three.js
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Model loader controller
 */
function createModelLoadingSystem(options = {}) {
  // Step 1: Default configuration
  const config = {
    loadingManager: options.loadingManager || new THREE.LoadingManager(),
    defaultTextureAnisotropy: options.defaultTextureAnisotropy || 4,
    modelPath: options.modelPath || './models/',
    texturePath: options.texturePath || './textures/',
    environmentPath: options.environmentPath || './environments/',
    enableModelCaching: options.enableModelCaching !== undefined ? options.enableModelCaching : true,
    enableTextureCaching: options.enableTextureCaching !== undefined ? options.enableTextureCaching : true,
    enableDraco: options.enableDraco !== undefined ? options.enableDraco : true,
    dracoPath: options.dracoPath || './draco/',
    ...options
  };
  
  // Step 2: Setup loaders and caches
  const modelCache = new Map();
  const textureCache = new Map();
  const materialCache = new Map();
  
  let gltfLoader, objLoader, fbxLoader, dracoLoader;
  let textureLoader, cubeTextureLoader, rgbeLoader;
  
  // Step 3: Initialize loaders
  function initLoaders() {
    // Setup texture loader
    textureLoader = new THREE.TextureLoader(config.loadingManager);
    textureLoader.setPath(config.texturePath);
    
    // Setup cube texture loader for environment maps
    cubeTextureLoader = new THREE.CubeTextureLoader(config.loadingManager);
    cubeTextureLoader.setPath(config.environmentPath);
    
    // Setup RGBE loader for HDR
    rgbeLoader = new THREE.RGBELoader(config.loadingManager);
    rgbeLoader.setPath(config.environmentPath);
    
    // Setup GLTF loader and Draco if enabled
    gltfLoader = new THREE.GLTFLoader(config.loadingManager);
    gltfLoader.setPath(config.modelPath);
    
    if (config.enableDraco) {
      dracoLoader = new THREE.DRACOLoader();
      dracoLoader.setDecoderPath(config.dracoPath);
      gltfLoader.setDRACOLoader(dracoLoader);
    }
    
    // Setup OBJ loader
    objLoader = new THREE.OBJLoader(config.loadingManager);
    objLoader.setPath(config.modelPath);
    
    // Setup FBX loader
    fbxLoader = new THREE.FBXLoader(config.loadingManager);
    fbxLoader.setPath(config.modelPath);
  }
  
  // Step 4: Configure loading manager callbacks
  function configureLoadingManager() {
    // Optional callbacks if not already provided
    if (!config.loadingManager.onStart) {
      config.loadingManager.onStart = (url, itemsLoaded, itemsTotal) => {
        if (config.onLoadingStart) {
          config.onLoadingStart(url, itemsLoaded, itemsTotal);
        }
      };
    }
    
    if (!config.loadingManager.onProgress) {
      config.loadingManager.onProgress = (url, itemsLoaded, itemsTotal) => {
        const progress = itemsLoaded / itemsTotal;
        if (config.onLoadingProgress) {
          config.onLoadingProgress(url, itemsLoaded, itemsTotal, progress);
        }
      };
    }
    
    if (!config.loadingManager.onLoad) {
      config.loadingManager.onLoad = () => {
        if (config.onLoadingComplete) {
          config.onLoadingComplete();
        }
      };
    }
    
    if (!config.loadingManager.onError) {
      config.loadingManager.onError = (url) => {
        console.error(`Error loading: ${url}`);
        if (config.onLoadingError) {
          config.onLoadingError(url);
        }
      };
    }
  }
  
  // Step 5: Load textures with caching
  function loadTexture(path, textureOptions = {}) {
    // Return from cache if enabled and available
    if (config.enableTextureCaching && textureCache.has(path)) {
      return Promise.resolve(textureCache.get(path));
    }
    
    return new Promise((resolve, reject) => {
      textureLoader.load(
        path,
        (texture) => {
          // Apply texture options
          if (textureOptions.repeat) {
            texture.repeat.set(
              textureOptions.repeat.x || 1, 
              textureOptions.repeat.y || 1
            );
          }
          
          if (textureOptions.offset) {
            texture.offset.set(
              textureOptions.offset.x || 0, 
              textureOptions.offset.y || 0
            );
          }
          
          if (textureOptions.wrapS) {
            texture.wrapS = textureOptions.wrapS;
          }
          
          if (textureOptions.wrapT) {
            texture.wrapT = textureOptions.wrapT;
          }
          
          if (textureOptions.flipY !== undefined) {
            texture.flipY = textureOptions.flipY;
          }
          
          if (textureOptions.encoding) {
            texture.encoding = textureOptions.encoding;
          }
          
          // Set anisotropy if not specified
          if (texture.anisotropy === undefined) {
            const renderer = config.renderer;
            const maxAnisotropy = renderer ? 
              renderer.capabilities.getMaxAnisotropy() : 
              config.defaultTextureAnisotropy;
              
            texture.anisotropy = maxAnisotropy;
          }
          
          // Generate mipmaps if not explicitly disabled
          if (textureOptions.generateMipmaps !== false) {
            texture.generateMipmaps = true;
          }
          
          // Cache the texture if enabled
          if (config.enableTextureCaching) {
            textureCache.set(path, texture);
          }
          
          resolve(texture);
        },
        undefined, // Progress callback uses loading manager
        (error) => {
          console.error(`Error loading texture ${path}:`, error);
          reject(error);
        }
      );
    });
  }
  
  // Step 6: Load environment maps (cube or HDR)
  function loadEnvironmentMap(path, options = {}) {
    const isHDR = path.toLowerCase().endsWith('.hdr');
    const type = options.type || (isHDR ? 'hdr' : 'cube');
    
    // Different loading based on type
    if (type === 'hdr') {
      return new Promise((resolve, reject) => {
        rgbeLoader.load(
          path,
          (texture) => {
            texture.mapping = options.mapping || THREE.EquirectangularReflectionMapping;
            
            if (options.encoding) {
              texture.encoding = options.encoding;
            } else {
              texture.encoding = THREE.RGBEEncoding;
            }
            
            resolve(texture);
          },
          undefined,
          reject
        );
      });
    } else if (type === 'cube') {
      // Parse cube map path to get separate files
      const basePath = path.includes('/') ? 
        path.substring(0, path.lastIndexOf('/') + 1) : '';
        
      const fileName = path.includes('/') ? 
        path.substring(path.lastIndexOf('/') + 1) : path;
        
      const fileExtension = fileName.includes('.') ? 
        fileName.substring(fileName.lastIndexOf('.')) : '';
        
      const baseName = fileName.includes('.') ? 
        fileName.substring(0, fileName.lastIndexOf('.')) : fileName;
      
      // Default cube map face names
      const defaultFaceNames = ['px', 'nx', 'py', 'ny', 'pz', 'nz'];
      const faceNames = options.faceNames || defaultFaceNames;
      
      // Create paths for each face
      const facePaths = faceNames.map(face => 
        `${basePath}${baseName}_${face}${fileExtension}`
      );
      
      return new Promise((resolve, reject) => {
        cubeTextureLoader.load(
          facePaths,
          (cubeTexture) => {
            if (options.encoding) {
              cubeTexture.encoding = options.encoding;
            }
            
            resolve(cubeTexture);
          },
          undefined,
          reject
        );
      });
    } else {
      return Promise.reject(new Error(`Unsupported environment map type: ${type}`));
    }
  }
  
  // Step 7: Load models with format detection and caching
  function loadModel(path, options = {}) {
    // Check cache first if enabled
    const cacheKey = path + JSON.stringify(options);
    if (config.enableModelCaching && modelCache.has(cacheKey)) {
      // Create a clone of the cached model
      const cachedModel = modelCache.get(cacheKey);
      return Promise.resolve(cloneModel(cachedModel));
    }
    
    // Determine file type from extension
    const fileExtension = path.toLowerCase().substring(path.lastIndexOf('.') + 1);
    
    let loaderPromise;
    
    switch (fileExtension) {
      case 'glb':
      case 'gltf':
        loaderPromise = loadGLTF(path, options);
        break;
      case 'obj':
        loaderPromise = loadOBJ(path, options);
        break;
      case 'fbx':
        loaderPromise = loadFBX(path, options);
        break;
      default:
        return Promise.reject(new Error(`Unsupported model format: ${fileExtension}`));
    }
    
    return loaderPromise.then(model => {
      // Apply transforms if specified
      if (options.position) {
        model.position.set(
          options.position.x || 0,
          options.position.y || 0,
          options.position.z || 0
        );
      }
      
      if (options.rotation) {
        model.rotation.set(
          options.rotation.x || 0,
          options.rotation.y || 0,
          options.rotation.z || 0
        );
      }
      
      if (options.scale) {
        if (typeof options.scale === 'number') {
          model.scale.set(options.scale, options.scale, options.scale);
        } else {
          model.scale.set(
            options.scale.x || 1,
            options.scale.y || 1,
            options.scale.z || 1
          );
        }
      }
      
      // Apply shadow settings
      if (options.castShadow !== undefined || options.receiveShadow !== undefined) {
        model.traverse(node => {
          if (node.isMesh) {
            if (options.castShadow !== undefined) {
              node.castShadow = options.castShadow;
            }
            
            if (options.receiveShadow !== undefined) {
              node.receiveShadow = options.receiveShadow;
            }
          }
        });
      }
      
      // Apply custom material overrides
      if (options.materialOverrides) {
        applyMaterialOverrides(model, options.materialOverrides);
      }
      
      // Cache model if enabled
      if (config.enableModelCaching) {
        modelCache.set(cacheKey, cloneModel(model));
      }
      
      return model;
    });
  }
  
  // Step 8: GLTF model loading
  function loadGLTF(path, options = {}) {
    return new Promise((resolve, reject) => {
      gltfLoader.load(
        path,
        (gltf) => {
          const model = gltf.scene;
          
          // Handle animations if present
          if (gltf.animations && gltf.animations.length > 0) {
            model.animations = gltf.animations;
            
            // Create animation mixer if requested
            if (options.createAnimationMixer) {
              model.mixer = new THREE.AnimationMixer(model);
              
              // Create animation actions collection
              model.actions = {};
              
              gltf.animations.forEach((animation) => {
                const action = model.mixer.clipAction(animation);
                model.actions[animation.name] = action;
              });
              
              // Play first animation if autoplay requested
              if (options.autoPlayAnimation) {
                const animationName = typeof options.autoPlayAnimation === 'string' 
                  ? options.autoPlayAnimation 
                  : gltf.animations[0].name;
                  
                if (model.actions[animationName]) {
                  model.actions[animationName].play();
                }
              }
            }
          }
          
          resolve(model);
        },
        undefined, // Progress callback is handled by loadingManager
        (error) => {
          console.error(`Error loading GLTF model ${path}:`, error);
          reject(error);
        }
      );
    });
  }
  
  // Step 9: OBJ model loading
  function loadOBJ(path, options = {}) {
    return new Promise((resolve, reject) => {
      objLoader.load(
        path,
        (object) => {
          // Apply materials if MTL file specified
          if (options.mtlPath) {
            const mtlLoader = new THREE.MTLLoader(config.loadingManager);
            mtlLoader.setPath(config.modelPath);
            
            mtlLoader.load(options.mtlPath, (materials) => {
              materials.preload();
              object.traverse(child => {
                if (child.isMesh) {
                  // Try to find matching material
                  const materialName = child.material.name;
                  if (materialName && materials.materials[materialName]) {
                    child.material = materials.materials[materialName];
                  }
                }
              });
              
              resolve(object);
            }, undefined, (error) => {
              console.warn(`MTL file ${options.mtlPath} failed to load:`, error);
              // Still resolve with the object even if MTL fails
              resolve(object);
            });
          } else {
            resolve(object);
          }
        },
        undefined, // Progress callback is handled by loadingManager
        (error) => {
          console.error(`Error loading OBJ model ${path}:`, error);
          reject(error);
        }
      );
    });
  }
  
  // Step 10: FBX model loading
  function loadFBX(path, options = {}) {
    return new Promise((resolve, reject) => {
      fbxLoader.load(
        path,
        (object) => {
          // Handle animations
          if (object.animations && object.animations.length > 0) {
            // Create animation mixer if requested
            if (options.createAnimationMixer) {
              object.mixer = new THREE.AnimationMixer(object);
              
              // Create animation actions collection
              object.actions = {};
              
              object.animations.forEach((animation) => {
                const action = object.mixer.clipAction(animation);
                object.actions[animation.name] = action;
              });
              
              // Play first animation if autoplay requested
              if (options.autoPlayAnimation) {
                const animationName = typeof options.autoPlayAnimation === 'string' 
                  ? options.autoPlayAnimation 
                  : object.animations[0].name;
                  
                if (object.actions[animationName]) {
                  object.actions[animationName].play();
                }
              }
            }
          }
          
          resolve(object);
        },
        undefined, // Progress callback is handled by loadingManager
        (error) => {
          console.error(`Error loading FBX model ${path}:`, error);
          reject(error);
        }
      );
    });
  }
  
  // Step 11: Apply material overrides to a model
  function applyMaterialOverrides(model, materialOverrides) {
    model.traverse(node => {
      if (node.isMesh) {
        // Check if we have overrides for this specific mesh
        let overrides = null;
        
        // First try to match by name
        if (node.name && materialOverrides[node.name]) {
          overrides = materialOverrides[node.name];
        }
        // Then try to match by material name
        else if (node.material && node.material.name && 
                materialOverrides[node.material.name]) {
          overrides = materialOverrides[node.material.name];
        }
        // Finally try to match by any wildcard patterns
        else if (materialOverrides['*']) {
          overrides = materialOverrides['*'];
        }
        
        // Apply overrides if found
        if (overrides) {
          // Handle array of materials
          if (Array.isArray(node.material)) {
            node.material = node.material.map((mat, index) => {
              // Check for index-specific override
              if (overrides[index]) {
                return createOverrideMaterial(mat, overrides[index]);
              } else if (overrides.all) {
                return createOverrideMaterial(mat, overrides.all);
              }
              return mat;
            });
          } 
          // Handle single material
          else {
            node.material = createOverrideMaterial(node.material, overrides);
          }
        }
      }
    });
  }
  
  // Step 12: Create a new material based on original and overrides
  function createOverrideMaterial(originalMaterial, overrides) {
    // If override specifies a completely new material, create it
    if (overrides.type) {
      let newMaterial;
      
      // Create specific material type
      switch (overrides.type) {
        case 'standard':
          newMaterial = new THREE.MeshStandardMaterial();
          break;
        case 'physical':
          newMaterial = new THREE.MeshPhysicalMaterial();
          break;
        case 'lambert':
          newMaterial = new THREE.MeshLambertMaterial();
          break;
        case 'phong':
          newMaterial = new THREE.MeshPhongMaterial();
          break;
        case 'basic':
          newMaterial = new THREE.MeshBasicMaterial();
          break;
        default:
          console.warn(`Unknown material type: ${overrides.type}, using Standard`);
          newMaterial = new THREE.MeshStandardMaterial();
      }
      
      // Copy basic properties from original if not specified in overrides
      if (originalMaterial.color && !overrides.color) {
        newMaterial.color.copy(originalMaterial.color);
      }
      
      if (originalMaterial.map && !overrides.map) {
        newMaterial.map = originalMaterial.map;
      }
      
      // Apply all overrides to new material
      Object.keys(overrides).forEach(key => {
        if (key !== 'type') {
          applyMaterialProperty(newMaterial, key, overrides[key]);
        }
      });
      
      return newMaterial;
    } 
    // Otherwise, clone original and modify
    else {
      const newMaterial = originalMaterial.clone();
      
      // Apply all overrides
      Object.keys(overrides).forEach(key => {
        applyMaterialProperty(newMaterial, key, overrides[key]);
      });
      
      return newMaterial;
    }
  }
  
  // Step 13: Apply a specific material property, handling textures
  function applyMaterialProperty(material, property, value) {
    // Handle texture properties
    const textureProperties = [
      'map', 'normalMap', 'bumpMap', 'roughnessMap', 'metalnessMap',
      'alphaMap', 'aoMap', 'emissiveMap', 'displacementMap',
      'envMap', 'lightMap', 'specularMap'
    ];
    
    if (textureProperties.includes(property)) {
      // If value is string, load texture
      if (typeof value === 'string') {
        loadTexture(value).then(texture => {
          material[property] = texture;
          material.needsUpdate = true;
        });
      }
      // If value is object, load texture with options
      else if (value && typeof value === 'object' && value.path) {
        loadTexture(value.path, value).then(texture => {
          material[property] = texture;
          material.needsUpdate = true;
        });
      }
      // If value is already a texture, assign directly
      else if (value && value.isTexture) {
        material[property] = value;
      }
      // If value is null, remove texture
      else if (value === null) {
        material[property] = null;
      }
    }
    // Handle color properties
    else if (['color', 'emissive', 'specular'].includes(property)) {
      if (material[property] && material[property].isColor) {
        if (typeof value === 'string' || typeof value === 'number') {
          material[property].set(value);
        }
        else if (Array.isArray(value) && value.length >= 3) {
          material[property].setRGB(value[0], value[1], value[2]);
        }
      }
    }
    // Handle all other properties
    else {
      material[property] = value;
    }
    
    material.needsUpdate = true;
  }
  
  // Step 14: Clone a model (including animations)
  function cloneModel(model) {
    const clone = model.clone();
    
    // Copy animations if they exist
    if (model.animations) {
      clone.animations = model.animations.map(anim => anim.clone());
    }
    
    // Create new mixer if original has one
    if (model.mixer) {
      clone.mixer = new THREE.AnimationMixer(clone);
      
      // Recreate actions if they exist
      if (model.actions) {
        clone.actions = {};
        
        Object.keys(model.actions).forEach(name => {
          const animation = clone.animations.find(anim => anim.name === name);
          if (animation) {
            clone.actions[name] = clone.mixer.clipAction(animation);
          }
        });
      }
    }
    
    return clone;
  }
  
  // Step 15: Initialize and return controller
  initLoaders();
  configureLoadingManager();
  
  // Return public API
  return {
    // Core loading methods
    loadModel,
    loadTexture,
    loadEnvironmentMap,
    
    // Advanced loading methods
    loadModelWithAnimations: (path, options = {}) => {
      return loadModel(path, {
        ...options,
        createAnimationMixer: true
      });
    },
    
    loadSceneWithModels: (modelConfigs = []) => {
      const scene = new THREE.Scene();
      
      const loadPromises = modelConfigs.map(modelConfig => {
        return loadModel(modelConfig.path, modelConfig).then(model => {
          scene.add(model);
          return model;
        });
      });
      
      return Promise.all(loadPromises).then(models => {
        // Attach loaded models to scene for reference
        scene.loadedModels = models;
        return scene;
      });
    },
    
    // Material utilities
    createMaterial: (type, properties = {}) => {
      let material;
      
      switch (type) {
        case 'standard':
          material = new THREE.MeshStandardMaterial(properties);
          break;
        case 'physical':
          material = new THREE.MeshPhysicalMaterial(properties);
          break;
        case 'lambert':
          material = new THREE.MeshLambertMaterial(properties);
          break;
        case 'phong':
          material = new THREE.MeshPhongMaterial(properties);
          break;
        case 'basic':
          material = new THREE.MeshBasicMaterial(properties);
          break;
        default:
          console.warn(`Unknown material type: ${type}, using Standard`);
          material = new THREE.MeshStandardMaterial(properties);
      }
      
      return material;
    },
    
    // Cache management methods
    clearModelCache: () => {
      modelCache.clear();
    },
    
    clearTextureCache: () => {
      textureCache.forEach(texture => {
        texture.dispose();
      });
      textureCache.clear();
    },
    
    // Animation utilities
    updateAnimations: (deltaTime) => {
      modelCache.forEach(model => {
        if (model.mixer) {
          model.mixer.update(deltaTime);
        }
      });
    },
    
    // Get loaders for direct access if needed
    getGLTFLoader: () => gltfLoader,
    getOBJLoader: () => objLoader,
    getFBXLoader: () => fbxLoader,
    getTextureLoader: () => textureLoader,
    
    // Get loading manager
    getLoadingManager: () => config.loadingManager
  };
}

// Example usage:
const modelLoader = createModelLoadingSystem({
  modelPath: '/assets/models/',
  texturePath: '/assets/textures/',
  environmentPath: '/assets/environments/',
  enableDraco: true,
  onLoadingProgress: (url, loaded, total, progress) => {
    document.getElementById('loading-progress').style.width = `${progress * 100}%`;
  },
  onLoadingComplete: () => {
    document.getElementById('loading-screen').style.display = 'none';
  }
});

// Load model with custom material overrides
modelLoader.loadModel('character.glb', {
  position: { x: 0, y: 0, z: 0 },
  scale: 1.5,
  castShadow: true,
  receiveShadow: true,
  createAnimationMixer: true,
  autoPlayAnimation: 'Idle',
  materialOverrides: {
    // Override material for mesh named 'Body'
    'Body': {
      color: 0x3366ff,
      roughness: 0.8,
      metalness: 0.2
    },
    // Override material for mesh named 'Eyes'
    'Eyes': {
      emissive: 0xffff00,
      emissiveIntensity: 0.5
    }
  }
}).then(model => {
  scene.add(model);
  
  // Play a different animation
  if (model.actions && model.actions['Walk']) {
    // Stop current animation
    Object.values(model.actions).forEach(action => action.stop());
    
    // Play walk animation
    model.actions['Walk'].play();
  }
});

// Load environment map
modelLoader.loadEnvironmentMap('studio.hdr', {
  type: 'hdr'
}).then(envMap => {
  scene.environment = envMap;
  scene.background = envMap;
});
```

## 7. Scene Design Patterns

### 7.1 Object Pooling Pattern

```javascript
/**
 * Object Pool for Three.js
 * Efficiently manages and reuses 3D objects to avoid garbage collection
 */
class ThreeObjectPool {
  /**
   * Create a new object pool
   * @param {Object} options - Configuration options
   */
  constructor(options = {}) {
    this.pools = new Map();
    
    this.defaultOptions = {
      initialSize: options.initialSize || 10,
      maxSize: options.maxSize || 100,
      autoExpand: options.autoExpand !== undefined ? options.autoExpand : true,
      onCreateObject: options.onCreateObject || null
    };
  }
  
  /**
   * Create or initialize a pool of objects
   * 
   * @param {String} poolName - Name of the pool
   * @param {Function} factoryFunction - Function to create new objects
   * @param {Object} options - Pool-specific options
   */
  createPool(poolName, factoryFunction, options = {}) {
    if (this.pools.has(poolName)) {
      console.warn(`Pool "${poolName}" already exists. Use another name.`);
      return;
    }
    
    // Merge with default options
    const poolOptions = {
      ...this.defaultOptions,
      ...options
    };
    
    // Create pool entry
    const pool = {
      active: new Set(),
      inactive: [],
      factory: factoryFunction,
      options: poolOptions,
      stats: {
        created: 0,
        reused: 0,
        released: 0,
        peak: 0
      }
    };
    
    // Pre-populate pool with initial objects
    for (let i = 0; i < poolOptions.initialSize; i++) {
      const object = factoryFunction();
      object.__poolName = poolName;
      object.__poolId = `${poolName}_${i}`;
      object.__active = false;
      
      if (poolOptions.onCreateObject) {
        poolOptions.onCreateObject(object);
      }
      
      pool.inactive.push(object);
      pool.stats.created++;
    }
    
    this.pools.set(poolName, pool);
    
    return pool;
  }
  
  /**
   * Get an object from the pool
   * 
   * @param {String} poolName - Name of the pool
   * @param {Object} initParams - Parameters to initialize the object
   * @returns {Object} An object from the pool
   */
  get(poolName, initParams = {}) {
    const pool = this.pools.get(poolName);
    
    if (!pool) {
      throw new Error(`Pool "${poolName}" doesn't exist. Create it first.`);
    }
    
    let object;
    
    // Check if we have an inactive object
    if (pool.inactive.length > 0) {
      object = pool.inactive.pop();
      pool.stats.reused++;
    }
    // Create a new object if allowed
    else if (pool.active.size < pool.options.maxSize || pool.options.autoExpand) {
      object = pool.factory();
      object.__poolName = poolName;
      object.__poolId = `${poolName}_${pool.stats.created}`;
      
      if (pool.options.onCreateObject) {
        pool.options.onCreateObject(object);
      }
      
      pool.stats.created++;
    }
    // No objects available and can't create more
    else {
      throw new Error(`Pool "${poolName}" is full (max size: ${pool.options.maxSize}).`);
    }
    
    // Initialize the object
    this.initObject(object, initParams);
    
    // Mark as active and track
    object.__active = true;
    pool.active.add(object);
    
    // Update peak usage stat
    pool.stats.peak = Math.max(pool.stats.peak, pool.active.size);
    
    return object;
  }
  
  /**
   * Initialize or reset an object with new parameters
   * 
   * @param {Object} object - The object to initialize
   * @param {Object} params - Initialization parameters
   */
  initObject(object, params = {}) {
    // Reset common Three.js properties
    if (object.position) {
      object.position.set(
        params.position?.x || 0,
        params.position?.y || 0,
        params.position?.z || 0
      );
    }
    
    if (object.rotation) {
      object.rotation.set(
        params.rotation?.x || 0,
        params.rotation?.y || 0,
        params.rotation?.z || 0
      );
    }
    
    if (object.scale) {
      if (typeof params.scale === 'number') {
        object.scale.set(params.scale, params.scale, params.scale);
      } else {
        object.scale.set(
          params.scale?.x || 1,
          params.scale?.y || 1,
          params.scale?.z || 1
        );
      }
    }
    
    if (object.visible !== undefined) {
      object.visible = params.visible !== undefined ? params.visible : true;
    }
    
    // Reset object matrix
    if (object.matrix && object.matrixAutoUpdate !== undefined) {
      object.matrixAutoUpdate = params.matrixAutoUpdate !== undefined 
        ? params.matrixAutoUpdate 
        : true;
        
      if (object.matrixAutoUpdate) {
        object.updateMatrix();
      }
    }
    
    // Call custom initializer if provided
    if (params.init && typeof params.init === 'function') {
      params.init(object);
    }
    
    // Call global initializer for the pool if provided
    const pool = this.pools.get(object.__poolName);
    if (pool && pool.options.onInit && typeof pool.options.onInit === 'function') {
      pool.options.onInit(object, params);
    }
  }
  
  /**
   * Release an object back to the pool
   * 
   * @param {Object} object - The object to release
   */
  release(object) {
    if (!object || !object.__poolName || !object.__active) {
      return false;
    }
    
    const poolName = object.__poolName;
    const pool = this.pools.get(poolName);
    
    if (!pool) {
      return false;
    }
    
    // Remove from active set
    pool.active.delete(object);
    
    // Reset common properties
    if (object.visible !== undefined) {
      object.visible = false;
    }
    
    // Remove from scene if it has a parent
    if (object.parent) {
      object.parent.remove(object);
    }
    
    // Call custom release method if provided
    if (pool.options.onRelease && typeof pool.options.onRelease === 'function') {
      pool.options.onRelease(object);
    }
    
    // Mark as inactive
    object.__active = false;
    
    // Add to inactive pool
    pool.inactive.push(object);
    pool.stats.released++;
    
    return true;
  }
  
  /**
   * Release all active objects in a pool
   * 
   * @param {String} poolName - Name of the pool
   */
  releaseAll(poolName) {
    const pool = this.pools.get(poolName);
    
    if (!pool) {
      return false;
    }
    
    // Create array from active set to avoid modification during iteration
    const activeObjects = Array.from(pool.active);
    
    // Release each object
    activeObjects.forEach(object => {
      this.release(object);
    });
    
    return true;
  }
  
  /**
   * Release all objects in all pools
   */
  releaseAllPools() {
    this.pools.forEach((pool, poolName) => {
      this.releaseAll(poolName);
    });
  }
  
  /**
   * Get statistics about a pool
   * 
   * @param {String} poolName - Name of the pool
   * @returns {Object} Pool statistics
   */
  getPoolStats(poolName) {
    const pool = this.pools.get(poolName);
    
    if (!pool) {
      return null;
    }
    
    return {
      ...pool.stats,
      active: pool.active.size,
      inactive: pool.inactive.length,
      total: pool.stats.created,
      utilization: pool.active.size / pool.stats.created
    };
  }
  
  /**
   * Get statistics for all pools
   * 
   * @returns {Object} Statistics for all pools
   */
  getAllPoolStats() {
    const stats = {};
    
    this.pools.forEach((pool, poolName) => {
      stats[poolName] = this.getPoolStats(poolName);
    });
    
    return stats;
  }
  
  /**
   * Destroy a pool and all its objects
   * 
   * @param {String} poolName - Name of the pool
   * @param {Boolean} disposeGeometry - Whether to dispose geometries
   * @param {Boolean} disposeMaterial - Whether to dispose materials
   */
  destroyPool(poolName, disposeGeometry = true, disposeMaterial = true) {
    const pool = this.pools.get(poolName);
    
    if (!pool) {
      return false;
    }
    
    // Helper to dispose object resources
    const disposeObject = (object) => {
      // Remove from scene if it has a parent
      if (object.parent) {
        object.parent.remove(object);
      }
      
      // Dispose geometry
      if (disposeGeometry && object.geometry && typeof object.geometry.dispose === 'function') {
        object.geometry.dispose();
      }
      
      // Dispose material(s)
      if (disposeMaterial && object.material) {
        if (Array.isArray(object.material)) {
          object.material.forEach(material => {
            if (material && typeof material.dispose === 'function') {
              material.dispose();
            }
          });
        } else if (typeof object.material.dispose === 'function') {
          object.material.dispose();
        }
      }
      
      // Call custom dispose method if provided
      if (pool.options.onDispose && typeof pool.options.onDispose === 'function') {
        pool.options.onDispose(object);
      }
    };
    
    // Dispose all active objects
    pool.active.forEach(disposeObject);
    
    // Dispose all inactive objects
    pool.inactive.forEach(disposeObject);
    
    // Remove pool
    this.pools.delete(poolName);
    
    return true;
  }
  
  /**
   * Destroy all pools and their objects
   * 
   * @param {Boolean} disposeGeometry - Whether to dispose geometries
   * @param {Boolean} disposeMaterial - Whether to dispose materials
   */
  destroyAllPools(disposeGeometry = true, disposeMaterial = true) {
    this.pools.forEach((_, poolName) => {
      this.destroyPool(poolName, disposeGeometry, disposeMaterial);
    });
  }
}

// Example usage:
// Create a particle system using object pooling
function createParticleSystem(scene) {
  // Create object pool
  const objectPool = new ThreeObjectPool({
    initialSize: 50,
    maxSize: 1000,
    autoExpand: true
  });
  
  // Create particle pool
  objectPool.createPool('particles', () => {
    const geometry = new THREE.SphereGeometry(0.1, 8, 8);
    const material = new THREE.MeshBasicMaterial({ color: 0xffffff });
    const particle = new THREE.Mesh(geometry, material);
    
    // Add custom properties for particles
    particle.velocity = new THREE.Vector3();
    particle.lifetime = 0;
    particle.maxLifetime = 0;
    
    return particle;
  }, {
    // Custom initialization when object is created
    onCreateObject: (particle) => {
      particle.visible = false;
    },
    
    // Custom initialization when object is retrieved from pool
    onInit: (particle, params) => {
      // Set lifetime
      particle.lifetime = 0;
      particle.maxLifetime = params.lifetime || 2;
      
      // Set random velocity
      particle.velocity.set(
        (Math.random() - 0.5) * 2,
        Math.random() * 3 + 1,
        (Math.random() - 0.5) * 2
      );
      
      // Set random color if requested
      if (params.randomColor) {
        particle.material.color.setHSL(Math.random(), 0.8, 0.5);
      }
    },
    
    // Custom release logic
    onRelease: (particle) => {
      particle.visible = false;
    }
  });
  
  // Emit particles
  function emitParticles(position, count = 10, options = {}) {
    for (let i = 0; i < count; i++) {
      // Get particle from pool
      const particle = objectPool.get('particles', {
        position: position,
        lifetime: options.lifetime || 2,
        randomColor: options.randomColor !== undefined ? options.randomColor : true
      });
      
      // Add to scene
      scene.add(particle);
      particle.visible = true;
    }
  }
  
  // Update particles
  function updateParticles(deltaTime) {
    const pool = objectPool.pools.get('particles');
    
    if (!pool) return;
    
    // Update active particles
    pool.active.forEach(particle => {
      // Update position based on velocity
      particle.position.x += particle.velocity.x * deltaTime;
      particle.position.y += particle.velocity.y * deltaTime;
      particle.position.z += particle.velocity.z * deltaTime;
      
      // Apply gravity
      particle.velocity.y -= 3 * deltaTime;
      
      // Update lifetime
      particle.lifetime += deltaTime;
      
      // Scale down as lifetime progresses
      const lifeRatio = 1 - (particle.lifetime / particle.maxLifetime);
      particle.scale.set(lifeRatio, lifeRatio, lifeRatio);
      
      // Release if lifetime exceeded
      if (particle.lifetime >= particle.maxLifetime) {
        objectPool.release(particle);
      }
    });
  }
  
  // Return particle system controller
  return {
    emitParticles,
    updateParticles,
    getPool: () => objectPool,
    getStats: () => objectPool.getPoolStats('particles'),
    cleanup: () => objectPool.destroyPool('particles')
  };
}

// Usage in animation loop:
const particleSystem = createParticleSystem(scene);

// Emit particles at mouse click
renderer.domElement.addEventListener('click', (event) => {
  // Convert mouse position to 3D coordinates
  const rect = renderer.domElement.getBoundingClientRect();
  const x = ((event.clientX - rect.left) / rect.width) * 2 - 1;
  const y = -((event.clientY - rect.top) / rect.height) * 2 + 1;
  
  // Create ray for picking
  const raycaster = new THREE.Raycaster();
  raycaster.setFromCamera({ x, y }, camera);
  
  // Find intersection with ground plane
  const groundPlane = new THREE.Plane(new THREE.Vector3(0, 1, 0), 0);
  const pointOfIntersection = new THREE.Vector3();
  raycaster.ray.intersectPlane(groundPlane, pointOfIntersection);
  
  // Emit particles at intersection point
  particleSystem.emitParticles(pointOfIntersection, 30, {
    lifetime: 2 + Math.random()
  });
});

// Update in animation loop
function animate(time) {
  requestAnimationFrame(animate);
  
  const deltaTime = clock.getDelta();
  
  // Update particles
  particleSystem.updateParticles(deltaTime);
  
  renderer.render(scene, camera);
}

// Clean up when done
function cleanup() {
  particleSystem.cleanup();
}
```

### 7.2 Scene Manager Pattern

```javascript
/**
 * Scene Manager for Three.js applications
 * Manages multiple scenes, transitions, and scene lifetime
 */
class ThreeSceneManager {
  /**
   * Create a scene manager
   * @param {THREE.WebGLRenderer} renderer - The Three.js renderer
   * @param {Object} options - Configuration options
   */
  constructor(renderer, options = {}) {
    this.renderer = renderer;
    
    this.options = {
      defaultTransitionDuration: options.defaultTransitionDuration || 1.0,
      includeCommonElements: options.includeCommonElements !== undefined ? options.includeCommonElements : true,
      ...options
    };
    
    // Scene registry
    this.scenes = new Map();
    this.activeScene = null;
    this.previousScene = null;
    
    // Common elements across scenes
    this.commonElements = new THREE.Group();
    
    // Transition state
    this.isTransitioning = false;
    this.transitionProgress = 0;
    this.transitionStartTime = 0;
    this.transitionDuration = 0;
    this.transitionSourceScene = null;
    this.transitionTargetScene = null;
    this.transitionCallback = null;
    this.transitionType = null;
    
    // Camera and other properties to interpolate during transition
    this.cameraProps = {
      position: new THREE.Vector3(),
      quaternion: new THREE.Quaternion(),
      zoom: 1
    };
    
    // Post-processing effects
    this.composer = null;
    this.effectsEnabled = false;
    
    // Initialize post-processing if options specified
    if (options.enablePostProcessing) {
      this.initPostProcessing(options.postProcessingOptions);
    }
  }
  
  /**
   * Initialize post-processing effects
   * @param {Object} options - Post-processing options
   */
  initPostProcessing(options = {}) {
    // Create composer
    this.composer = new THREE.EffectComposer(this.renderer);
    
    // Add render pass (always needed as first pass)
    this.renderPass = new THREE.RenderPass(
      this.activeScene ? this.activeScene.scene : new THREE.Scene(),
      this.activeScene ? this.activeScene.camera : new THREE.PerspectiveCamera()
    );
    this.composer.addPass(this.renderPass);
    
    // Add optional effects
    if (options.addBloom) {
      const bloomPass = new THREE.UnrealBloomPass(
        new THREE.Vector2(
          this.renderer.domElement.width, 
          this.renderer.domElement.height
        ),
        options.bloomStrength || 1.5,
        options.bloomRadius || 0.4,
        options.bloomThreshold || 0.85
      );
      this.composer.addPass(bloomPass);
      this.bloomPass = bloomPass;
    }
    
    if (options.addFilm) {
      const filmPass = new THREE.FilmPass(
        options.filmNoisiness || 0.35,
        options.filmScanlines || 0.5,
        options.filmScanlinesCount || 648,
        options.filmGrayscale !== undefined ? options.filmGrayscale : false
      );
      this.composer.addPass(filmPass);
      this.filmPass = filmPass;
    }
    
    // Add optional FXAA pass for anti-aliasing
    if (options.addFXAA) {
      const fxaaPass = new THREE.ShaderPass(THREE.FXAAShader);
      fxaaPass.material.uniforms['resolution'].value.set(
        1 / this.renderer.domElement.width,
        1 / this.renderer.domElement.height
      );
      this.composer.addPass(fxaaPass);
      this.fxaaPass = fxaaPass;
    }
    
    this.effectsEnabled = true;
  }
  
  /**
   * Register a scene with the manager
   * 
   * @param {String} name - Unique name for the scene
   * @param {THREE.Scene} scene - The Three.js scene
   * @param {THREE.Camera} camera - Camera for the scene
   * @param {Object} options - Scene-specific options
   */
  registerScene(name, scene, camera, options = {}) {
    // Check if scene name already exists
    if (this.scenes.has(name)) {
      console.warn(`Scene "${name}" already registered. Overwriting.`);
    }
    
    // Create scene config
    const sceneConfig = {
      name: name,
      scene: scene,
      camera: camera,
      initialized: false,
      active: false,
      initFunction: options.init || null,
      updateFunction: options.update || null,
      disposeFunction: options.dispose || null,
      enterFunction: options.onEnter || null,
      exitFunction: options.onExit || null,
      userData: options.userData || {},
      frozen: false
    };
    
    // Add to registry
    this.scenes.set(name, sceneConfig);
    
    // Auto-activate first scene
    if (this.scenes.size === 1 && !this.activeScene) {
      this.activateScene(name);
    }
    
    return sceneConfig;
  }
  
  /**
   * Activate a scene by name
   * 
   * @param {String} name - Name of the scene to activate
   * @param {Boolean} initialize - Whether to initialize if not already
   */
  activateScene(name, initialize = true) {
    if (!this.scenes.has(name)) {
      console.error(`Scene "${name}" not found.`);
      return false;
    }
    
    // Get scene config
    const sceneConfig = this.scenes.get(name);
    
    // Initialize if needed
    if (!sceneConfig.initialized && initialize) {
      this.initializeScene(name);
    }
    
    // Deactivate current scene
    if (this.activeScene) {
      this.activeScene.active = false;
      
      // Call exit function if provided
      if (this.activeScene.exitFunction) {
        this.activeScene.exitFunction(this.activeScene);
      }
      
      // Store as previous scene
      this.previousScene = this.activeScene;
    }
    
    // Activate new scene
    sceneConfig.active = true;
    this.activeScene = sceneConfig;
    
    // Add common elements if enabled
    if (this.options.includeCommonElements) {
      sceneConfig.scene.add(this.commonElements);
    }
    
    // Call enter function if provided
    if (sceneConfig.enterFunction) {
      sceneConfig.enterFunction(sceneConfig);
    }
    
    // Update render pass for post-processing
    if (this.effectsEnabled && this.renderPass) {
      this.renderPass.scene = sceneConfig.scene;
      this.renderPass.camera = sceneConfig.camera;
    }
    
    return true;
  }
  
  /**
   * Initialize a scene
   * 
   * @param {String} name - Name of the scene to initialize
   */
  initializeScene(name) {
    if (!this.scenes.has(name)) {
      console.error(`Scene "${name}" not found.`);
      return false;
    }
    
    const sceneConfig = this.scenes.get(name);
    
    // Skip if already initialized
    if (sceneConfig.initialized) {
      return true;
    }
    
    // Call init function if provided
    if (sceneConfig.initFunction) {
      sceneConfig.initFunction(sceneConfig);
    }
    
    sceneConfig.initialized = true;
    return true;
  }
  
  /**
   * Transition to another scene with animation
   * 
   * @param {String} targetName - Name of the target scene
   * @param {Object} options - Transition options
   */
  transitionToScene(targetName, options = {}) {
    if (!this.scenes.has(targetName)) {
      console.error(`Target scene "${targetName}" not found.`);
      return false;
    }
    
    // Skip if already transitioning
    if (this.isTransitioning) {
      console.warn('Already transitioning. Ignoring request.');
      return false;
    }
    
    // Skip if target is already active
    if (this.activeScene && this.activeScene.name === targetName) {
      console.warn(`Scene "${targetName}" is already active.`);
      return false;
    }
    
    // Initialize target scene if needed
    if (!this.scenes.get(targetName).initialized) {
      this.initializeScene(targetName);
    }
    
    // Setup transition
    this.transitionSourceScene = this.activeScene;
    this.transitionTargetScene = this.scenes.get(targetName);
    this.transitionDuration = options.duration || this.options.defaultTransitionDuration;
    this.transitionStartTime = performance.now();
    this.transitionProgress = 0;
    this.transitionCallback = options.onComplete || null;
    this.transitionType = options.type || 'fade';
    this.isTransitioning = true;
    
    // Setup specific transition type
    switch (this.transitionType) {
      case 'fade':
        // Nothing special to set up here
        break;
        
      case 'crossfade':
        // Create a clone of the target scene's camera
        this.targetCameraProps = {
          position: this.transitionTargetScene.camera.position.clone(),
          quaternion: this.transitionTargetScene.camera.quaternion.clone(),
          zoom: this.transitionTargetScene.camera.zoom
        };
        break;
        
      case 'camera-move':
        // Store initial camera properties
        this.sourceCameraProps = {
          position: this.transitionSourceScene.camera.position.clone(),
          quaternion: this.transitionSourceScene.camera.quaternion.clone(),
          zoom: this.transitionSourceScene.camera.zoom
        };
        
        // Start with source scene
        this.activateScene(this.transitionSourceScene.name);
        break;
        
      default:
        // Default to fade
        console.warn(`Unknown transition type: "${this.transitionType}". Using "fade".`);
        this.transitionType = 'fade';
    }
    
    return true;
  }
  
  /**
   * Update the transition progress
   */
  updateTransition() {
    if (!this.isTransitioning) return;
    
    // Calculate progress
    const currentTime = performance.now();
    const elapsed = currentTime - this.transitionStartTime;
    this.transitionProgress = Math.min(elapsed / (this.transitionDuration * 1000), 1.0);
    
    // Apply transition based on type
    switch (this.transitionType) {
      case 'fade':
        this.updateFadeTransition();
        break;
        
      case 'crossfade':
        this.updateCrossfadeTransition();
        break;
        
      case 'camera-move':
        this.updateCameraMoveTransition();
        break;
    }
    
    // Check if transition is complete
    if (this.transitionProgress >= 1.0) {
      this.completeTransition();
    }
  }
  
  /**
   * Update fade transition
   */
  updateFadeTransition() {
    // At halfway point, switch scenes
    if (this.transitionProgress >= 0.5 && this.activeScene !== this.transitionTargetScene) {
      this.activateScene(this.transitionTargetScene.name);
    }
  }
  
  /**
   * Update crossfade transition
   */
  updateCrossfadeTransition() {
    // Implement based on renderer capabilities
    // This might require custom shaders or render targets
    console.warn('Crossfade transition not fully implemented yet.');
    
    // At halfway point, switch scenes
    if (this.transitionProgress >= 0.5 && this.activeScene !== this.transitionTargetScene) {
      this.activateScene(this.transitionTargetScene.name);
    }
  }
  
  /**
   * Update camera move transition
   */
  updateCameraMoveTransition() {
    if (!this.sourceCameraProps) return;
    
    // Get source and target cameras
    const sourceCamera = this.transitionSourceScene.camera;
    const targetCamera = this.transitionTargetScene.camera;
    
    // Create eased progress
    const easedProgress = this.easeInOutCubic(this.transitionProgress);
    
    // Interpolate camera properties
    sourceCamera.position.lerp(targetCamera.position, easedProgress);
    sourceCamera.quaternion.slerp(targetCamera.quaternion, easedProgress);
    sourceCamera.zoom = this.lerp(
      this.sourceCameraProps.zoom,
      targetCamera.zoom,
      easedProgress
    );
    
    // Update projection matrix if needed
    sourceCamera.updateProjectionMatrix();
    
    // Switch scenes at the end
    if (this.transitionProgress >= 0.95 && this.activeScene !== this.transitionTargetScene) {
      this.activateScene(this.transitionTargetScene.name);
    }
  }
  
  /**
   * Complete the transition
   */
  completeTransition() {
    this.isTransitioning = false;
    
    // Ensure target scene is active
    if (this.activeScene !== this.transitionTargetScene) {
      this.activateScene(this.transitionTargetScene.name);
    }
    
    // Reset transition properties
    this.transitionProgress = 0;
    this.transitionSourceScene = null;
    this.sourceCameraProps = null;
    this.targetCameraProps = null;
    
    // Call callback if provided
    if (this.transitionCallback) {
      this.transitionCallback(this.transitionTargetScene);
      this.transitionCallback = null;
    }
  }
  
  /**
   * Update all active scenes
   * 
   * @param {Number} deltaTime - Time since last update in seconds
   */
  update(deltaTime) {
    // Update transition if in progress
    if (this.isTransitioning) {
      this.updateTransition();
    }
    
    // Update active scene
    if (this.activeScene && !this.activeScene.frozen) {
      if (this.activeScene.updateFunction) {
        this.activeScene.updateFunction(this.activeScene, deltaTime);
      }
    }
  }
  
  /**
   * Render the current scene
   */
  render() {
    if (!this.activeScene) return;
    
    if (this.effectsEnabled && this.composer) {
      // Update composer settings if needed
      this.renderPass.scene = this.activeScene.scene;
      this.renderPass.camera = this.activeScene.camera;
      
      // Render with effects
      this.composer.render();
    } else {
      // Standard rendering
      this.renderer.render(this.activeScene.scene, this.activeScene.camera);
    }
  }
  
  /**
   * Add an object to the common elements group
   * 
   * @param {THREE.Object3D} object - The object to add
   */
  addCommonElement(object) {
    this.commonElements.add(object);
  }
  
  /**
   * Remove an object from the common elements group
   * 
   * @param {THREE.Object3D} object - The object to remove
   */
  removeCommonElement(object) {
    this.commonElements.remove(object);
  }
  
  /**
   * Set the frozen state of a scene
   * 
   * @param {String} name - Name of the scene
   * @param {Boolean} frozen - Whether the scene is frozen
   */
  setSceneFrozen(name, frozen) {
    if (!this.scenes.has(name)) {
      console.error(`Scene "${name}" not found.`);
      return false;
    }
    
    this.scenes.get(name).frozen = frozen;
    return true;
  }
  
  /**
   * Resize handler - call when canvas size changes
   * 
   * @param {Number} width - New width
   * @param {Number} height - New height
   */
  resize(width, height) {
    // Update all cameras
    this.scenes.forEach(sceneConfig => {
      if (sceneConfig.camera.isPerspectiveCamera) {
        sceneConfig.camera.aspect = width / height;
        sceneConfig.camera.updateProjectionMatrix();
      } else if (sceneConfig.camera.isOrthographicCamera) {
        // Update orthographic camera differently
        const aspectRatio = width / height;
        const frustumSize = sceneConfig.camera.userData.frustumSize || 10;
        
        sceneConfig.camera.left = frustumSize * aspectRatio / -2;
        sceneConfig.camera.right = frustumSize * aspectRatio / 2;
        sceneConfig.camera.top = frustumSize / 2;
        sceneConfig.camera.bottom = frustumSize / -2;
        sceneConfig.camera.updateProjectionMatrix();
      }
    });
    
    // Update renderer
    this.renderer.setSize(width, height);
    
    // Update composer if it exists
    if (this.composer) {
      this.composer.setSize(width, height);
      
      // Update any FXAA pass
      if (this.fxaaPass) {
        this.fxaaPass.material.uniforms['resolution'].value.set(
          1 / width,
          1 / height
        );
      }
    }
  }
  
  /**
   * Dispose a scene and all its resources
   * 
   * @param {String} name - Name of the scene to dispose
   */
  disposeScene(name) {
    if (!this.scenes.has(name)) {
      console.error(`Scene "${name}" not found.`);
      return false;
    }
    
    const sceneConfig = this.scenes.get(name);
    
    // Skip if it's the active scene
    if (sceneConfig === this.activeScene) {
      console.warn(`Cannot dispose active scene "${name}". Activate another scene first.`);
      return false;
    }
    
    // Call dispose function if provided
    if (sceneConfig.disposeFunction) {
      sceneConfig.disposeFunction(sceneConfig);
    }
    
    // Dispose all disposable resources
    sceneConfig.scene.traverse(object => {
      // Dispose geometries
      if (object.geometry) {
        object.geometry.dispose();
      }
      
      // Dispose materials
      if (object.material) {
        if (Array.isArray(object.material)) {
          object.material.forEach(material => {
            this.disposeMaterial(material);
          });
        } else {
          this.disposeMaterial(object.material);
        }
      }
    });
    
    // Remove from registry
    this.scenes.delete(name);
    
    // Reset previous scene if it was this one
    if (this.previousScene === sceneConfig) {
      this.previousScene = null;
    }
    
    return true;
  }
  
  /**
   * Dispose a material and its textures
   * 
   * @param {THREE.Material} material - The material to dispose
   */
  disposeMaterial(material) {
    if (!material) return;
    
    // Dispose textures
    const textureProperties = [
      'map', 'normalMap', 'bumpMap', 'roughnessMap', 'metalnessMap',
      'alphaMap', 'aoMap', 'emissiveMap', 'displacementMap',
      'envMap', 'lightMap', 'specularMap'
    ];
    
    textureProperties.forEach(prop => {
      if (material[prop]) {
        material[prop].dispose();
      }
    });
    
    // Dispose material
    material.dispose();
  }
  
  /**
   * Dispose all scenes and resources
   */
  dispose() {
    // Dispose all scenes
    this.scenes.forEach((_, name) => {
      this.disposeScene(name);
    });
    
    // Clear maps
    this.scenes.clear();
    
    // Clear references
    this.activeScene = null;
    this.previousScene = null;
    
    // Dispose composer if it exists
    if (this.composer) {
      this.composer.dispose();
      this.composer = null;
    }
    
    // Clear common elements
    while (this.commonElements.children.length > 0) {
      const object = this.commonElements.children[0];
      this.commonElements.remove(object);
    }
  }
  
  /**
   * Utility: Linear interpolation
   */
  lerp(a, b, t) {
    return a + (b - a) * t;
  }
  
  /**
   * Utility: Ease in-out cubic function
   */
  easeInOutCubic(t) {
    return t < 0.5
      ? 4 * t * t * t
      : 1 - Math.pow(-2 * t + 2, 3) / 2;
  }
}

// Example usage:
function createSceneManagerExample(renderer) {
  const sceneManager = new ThreeSceneManager(renderer, {
    defaultTransitionDuration: 1.2,
    enablePostProcessing: true,
    postProcessingOptions: {
      addFXAA: true,
      addBloom: true
    }
  });
  
  // Create first scene
  const scene1 = new THREE.Scene();
  const camera1 = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera1.position.set(0, 5, 10);
  
  // Create second scene
  const scene2 = new THREE.Scene();
  scene2.background = new THREE.Color(0x222222);
  const camera2 = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera2.position.set(10, 2, 5);
  
  // Register scenes
  sceneManager.registerScene('main', scene1, camera1, {
    init: (sceneConfig) => {
      // Setup lighting
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
      scene1.add(ambientLight);
      
      const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
      directionalLight.position.set(5, 10, 7.5);
      scene1.add(directionalLight);
      
      // Add a cube
      const geometry = new THREE.BoxGeometry(2, 2, 2);
      const material = new THREE.MeshStandardMaterial({ color: 0x3399ff });
      const cube = new THREE.Mesh(geometry, material);
      scene1.add(cube);
      
      // Store reference for updates
      sceneConfig.userData.cube = cube;
    },
    update: (sceneConfig, deltaTime) => {
      // Rotate cube
      const cube = sceneConfig.userData.cube;
      if (cube) {
        cube.rotation.x += 0.5 * deltaTime;
        cube.rotation.y += 0.7 * deltaTime;
      }
    },
    onEnter: (sceneConfig) => {
      console.log('Entered main scene');
    }
  });
  
  sceneManager.registerScene('secondary', scene2, camera2, {
    init: (sceneConfig) => {
      // Setup lighting
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.3);
      scene2.add(ambientLight);
      
      const spotLight = new THREE.SpotLight(0xffffff, 1);
      spotLight.position.set(0, 10, 0);
      scene2.add(spotLight);
      
      // Add a sphere
      const geometry = new THREE.SphereGeometry(2, 32, 32);
      const material = new THREE.MeshStandardMaterial({ 
        color: 0xff5533,
        roughness: 0.4,
        metalness: 0.6
      });
      const sphere = new THREE.Mesh(geometry, material);
      scene2.add(sphere);
      
      // Store reference for updates
      sceneConfig.userData.sphere = sphere;
    },
    update: (sceneConfig, deltaTime) => {
      // Animate sphere
      const sphere = sceneConfig.userData.sphere;
      if (sphere) {
        sphere.position.y = 2 + Math.sin(performance.now() * 0.001) * 0.5;
      }
    },
    onEnter: (sceneConfig) => {
      console.log('Entered secondary scene');
    }
  });
  
  // Add common elements (visible in all scenes)
  const commonGeometry = new THREE.PlaneGeometry(50, 50);
  const commonMaterial = new THREE.MeshStandardMaterial({ color: 0xcccccc });
  const ground = new THREE.Mesh(commonGeometry, commonMaterial);
  ground.rotation.x = -Math.PI / 2;
  ground.position.y = -1;
  ground.receiveShadow = true;
  
  sceneManager.addCommonElement(ground);
  
  // Return scene manager for use in animation loop
  return sceneManager;
}
```

## 8. Implementation Planning Guidelines

### 8.1 Project Architecture Decision Framework

When implementing Three.js in a project, follow this decision-making framework to select the appropriate architecture:

1. **Project Size and Complexity Assessment:**
   - SMALL: Simple visualization, few objects, minimal interaction (single file)
   - MEDIUM: Multiple scenes, moderate interactions, some custom shaders
   - LARGE: Complex visualization, many objects, advanced interactions, custom rendering

2. **Framework Integration Level:**
   - STANDALONE: Pure Three.js with minimal dependencies
   - LIGHT_FRAMEWORK: Three.js with minimal framework integration (e.g., Vite, Webpack)
   - DEEP_FRAMEWORK: Three.js deeply integrated with framework (React, Vue, Angular)

3. **Code Structure Based on Project Size:**

   **SMALL + ANY_FRAMEWORK:**
   - Single module with inline object creation
   - Scene, camera, and renderer in main file
   - Simple animation loop

   **MEDIUM + STANDALONE:**
   - Modular file structure with ES modules
   - Object-oriented approach for reusable components
   - Scene manager for multiple scenes
   - Asset preloading system

   **MEDIUM + LIGHT_FRAMEWORK:**
   - Component-based architecture
   - Service objects for shared functionality
   - Build system for optimization
   - Basic state management

   **MEDIUM + DEEP_FRAMEWORK:**
   - Framework-specific components
   - Hooks/composables for reusable logic
   - Framework state management integration
   - Component lifecycle integration

   **LARGE + ANY_FRAMEWORK:**
   - Complete architecture with design patterns
   - Custom rendering pipeline
   - Advanced state management
   - Optimized asset loading system
   - Performance monitoring
   - WebWorker integration for complex calculations

4. **File Structure Recommendations:**

   **SMALL Project:**
   ```
   project/
   ├── index.html
   ├── main.js (or app.js)
   ├── style.css
   └── assets/
       ├── models/
       └── textures/
   ```

   **MEDIUM Project:**
   ```
   project/
   ├── src/
   │   ├── main.js (entry point)
   │   ├── components/
   │   │   ├── SceneManager.js
   │   │   ├── ModelLoader.js
   │   │   └── Interactions.js
   │   ├── scenes/
   │   │   ├── MainScene.js
   │   │   └── SecondaryScene.js
   │   ├── objects/
   │   │   ├── Environment.js
   │   │   └── Characters.js
   │   └── utils/
   │       ├── math.js
   │       └── helpers.js
   ├── assets/
   │   ├── models/
   │   └── textures/
   ├── index.html
   └── build/ (or dist/)
   ```

   **LARGE Project:**
   ```
   project/
   ├── src/
   │   ├── main.js (entry point)
   │   ├── config/
   │   │   ├── settings.js
   │   │   └── constants.js
   │   ├── core/
   │   │   ├── Engine.js
   │   │   ├── AssetManager.js
   │   │   ├── SceneManager.js
   │   │   └── RenderManager.js
   │   ├── components/
   │   │   ├── physics/
   │   │   ├── lighting/
   │   │   └── interaction/
   │   ├── scenes/
   │   │   ├── BaseScene.js
   │   │   ├── MainScene.js
   │   │   └── [OtherScenes].js
   │   ├── objects/
   │   │   ├── environment/
   │   │   ├── characters/
   │   │   └── ui/
   │   ├── shaders/
   │   │   ├── materials/
   │   │   └── postprocessing/
   │   ├── utils/
   │   │   ├── math.js
   │   │   ├── debug.js
   │   │   └── performance.js
   │   └── workers/
   │       └── physicsWorker.js
   ├── assets/
   │   ├── models/
   │   ├── textures/
   │   └── sounds/
   ├── tests/
   ├── build/ (or dist/)
   └── docs/
   ```

### 8.2 Implementation Timeline Guidelines

When planning Three.js implementation, consider these timeline estimates based on project complexity:

1. **SIMPLE Project (Basic 3D Visualization)**
   - Setup and Environment: 1 day
   - Core Functionality: 2-3 days
   - Testing and Optimization: 1 day
   - **Total: ~1 week**

2. **INTERMEDIATE Project (Interactive 3D Experience)**
   - Planning and Architecture: 2-3 days
   - Environment Setup: 2 days
   - Core Systems Development: 5-7 days
   - Asset Integration: 3-4 days
   - Interactions and UI: 3-5 days
   - Testing and Optimization: 3-4 days
   - **Total: ~3-4 weeks**

3. **ADVANCED Project (Complex 3D Application)**
   - Requirements Analysis: 1 week
   - Architecture Design: 1 week
   - Core Systems Development: 3-4 weeks
   - Custom Shaders and Visual Effects: 2-3 weeks
   - Asset Pipeline and Integration: 2 weeks
   - Advanced Interactions: 2-3 weeks
   - Performance Optimization: 1-2 weeks
   - Testing and Refinement: 2 weeks
   - **Total: ~3-4 months**

Priority should be given to creating a functional prototype early in the process, with optimization, effects, and refinements added iteratively.

### 8.3 Performance Budget Guidelines

Establish performance budgets based on target device capabilities:

1. **LOW-END Devices (Budget)**
   - Draw calls: < 50 per frame
   - Triangles: < 100,000
   - Texture memory: < 25 MB
   - JavaScript memory: < 50 MB
   - Target frame rate: 30 FPS

2. **MID-RANGE Devices (Standard)**
   - Draw calls: < 150 per frame
   - Triangles: < 500,000
   - Texture memory: < 100 MB
   - JavaScript memory: < 150 MB
   - Target frame rate: 60 FPS

3. **HIGH-END Devices (Premium)**
   - Draw calls: < 500 per frame
   - Triangles: < 2,000,000
   - Texture memory: < 500 MB
   - JavaScript memory: < 500 MB
   - Target frame rate: 60+ FPS

Implement device detection and adaptive rendering to optimize for all target devices.

## 9. Case Studies and Implementation Patterns

### 9.1 Product Configurator Implementation

Key architectural components for a Three.js product configurator:

1. **Core Structure:**
   - Model loading system with variant management
   - Configuration state management
   - Real-time material/texture swapping
   - Camera control system for viewpoints
   - Screenshot/sharing capabilities

2. **Optimization Techniques:**
   - Pre-loading of all variant models/textures
   - Instanced geometries for identical components
   - LOD models based on zoom level
   - Material caching and reuse

3. **Implementation Approach:**
   - Component-based architecture with clear responsibility separation
   - State management for configurations
   - Undo/redo functionality
   - Dynamic scene updates without re-rendering entire scene

### 9.2 Immersive Scroll Experience Implementation

Key architectural components for a Three.js scroll-based experience:

1. **Core Structure:**
   - Scroll position tracking and synchronization
   - Scene progression management
   - Camera path animation system
   - Scroll-based object animation system
   - Overlay UI integration

2. **Optimization Techniques:**
   - Lazy-loading of scene elements based on scroll position
   - Object pooling for repeated elements
   - Progressive enhancement based on device capability
   - Throttled scene updates

3. **Implementation Approach:**
   - Keyframe-based animation system
   - Animation weight blending for smooth transitions
   - Frustum culling for off-screen elements
   - Intersection Observer integration for performance

### 9.3 Interactive Data Visualization Implementation

Key architectural components for a Three.js data visualization:

1. **Core Structure:**
   - Data processing and normalization system
   - Dynamic geometry generation
   - Interactive selection and filtering
   - Animation system for data transitions
   - Integration with UI controls

2. **Optimization Techniques:**
   - Custom shaders for large datasets
   - Instanced geometries for repeated elements
   - LOD-based detail management
   - WebWorkers for data processing

3. **Implementation Approach:**
   - Modular architecture with clean separation of data and visualization
   - Reactive system for data changes
   - Custom interactions for exploration
   - Viewport management for focusing on selected data

## 10. Final Recommendations and Best Practices

### 10.1 Performance Best Practices Summary

1. **Rendering Optimization:**
   - Minimize draw calls through geometry/material merging and instancing
   - Use appropriate LOD (Level of Detail) models
   - Implement frustum and occlusion culling
   - Use WebGL renderer hints (preserveDrawingBuffer, precision, etc.)

2. **Asset Optimization:**
   - Optimize models (reduce polygon count, use draco compression)
   - Optimize textures (right-size, compress, use texture atlases)
   - Use appropriate texture formats (KTX2, WebP)
   - Implement progressive loading strategies

3. **Code Optimization:**
   - Avoid garbage collection by reusing objects (object pooling)
   - Use typed arrays for large data sets
   - Implement proper dispose() patterns
   - Offload heavy computation to WebWorkers

4. **Memory Management:**
   - Properly dispose of unused resources (geometries, materials, textures)
   - Implement asset unloading for scenes not in use
   - Monitor memory usage and implement memory budgets
   - Use texture compression and mipmap generation

### 10.2 Cross-Browser and Cross-Device Compatibility

1. **Browser Support Strategy:**
   - Implement feature detection for WebGL capabilities
   - Provide fallbacks for unsupported features
   - Test across Chrome, Firefox, Safari, and Edge
   - Use standard WebGL 1.0 features for maximum compatibility

2. **Mobile Optimization:**
   - Implement touch controls and gestures
   - Adjust rendering quality based on device capability
   - Optimize for battery life (request animation frame throttling)
   - Test on actual devices, not just emulators

3. **Responsive Design:**
   - Implement adaptive rendering resolution
   - Adjust scene complexity based on device performance
   - Design UI elements for both desktop and mobile interaction
   - Use responsive sizing for canvas elements

### 10.3 Implementation Checklist

Use this checklist for all Three.js implementations:

1. **Performance**
   - [ ] Implemented adaptive rendering quality based on device
   - [ ] Optimized geometries and materials
   - [ ] Set up proper resource disposal
   - [ ] Tested on target devices and browsers
   - [ ] Used performance monitoring tools

2. **User Experience**
   - [ ] Implemented responsive canvas sizing
   - [ ] Added loading indicators
   - [ ] Ensured smooth camera controls
   - [ ] Provided fallbacks for low-end devices
   - [ ] Added proper interaction feedback

3. **Code Quality**
   - [ ] Used modular architecture
   - [ ] Implemented proper error handling
   - [ ] Applied consistent naming conventions
   - [ ] Added documentation for complex parts
   - [ ] Set up proper memory management

4. **Compatibility**
   - [ ] Tested across browsers (Chrome, Firefox, Safari, Edge)
   - [ ] Verified mobile support (iOS, Android)
   - [ ] Added WebGL support detection
   - [ ] Provided fallbacks where necessary

5. **Optimization**
   - [ ] Minimized draw calls
   - [ ] Optimized asset loading
   - [ ] Implemented culling techniques
   - [ ] Added LOD systems for complex models
   - [ ] Used object pooling for dynamic objects

Following these guidelines will ensure your Three.js implementations are performant, maintainable, and provide excellent user experiences across a wide range of devices and browsers.

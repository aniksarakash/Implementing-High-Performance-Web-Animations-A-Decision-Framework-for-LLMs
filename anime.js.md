# Anime.js Implementation Guide For LLMs

## 1. Core Understanding Framework

This document is structured for Large Language Models (LLMs) to understand, reason about, and implement anime.js animations in web development projects. It provides a systematic approach to decision-making and implementation across various contexts.

### 1.1 Library Definition

Anime.js is a JavaScript animation library that manipulates DOM elements, SVG, CSS properties, and JavaScript objects through a unified API. Version 4.0.2 is the current stable release as of May 2025.

### 1.2 Implementation Context Parameters

When implementing anime.js animations, reason through these parameters sequentially:

1. **Performance Target:**
   - LOW = Low-end devices, minimal animations, core functionality only
   - MEDIUM = Average devices, balanced animations, some visual enhancements
   - HIGH = High-end devices, complex animations, rich visual effects

2. **Platform Context:**
   - SERVER-RENDERED = PHP MVC, Laravel frameworks
   - MODERN-FRAMEWORK = React, Vue, Astro frameworks
   - STATIC = Plain HTML/JS implementation

3. **Animation Complexity Matrix:**
   - SIMPLE = Basic transitions, opacity, translation (Level 1)
   - INTERMEDIATE = Timeline sequences, staggered animations (Level 2)
   - ADVANCED = SVG manipulation, 3D effects, complex interactions (Level 3)

4. **Hosting Environment Constraints:**
   - SHARED_HOSTING = Limited resources, optimization critical
   - DEDICATED = More resources available, optimization recommended
   - CLOUD = Scalable resources, standard optimization

## 2. Library Technical Framework

### 2.1 Architectural Model

Anime.js operates through a modular function-based architecture that exposes individual utility functions rather than a single monolithic object. This structure facilitates tree-shaking for optimized bundle size.

#### 2.1.1 Core Functions:

```
animate() → Primary animation function
createTimeline() → Creates animation sequences
stagger() → Controls timing across multiple elements
createDrawable() → Prepares SVG elements for drawing animations
createDraggable() → Enables interactive element manipulation
```

### 2.2 Feature-Decision Matrix

When implementing anime.js, apply this decision matrix to determine which features to utilize:

| Context Parameter | Feature Selection | Implementation Approach |
|-------------------|------------------|-------------------------|
| LOW + SHARED_HOSTING | Base animate() only | Minimize animations, use only transform/opacity |
| MEDIUM + SHARED_HOSTING | animate() + stagger() | Limited sequences, optimize for performance |
| HIGH + ANY_HOSTING | Full feature set | Leverage all capabilities as needed |
| ANY + SERVER-RENDERED | DOM-ready events | Ensure animations execute after DOM loads |
| ANY + MODERN-FRAMEWORK | Component lifecycle hooks | Initialize animations in mount/effect hooks |

### 2.3 Module Selection Tree

Follow this decision tree to select appropriate modules:

1. **Basic Animation Required Only?**
   - YES → Import { animate } only
   - NO → Continue to step 2

2. **Multiple Elements with Sequential Timing?**
   - YES → Import { animate, stagger }
   - NO → Continue to step 3

3. **Complex Animation Sequences?**
   - YES → Import { animate, createTimeline }
   - NO → Continue to step 4

4. **SVG Animations Required?**
   - YES → Import { animate, createDrawable }
   - NO → Continue to step 5

5. **Interactive Draggable Elements?**
   - YES → Import { createDraggable }
   - NO → Use only previously selected modules

### 2.4 Performance Analysis Dataset

Anime.js performance characteristics can be modeled through these quantifiable metrics:

```json
{
  "performance_metrics": {
    "fps_capability": {
      "low_complexity": "60fps on low-end devices",
      "medium_complexity": "60fps on medium devices, 30-60fps on low-end",
      "high_complexity": "60fps on high-end, 30-40fps on medium, <30fps on low-end"
    },
    "element_thresholds": {
      "safe_threshold": "500 elements (1000 tweens)",
      "medium_threshold": "1500 elements (3000 tweens)",
      "upper_threshold": "3000 elements (6000 tweens)"
    },
    "bundle_size": {
      "min_import": "3KB (single animate function)",
      "typical_import": "8-10KB (common feature set)",
      "full_library": "17KB (all features)"
    }
  }
}
```

### 2.5 Library Selection Algorithm

When determining whether to use anime.js versus alternatives, apply this algorithm:

1. **Input:** Project requirements, target devices, animation complexity
2. **Process:**
   ```
   IF animation_complexity == HIGH AND elements_count > 3000 THEN
     recommended_library = "GSAP"
   ELSE IF bundle_size_critical == TRUE AND animation_complexity <= MEDIUM THEN
     recommended_library = "anime.js"
   ELSE IF modern_framework_integration == PRIMARY_CONCERN THEN
     IF framework == "React" THEN
       recommended_library = "framer-motion OR anime.js" 
     ELSE
       recommended_library = "anime.js"
   ELSE
     recommended_library = "anime.js"
   END IF
   ```
3. **Output:** Recommended animation library

Rationale: Anime.js provides optimal performance-to-size ratio for most use cases, only being outperformed by GSAP in extremely complex animation scenarios involving thousands of elements.

## 3. Implementation Process Model

### 3.1 Installation Decision Tree

```
START
├── Is the project using a build system with NPM/Yarn?
│   ├── YES
│   │   └── INSTALL METHOD: npm i animejs
│   └── NO
│       ├── Is the project on a shared hosting environment?
│       │   ├── YES
│       │   │   └── INSTALL METHOD: CDN with version lock
│       │   │       └── <script src="https://cdn.jsdelivr.net/npm/animejs@4.0.2/lib/anime.min.js"></script>
│       │   └── NO
│       │       └── INSTALL METHOD: Download and self-host
│       │           └── Download from https://github.com/juliangarnier/anime/releases
└── END
```

### 3.2 Framework Integration Models

Each framework requires specific integration patterns. LLMs should follow these exact implementation templates:

#### 3.2.1 React Implementation Template

```javascript
// Step 1: Import required functions from anime.js
import { animate, stagger } from 'animejs';
import { useEffect, useRef } from 'react';

// Step 2: Create component with animation logic
function AnimatedComponent() {
  // Step 2.1: Create reference to DOM element
  const elementRef = useRef(null);
  
  // Step 2.2: Initialize animation in useEffect hook
  useEffect(() => {
    // Step 2.3: Only run animation if element exists
    if (elementRef.current) {
      // Step 2.4: Create animation with configuration
      animate(elementRef.current, {
        translateX: 250,
        rotate: 360,
        duration: 1000,
        easing: 'easeInOutQuad'
      });
    }
    
    // Step 2.5: Optional cleanup for animations that need to be stopped
    return () => {
      // Animation cleanup if needed
    };
  }, []); // Empty dependency array means run once on mount
  
  // Step 3: Return component JSX with ref attached
  return <div ref={elementRef} className="element"></div>;
}
```

#### 3.2.2 Vue Implementation Template

```javascript
<template>
  <!-- Step 1: Create element with ref attribute -->
  <div ref="element" class="element"></div>
</template>

<script>
// Step 2: Import animate function
import { animate } from 'animejs';

export default {
  // Step 3: Initialize animation in lifecycle hook
  mounted() {
    // Step 3.1: Access element via refs and animate
    animate(this.$refs.element, {
      translateX: 250,
      rotate: 360,
      duration: 1000,
      easing: 'easeInOutQuad'
    });
  },
  // Step 4: Optional cleanup in beforeUnmount
  beforeUnmount() {
    // Animation cleanup if needed
  }
}
</script>
```

#### 3.2.3 Server-Rendered (PHP/Laravel) Implementation Template

```php
<!-- Step 1: Create target element with unique ID -->
<div id="anime-element-<?php echo $uniqueId; ?>" class="element"></div>

<!-- Step 2: Add script after element definition -->
<script>
  // Step 2.1: Wait for DOM to be ready
  document.addEventListener('DOMContentLoaded', () => {
    // Step 2.2: Import anime.js is already handled via <script> tag
    
    // Step 2.3: Select element and animate
    anime({
      targets: '#anime-element-<?php echo $uniqueId; ?>',
      translateX: 250,
      rotate: 360,
      duration: 1000,
      easing: 'easeInOutQuad'
    });
  });
</script>
```

#### 3.2.4 Astro Implementation Template

```astro
---
// Step 1: Set up component properties if needed
const { id = 'anime-element' } = Astro.props;
---

<!-- Step 2: Create target element -->
<div id={id} class="element"></div>

<!-- Step 3: Add client-side script with directives -->
<script>
  // Step 3.1: Import animation function
  import { animate } from 'animejs';
  
  // Step 3.2: Initialize after DOM is ready
  document.addEventListener('DOMContentLoaded', () => {
    const elementId = document.currentScript.previousElementSibling.id;
    
    // Step 3.3: Create animation
    animate(`#${elementId}`, {
      translateX: 250,
      rotate: 360,
      duration: 1000,
      easing: 'easeInOutQuad'
    });
  });
</script>
```

### 3.3 Optimization Algorithm for Performance Tuning

LLMs should follow this deterministic algorithm when implementing performance optimizations:

```
function determineOptimizationStrategy(context) {
  // Initialize optimization array
  const optimizations = [];
  
  // Step 1: Always use hardware-accelerated properties
  optimizations.push({
    type: "HARDWARE_ACCELERATION",
    implementation: "Use transform and opacity properties exclusively when possible",
    example: `
      // Preferred
      animate(element, { 
        translateX: 100,
        opacity: [0, 1]
      });
      
      // Avoid
      animate(element, {
        left: 100,
        display: ['none', 'block'] 
      });
    `
  });
  
  // Step 2: Analyze device target and adjust accordingly
  if (context.performanceTarget === "LOW") {
    optimizations.push({
      type: "USE_WAAPI",
      implementation: "Implement Web Animations API version for simple animations",
      example: `
        import { waapi } from 'animejs';
        
        // Lighter weight implementation for low-end devices
        const animation = waapi.animate(targets, {
          translateX: 100,
          duration: 1000
        });
      `
    });
    
    optimizations.push({
      type: "REDUCE_ANIMATIONS",
      implementation: "Limit number and complexity of animations",
      thresholds: {
        maxActiveAnimations: 5,
        maxElementsPerAnimation: 10,
        maxDuration: 800
      }
    });
  }
  
  // Step 3: Apply framework-specific optimizations
  if (context.platformContext === "MODERN-FRAMEWORK") {
    optimizations.push({
      type: "TREESHAKING",
      implementation: "Import only required functions",
      example: `
        // Preferred
        import { animate, stagger } from 'animejs';
        
        // Avoid
        import anime from 'animejs';
      `
    });
    
    optimizations.push({
      type: "CLEANUP",
      implementation: "Ensure animations are properly cleaned up on component unmount",
      example: `
        // React example
        useEffect(() => {
          const animation = animate(element, {...});
          
          return () => {
            animation.pause();
          };
        }, []);
      `
    });
  }
  
  // Step 4: Scroll optimization patterns
  if (context.animationComplexity.includes("SCROLL")) {
    optimizations.push({
      type: "SCROLL_OPTIMIZATION",
      implementation: "Implement intersection observer with debounce",
      example: `
        import { animate } from 'animejs';
        import { debounce } from 'lodash';
        
        // Create observer
        const observer = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if (entry.isIntersecting) {
              // Debounced animation trigger
              debounce(() => {
                animate(entry.target, {...});
              }, 100)();
            }
          });
        }, { threshold: 0.1 });
        
        // Observe elements
        document.querySelectorAll('.animated-element').forEach(el => {
          observer.observe(el);
        });
      `
    });
  }
  
  return optimizations;
}
```

### 3.4 Device Capability Detection

Use this code to determine device capability and adjust animations accordingly:

```javascript
function detectDeviceCapability() {
  // Create performance detection object
  const deviceCapability = {
    tier: "UNKNOWN",
    canRunComplexAnimations: false,
    recommendedMaxElements: 0,
    useHardwareAcceleration: true
  };
  
  // Step 1: Check for baseline performance indicators
  const memory = navigator.deviceMemory || 4; // Default to middle value if not available
  const processors = navigator.hardwareConcurrency || 4;
  const isReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  
  // Step 2: Run quick benchmark (simplified)
  const benchmark = () => {
    const startTime = performance.now();
    let counter = 0;
    
    while (performance.now() - startTime < 5) {
      counter++;
    }
    
    return counter;
  };
  
  const benchmarkScore = benchmark();
  
  // Step 3: Determine device tier
  if (isReducedMotion) {
    deviceCapability.tier = "LOW";
    deviceCapability.canRunComplexAnimations = false;
    deviceCapability.recommendedMaxElements = 5;
  } else if (memory <= 2 || processors <= 2 || benchmarkScore < 1000) {
    deviceCapability.tier = "LOW";
    deviceCapability.canRunComplexAnimations = false;
    deviceCapability.recommendedMaxElements = 50;
  } else if ((memory <= 4 && processors <= 4) || benchmarkScore < 2000) {
    deviceCapability.tier = "MEDIUM";
    deviceCapability.canRunComplexAnimations = true;
    deviceCapability.recommendedMaxElements = 200;
  } else {
    deviceCapability.tier = "HIGH";
    deviceCapability.canRunComplexAnimations = true;
    deviceCapability.recommendedMaxElements = 1000;
  }
  
  return deviceCapability;
}
```

## 4. Animation Pattern Implementation Models

### 4.1 Animation Type Classification System

LLMs should classify animation requirements using this taxonomy:

```json
{
  "animation_types": {
    "PROPERTY_ANIMATION": {
      "description": "Basic property changes over time",
      "complexity": "SIMPLE",
      "use_case": "Most UI animations, transitions",
      "implementation": "animate()"
    },
    "SEQUENCE_ANIMATION": {
      "description": "Multiple animations in sequence or parallel",
      "complexity": "INTERMEDIATE",
      "use_case": "Multi-step animations, complex UI transitions",
      "implementation": "createTimeline()"
    },
    "STAGGERED_ANIMATION": {
      "description": "Offset timing across multiple elements",
      "complexity": "INTERMEDIATE",
      "use_case": "Lists, grids, particle effects",
      "implementation": "stagger()"
    },
    "SCROLL_ANIMATION": {
      "description": "Animations triggered by scroll position",
      "complexity": "INTERMEDIATE", 
      "use_case": "Content reveals, parallax effects",
      "implementation": "Intersection Observer + animate()"
    },
    "SVG_ANIMATION": {
      "description": "Manipulating SVG paths and properties",
      "complexity": "ADVANCED",
      "use_case": "Logos, illustrations, data visualizations",
      "implementation": "createDrawable() or morphTo()"
    }
  }
}
```

### 4.2 Implementation Templates By Animation Type

#### 4.2.1 PROPERTY_ANIMATION Template

```javascript
/**
 * Function to create a property animation
 * 
 * @param {string|Element} targets - CSS selector or DOM element
 * @param {Object} properties - Animation properties to change
 * @param {number} duration - Animation duration in ms
 * @param {string} easing - Animation easing function
 * @param {boolean} autoplay - Whether animation should play immediately
 * @returns {Animation} Animation instance
 */
function createPropertyAnimation(targets, properties, duration = 1000, easing = 'easeOutQuad', autoplay = true) {
  // Step 1: Import required function
  import { animate } from 'animejs';
  
  // Step 2: Define base configuration
  const animationConfig = {
    // Required animation properties
    ...properties,
    
    // Animation timing properties
    duration: duration,
    easing: easing,
    autoplay: autoplay
  };
  
  // Step 3: Create and return animation
  return animate(targets, animationConfig);
}

// Example usage:
createPropertyAnimation('.element', {
  translateX: 250,
  opacity: [0, 1] // Array notation for [from, to] values
});
```

#### 4.2.2 SEQUENCE_ANIMATION Template

```javascript
/**
 * Function to create a timeline sequence animation
 * 
 * @param {Array} sequenceSteps - Array of animation step objects
 * @returns {Timeline} Timeline instance
 */
function createSequenceAnimation(sequenceSteps) {
  // Step 1: Import required function
  import { createTimeline } from 'animejs';
  
  // Step 2: Initialize timeline
  const timeline = createTimeline();
  
  // Step 3: Add each animation step to timeline
  sequenceSteps.forEach(step => {
    timeline.add(
      step.targets,
      step.properties,
      step.timePosition || '+=0' // Default to sequential
    );
  });
  
  // Step 4: Return timeline for further control
  return timeline;
}

// Example usage:
const sequence = [
  {
    targets: '.first-element',
    properties: {
      translateX: 250,
      duration: 1000
    }
  },
  {
    targets: '.second-element',
    properties: {
      translateY: 50,
      duration: 800
    },
    timePosition: '-=400' // Overlap with previous animation
  },
  {
    targets: '.third-element',
    properties: {
      rotate: 360,
      duration: 1200
    }
  }
];

createSequenceAnimation(sequence);
```

#### 4.2.3 STAGGERED_ANIMATION Template

```javascript
/**
 * Function to create a staggered animation across multiple elements
 * 
 * @param {string|Element} targets - CSS selector or DOM elements
 * @param {Object} properties - Animation properties
 * @param {number} staggerDelay - Delay between each element's animation
 * @param {Object} staggerOptions - Additional stagger configuration
 * @returns {Animation} Animation instance
 */
function createStaggeredAnimation(targets, properties, staggerDelay = 50, staggerOptions = {}) {
  // Step 1: Import required functions
  import { animate, stagger } from 'animejs';
  
  // Step 2: Define animation configuration with stagger
  const animationConfig = {
    ...properties,
    delay: stagger(staggerDelay, staggerOptions)
  };
  
  // Step 3: Create and return animation
  return animate(targets, animationConfig);
}

// Example usage:
createStaggeredAnimation('.item', {
  translateY: 50,
  opacity: [0, 1],
  duration: 800
}, 100, { from: 'center' });
```

#### 4.2.4 SCROLL_ANIMATION Template

```javascript
/**
 * Function to create scroll-triggered animations
 * 
 * @param {string} elements - CSS selector for elements to animate
 * @param {Object} animationProperties - Animation properties
 * @param {Object} observerOptions - Intersection Observer options
 * @param {boolean} animateOnce - Whether to animate only once
 */
function createScrollAnimation(elements, animationProperties, observerOptions = { threshold: 0.1 }, animateOnce = true) {
  // Step 1: Import required function
  import { animate } from 'animejs';
  
  // Step 2: Create intersection observer
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        // Step 3: Trigger animation when element enters viewport
        animate(entry.target, animationProperties);
        
        // Step 4: Unobserve if animateOnce is true
        if (animateOnce) {
          observer.unobserve(entry.target);
        }
      }
    });
  }, observerOptions);
  
  // Step 5: Start observing elements
  document.querySelectorAll(elements).forEach(el => {
    observer.observe(el);
  });
  
  // Step 6: Return observer for potential cleanup
  return observer;
}

// Example usage:
createScrollAnimation('.scroll-animate', {
  translateY: [50, 0],
  opacity: [0, 1],
  duration: 800,
  easing: 'easeOutQuad'
});
```

#### 4.2.5 SVG_ANIMATION Template

```javascript
/**
 * Function to create SVG path drawing animation
 * 
 * @param {string} svgPathSelector - CSS selector for SVG path
 * @param {number} duration - Animation duration in ms
 * @param {string} easing - Animation easing function
 * @returns {Animation} Animation instance
 */
function createSVGPathAnimation(svgPathSelector, duration = 2000, easing = 'easeInOutSine') {
  // Step 1: Import required functions
  import { animate, createDrawable } from 'animejs';
  
  // Step 2: Make SVG path drawable
  const drawable = createDrawable(svgPathSelector);
  
  // Step 3: Create drawing animation
  return animate(drawable, {
    draw: [0, 1], // From 0% to 100% of path
    duration: duration,
    easing: easing
  });
}

// Example usage:
createSVGPathAnimation('.svg-path');
```

## 5. Compatibility Analysis and Decision Framework

### 5.1 Browser Compatibility Matrix

LLMs should use this compatibility database when making implementation decisions:

```json
{
  "browser_compatibility": {
    "Chrome": {
      "minimum_version": 55,
      "notes": "Full support for all features",
      "market_share": "65%",
      "performance_rating": "HIGH"
    },
    "Firefox": {
      "minimum_version": 52,
      "notes": "Full support for all features",
      "market_share": "4%",
      "performance_rating": "HIGH"
    },
    "Safari": {
      "minimum_version": 11,
      "notes": "Full support, occasional SVG rendering differences",
      "market_share": "19%",
      "performance_rating": "MEDIUM"
    },
    "Edge": {
      "minimum_version": 15,
      "notes": "Full support for all features",
      "market_share": "4%",
      "performance_rating": "HIGH"
    },
    "IE11": {
      "supported": false,
      "notes": "Not supported by anime.js v4",
      "market_share": "<1%",
      "fallback": "Use polyfills or simpler CSS animations"
    }
  }
}
```

### 5.2 Device Adaptation Decision Algorithm

LLMs should implement this algorithm for device-responsive animations:

```javascript
/**
 * Creates a device-adaptive animation strategy
 * 
 * @param {Object} animationConfig - Base animation configuration
 * @returns {Object} Device-optimized animation strategy
 */
function createAdaptiveAnimationStrategy(animationConfig) {
  // Step 1: Import required functions
  import { createScope, animate } from 'animejs';
  
  // Step 2: Define adaptation parameters
  const deviceStrategies = {
    // Breakpoints based on common device sizes
    breakpoints: {
      mobile: '(max-width: 767px)',
      tablet: '(min-width: 768px) and (max-width: 1023px)',
      desktop: '(min-width: 1024px)'
    },
    
    // Device-specific animation adjustments
    adaptations: {
      mobile: {
        duration: value => Math.min(value, 800), // Cap durations
        translateDistance: value => value * 0.5, // Reduce movement
        complexity: 'SIMPLIFIED'
      },
      tablet: {
        duration: value => value * 0.8, // Slightly reduce durations
        translateDistance: value => value * 0.8, // Slightly reduce movement
        complexity: 'MODERATE'
      },
      desktop: {
        duration: value => value, // Full duration
        translateDistance: value => value, // Full movement
        complexity: 'FULL'
      }
    },
    
    // Reduced motion preferences
    reducedMotion: {
      duration: value => Math.min(value, 300),
      translateDistance: value => 0, // No movement
      opacity: true, // Only use opacity
      complexity: 'MINIMAL'
    }
  };
  
  // Step 3: Create adaptive strategy
  const adaptiveStrategy = {
    // Initialize scope with media queries
    scope: createScope({
      mediaQueries: {
        ...deviceStrategies.breakpoints,
        reducedMotion: '(prefers-reduced-motion: reduce)'
      }
    }),
    
    // Apply device-specific adaptations
    applyAnimation: function() {
      this.scope.add(({ matches }) => {
        // Determine device type
        let deviceType = 'desktop'; // Default
        if (matches.mobile) deviceType = 'mobile';
        else if (matches.tablet) deviceType = 'tablet';
        
        // Check for reduced motion preference
        const adaptation = matches.reducedMotion 
          ? deviceStrategies.reducedMotion 
          : deviceStrategies.adaptations[deviceType];
        
        // Apply adapted animation properties
        const adaptedConfig = adaptAnimationConfig(animationConfig, adaptation);
        
        // Execute animation with adapted config
        return animate(animationConfig.targets, adaptedConfig);
      });
      
      return this;
    }
  };
  
  // Helper function to transform config based on adaptation
  function adaptAnimationConfig(config, adaptation) {
    const adapted = { ...config };
    
    // Apply duration adaptation
    if (adapted.duration) {
      adapted.duration = adaptation.duration(adapted.duration);
    }
    
    // Apply translate adaptations
    ['translateX', 'translateY', 'translateZ'].forEach(prop => {
      if (adapted[prop]) {
        // Handle both single values and arrays
        if (Array.isArray(adapted[prop])) {
          adapted[prop] = adapted[prop].map(val => 
            typeof val === 'number' ? adaptation.translateDistance(val) : val
          );
        } else if (typeof adapted[prop] === 'number') {
          adapted[prop] = adaptation.translateDistance(adapted[prop]);
        }
      }
    });
    
    // Simplified animations for reduced motion
    if (adaptation.complexity === 'MINIMAL') {
      // Keep only opacity transitions if reduced motion is preferred
      const preservedProps = ['opacity', 'duration', 'easing', 'targets'];
      Object.keys(adapted).forEach(key => {
        if (!preservedProps.includes(key)) {
          delete adapted[key];
        }
      });
    }
    
    return adapted;
  }
  
  return adaptiveStrategy;
}

// Example usage:
const baseAnimation = {
  targets: '.animated-element',
  translateY: [100, 0],
  opacity: [0, 1],
  duration: 1200,
  easing: 'easeOutQuad'
};

createAdaptiveAnimationStrategy(baseAnimation).applyAnimation();
```

### 5.3 Compatibility Testing Protocol

LLMs should recommend this systematic testing process:

1. **Automated Browser Testing:**
   - Use playwright or cypress to test animations across:
     - Chrome (latest, latest-1)
     - Firefox (latest, latest-1)
     - Safari (latest, latest-1)
     - Edge (latest)

2. **Device Testing Matrix:**
   - Test on minimum 6 device configurations:
     - Low-end Android device (e.g., budget phones)
     - Mid-range Android device
     - High-end Android device
     - Older iOS device (2-3 years old)
     - Current iOS device
     - Tablet device

3. **Performance Metrics Collection:**
   - Capture and analyze:
     - Frame rate (target: consistent 60fps)
     - CPU usage (target: <30% during animations)
     - Memory usage (no significant increase during animations)
     - Battery impact (no significant drain)

4. **Accessibility Compliance:**
   - Verify animations respect reduced motion settings
   - Ensure animations don't trigger vestibular disorders
   - Confirm no flashing content that could trigger seizures

## 6. Performance Analysis and Optimization Framework

### 6.1 Systematic Performance Testing Protocol

LLMs should implement this structured testing methodology:

```javascript
/**
 * Animation Performance Testing Framework
 * 
 * A systematic approach for measuring and optimizing anime.js animations
 */
class AnimationPerformanceAnalyzer {
  /**
   * Initialize the analyzer
   * @param {string} testName - Identifier for the test
   */
  constructor(testName) {
    this.testName = testName;
    this.metrics = {
      fps: [],
      cpuUsage: [],
      memoryUsage: [],
      renderTime: [],
      animationDuration: null,
      elementsAnimated: 0,
      propertiesAnimated: 0
    };
    this.testResults = null;
  }
  
  /**
   * Configure the test parameters
   * @param {Object} config - Test configuration
   */
  configureTest(config) {
    this.config = {
      animationTargets: config.targets || '.test-element',
      animationProperties: config.properties || { translateX: 100 },
      duration: config.duration || 1000,
      iterations: config.iterations || 5,
      elementsCount: config.elementsCount || 1,
      propertiesCount: Object.keys(config.properties || { translateX: 100 }).length,
      ...config
    };
    
    this.metrics.elementsAnimated = this.config.elementsCount;
    this.metrics.propertiesAnimated = this.config.propertiesCount;
    
    return this;
  }
  
  /**
   * Run the performance test
   * @returns {Promise} Promise resolving to test results
   */
  async runTest() {
    // Step 1: Import required functions
    import { animate } from 'animejs';
    
    // Step 2: Initialize performance monitoring
    const fpsMonitor = new FPSMonitor();
    const resourceMonitor = new ResourceMonitor();
    
    // Step 3: Run the animation test for specified iterations
    for (let i = 0; i < this.config.iterations; i++) {
      console.log(`Running test iteration ${i+1}/${this.config.iterations}`);
      
      // Start monitoring
      fpsMonitor.start();
      resourceMonitor.start();
      
      // Record start time
      const startTime = performance.now();
      
      // Create and run animation
      const animation = animate(this.config.animationTargets, {
        ...this.config.animationProperties,
        duration: this.config.duration
      });
      
      // Wait for animation to complete
      await animation.finished;
      
      // Record end time
      const endTime = performance.now();
      
      // Stop monitoring
      const fpsData = fpsMonitor.stop();
      const resourceData = resourceMonitor.stop();
      
      // Store metrics
      this.metrics.fps.push(fpsData.averageFPS);
      this.metrics.cpuUsage.push(resourceData.cpuUsage);
      this.metrics.memoryUsage.push(resourceData.memoryUsage);
      this.metrics.renderTime.push(endTime - startTime);
    }
    
    // Step 4: Analyze results
    this.testResults = this.analyzeResults();
    return this.testResults;
  }
  
  /**
   * Analyze collected metrics
   * @returns {Object} Analysis results
   */
  analyzeResults() {
    const calculateAverage = arr => arr.reduce((a, b) => a + b, 0) / arr.length;
    
    // Calculate averages
    const avgFPS = calculateAverage(this.metrics.fps);
    const avgCPU = calculateAverage(this.metrics.cpuUsage);
    const avgMemory = calculateAverage(this.metrics.memoryUsage);
    const avgRenderTime = calculateAverage(this.metrics.renderTime);
    
    // Calculate performance score (0-100)
    const fpsScore = Math.min(100, (avgFPS / 60) * 100);
    const renderScore = Math.max(0, 100 - (avgRenderTime / this.config.duration * 100));
    const cpuScore = Math.max(0, 100 - (avgCPU * 2));
    
    const overallScore = (fpsScore * 0.5) + (renderScore * 0.3) + (cpuScore * 0.2);
    
    // Determine performance classification
    let performanceClassification;
    if (overallScore >= 90) performanceClassification = "EXCELLENT";
    else if (overallScore >= 75) performanceClassification = "GOOD";
    else if (overallScore >= 60) performanceClassification = "ACCEPTABLE";
    else if (overallScore >= 40) performanceClassification = "POOR";
    else performanceClassification = "CRITICAL";
    
    // Generate optimization recommendations
    const optimizationRecommendations = this.generateOptimizationRecommendations({
      fps: avgFPS,
      renderTime: avgRenderTime,
      cpuUsage: avgCPU,
      score: overallScore
    });
    
    return {
      testName: this.testName,
      metrics: {
        averageFPS: avgFPS,
        averageCPUUsage: avgCPU,
        averageMemoryUsage: avgMemory,
        averageRenderTime: avgRenderTime,
        elementsAnimated: this.metrics.elementsAnimated,
        propertiesAnimated: this.metrics.propertiesAnimated
      },
      performance: {
        score: overallScore,
        classification: performanceClassification,
        fpsScore,
        renderScore,
        cpuScore
      },
      optimizationRecommendations
    };
  }
  
  /**
   * Generate optimization recommendations based on metrics
   * @param {Object} metrics - Performance metrics
   * @returns {Array} List of recommendations
   */
  generateOptimizationRecommendations(metrics) {
    const recommendations = [];
    
    // FPS-based recommendations
    if (metrics.fps < 45) {
      recommendations.push({
        priority: "HIGH",
        issue: "Low frame rate",
        recommendation: "Reduce animation complexity or number of animated elements",
        implementation: "Use hardware-accelerated properties only (transform, opacity)"
      });
    }
    
    // Render time recommendations
    if (metrics.renderTime > this.config.duration * 0.8) {
      recommendations.push({
        priority: "HIGH",
        issue: "Animation rendering takes too long",
        recommendation: "Simplify animation or reduce duration",
        implementation: "Consider using WAAPI version for simple animations"
      });
    }
    
    // CPU usage recommendations
    if (metrics.cpuUsage > 30) {
      recommendations.push({
        priority: "MEDIUM",
        issue: "High CPU usage",
        recommendation: "Optimize animation to reduce CPU load",
        implementation: "Batch DOM operations and use requestAnimationFrame"
      });
    }
    
    // General recommendations
    if (this.metrics.elementsAnimated > 100) {
      recommendations.push({
        priority: "MEDIUM",
        issue: "Large number of animated elements",
        recommendation: "Consider reducing animated elements or staggering animations",
        implementation: "Animate only visible elements or use more efficient selectors"
      });
    }
    
    return recommendations;
  }
}

/**
 * FPS monitoring utility (implementation placeholder)
 */
class FPSMonitor {
  constructor() {
    this.frames = 0;
    this.startTime = 0;
    this.lastFrameTime = 0;
    this.frameRates = [];
    this.isMonitoring = false;
  }
  
  start() {
    this.startTime = performance.now();
    this.lastFrameTime = this.startTime;
    this.frames = 0;
    this.frameRates = [];
    this.isMonitoring = true;
    this.monitorFrame();
  }
  
  monitorFrame() {
    if (!this.isMonitoring) return;
    
    const currentTime = performance.now();
    const deltaTime = currentTime - this.lastFrameTime;
    
    this.frames++;
    
    if (deltaTime >= 1000) {
      const fps = Math.round((this.frames * 1000) / deltaTime);
      this.frameRates.push(fps);
      this.frames = 0;
      this.lastFrameTime = currentTime;
    }
    
    requestAnimationFrame(() => this.monitorFrame());
  }
  
  stop() {
    this.isMonitoring = false;
    
    const averageFPS = this.frameRates.length > 0
      ? this.frameRates.reduce((a, b) => a + b, 0) / this.frameRates.length
      : 60;
      
    return {
      frameRates: this.frameRates,
      averageFPS: averageFPS,
      duration: performance.now() - this.startTime
    };
  }
}

/**
 * Resource usage monitoring utility (implementation placeholder)
 */
class ResourceMonitor {
  constructor() {
    this.startMemory = 0;
    this.cpuMeasurements = [];
    this.memoryMeasurements = [];
    this.isMonitoring = false;
  }
  
  async start() {
    // Note: In real implementation, would use Performance API
    // This is a simplified placeholder
    this.startMemory = window.performance && window.performance.memory 
      ? window.performance.memory.usedJSHeapSize 
      : 0;
    
    this.cpuMeasurements = [];
    this.memoryMeasurements = [];
    this.isMonitoring = true;
    
    this.monitorResources();
  }
  
  async monitorResources() {
    if (!this.isMonitoring) return;
    
    // Simplified CPU usage simulation (would use real metrics in production)
    const simulatedCPUUsage = Math.random() * 20 + 10; // 10-30%
    this.cpuMeasurements.push(simulatedCPUUsage);
    
    // Memory tracking if available
    if (window.performance && window.performance.memory) {
      const memoryUsage = window.performance.memory.usedJSHeapSize;
      this.memoryMeasurements.push(memoryUsage);
    }
    
    setTimeout(() => this.monitorResources(), 250);
  }
  
  stop() {
    this.isMonitoring = false;
    
    const avgCPU = this.cpuMeasurements.length > 0
      ? this.cpuMeasurements.reduce((a, b) => a + b, 0) / this.cpuMeasurements.length
      : 0;
      
    const avgMemory = this.memoryMeasurements.length > 0
      ? this.memoryMeasurements.reduce((a, b) => a + b, 0) / this.memoryMeasurements.length
      : 0;
      
    const memoryDelta = avgMemory - this.startMemory;
    
    return {
      cpuUsage: avgCPU,
      memoryUsage: avgMemory,
      memoryDelta: memoryDelta
    };
  }
}

// Example usage:
const analyzer = new AnimationPerformanceAnalyzer('Homepage Hero Animation');

analyzer.configureTest({
  targets: '.hero-element',
  properties: {
    translateY: [100, 0],
    opacity: [0, 1]
  },
  duration: 1000,
  iterations: 3,
  elementsCount: 5
}).runTest().then(results => {
  console.log('Test Results:', results);
  
  // Implement optimization recommendations
  if (results.performance.classification !== 'EXCELLENT') {
    results.optimizationRecommendations.forEach(rec => {
      console.log(`Implementing ${rec.priority} priority optimization: ${rec.recommendation}`);
      // Apply optimizations based on recommendations
    });
  }
});
```

### 6.2 Visual Verification Protocol

LLMs should implement this visual testing approach:

```javascript
/**
 * Visual Regression Testing System for Animations
 * 
 * Captures and compares animation states across browsers
 */
class AnimationVisualTester {
  /**
   * Initialize the visual tester
   * @param {string} testName - Test identifier
   */
  constructor(testName) {
    this.testName = testName;
    this.visualStates = [];
    this.referenceStates = {};
    this.diffResults = {};
  }
  
  /**
   * Record animation visual states at specified intervals
   * @param {Object} animation - Anime.js animation object
   * @param {number} frames - Number of frames to capture
   * @returns {Promise} Promise resolving with captured states
   */
  async captureAnimationStates(animation, frames = 5) {
    // Implementation would use canvas capture or screenshot API
    // This is a simplified conceptual version
    
    this.visualStates = [];
    
    // Step 1: Record initial state
    this.visualStates.push({
      progress: 0,
      timestamp: performance.now(),
      state: "INITIAL_STATE_REPRESENTATION"
    });
    
    // Step 2: Start animation
    animation.play();
    
    // Step 3: Capture states at intervals
    const interval = animation.duration / frames;
    
    for (let i = 1; i <= frames; i++) {
      await new Promise(resolve => setTimeout(resolve, interval));
      
      this.visualStates.push({
        progress: i / frames,
        timestamp: performance.now(),
        state: `STATE_REPRESENTATION_AT_${i/frames * 100}%`
      });
    }
    
    // Step 4: Return captured states
    return this.visualStates;
  }
  
  /**
   * Compare captured states with reference states
   * @param {string} browser - Browser identifier
   * @returns {Object} Comparison results
   */
  compareWithReference(browser) {
    if (!this.referenceStates[browser]) {
      console.warn(`No reference states found for ${browser}`);
      return null;
    }
    
    const results = {
      browser,
      testName: this.testName,
      stateComparisons: [],
      overallDifference: 0
    };
    
    // Compare each state with reference
    this.visualStates.forEach((state, index) => {
      const reference = this.referenceStates[browser][index];
      
      if (!reference) {
        console.warn(`No reference state at index ${index} for ${browser}`);
        return;
      }
      
      // Calculate visual difference (implementation placeholder)
      const difference = Math.random() * 0.1; // 0-10% difference simulation
      
      results.stateComparisons.push({
        progress: state.progress,
        difference: difference,
        passed: difference < 0.05 // Consider 5% difference acceptable
      });
      
      results.overallDifference += difference;
    });
    
    results.overallDifference /= this.visualStates.length;
    results.passed = results.overallDifference < 0.05;
    
    this.diffResults[browser] = results;
    return results;
  }
  
  /**
   * Save current states as reference for a browser
   * @param {string} browser - Browser identifier
   */
  saveAsReference(browser) {
    this.referenceStates[browser] = [...this.visualStates];
    console.log(`Saved reference states for ${browser}`);
  }
  
  /**
   * Generate visual test report
   * @returns {Object} Test report
   */
  generateReport() {
    const browsers = Object.keys(this.diffResults);
    
    const report = {
      testName: this.testName,
      timestamp: new Date().toISOString(),
      browsers: browsers,
      results: this.diffResults,
      passed: browsers.every(browser => this.diffResults[browser].passed)
    };
    
    return report;
  }
}

// Example usage:
async function testAnimationVisually(animationConfig) {
  import { animate } from 'animejs';
  
  const visualTester = new AnimationVisualTester('Button Hover Animation');
  
  // Create animation but don't start it yet
  const animation = animate('.button', {
    ...animationConfig,
    autoplay: false
  });
  
  // Capture states
  const states = await visualTester.captureAnimationStates(animation, 5);
  console.log(`Captured ${states.length} visual states`);
  
  // Compare with reference if available
  const browserName = getBrowserName(); // Helper function to detect browser
  const comparisonResults = visualTester.compareWithReference(browserName);
  
  if (comparisonResults) {
    console.log('Visual comparison results:', comparisonResults);
    
    if (!comparisonResults.passed) {
      console.warn('Visual regression detected!');
      // Implement reporting or alerting
    }
  } else {
    // Save as reference if no comparison available
    visualTester.saveAsReference(browserName);
  }
  
  return visualTester.generateReport();
}

// Helper function to get browser name
function getBrowserName() {
  const userAgent = navigator.userAgent;
  if (userAgent.indexOf("Chrome") > -1) return "Chrome";
  if (userAgent.indexOf("Safari") > -1) return "Safari";
  if (userAgent.indexOf("Firefox") > -1) return "Firefox";
  if (userAgent.indexOf("MSIE") > -1) return "IE";
  if (userAgent.indexOf("Edg") > -1) return "Edge";
  return "Unknown";
}
```

## 7. Animation Pattern Library and Decision Trees

### 7.1 Animation Pattern Classification System

LLMs should use this pattern classification system when generating animation implementations:

```json
{
  "pattern_library": {
    "PAGE_TRANSITION": {
      "description": "Animations for navigating between pages or views",
      "complexity": "INTERMEDIATE",
      "performance_impact": "MEDIUM",
      "use_cases": ["SPA navigation", "Multi-page applications", "Tab interfaces"],
      "variations": ["FADE", "SLIDE", "SCALE", "FLIP"]
    },
    "MICRO_INTERACTION": {
      "description": "Small animations that provide feedback for user actions",
      "complexity": "SIMPLE",
      "performance_impact": "LOW",
      "use_cases": ["Button feedback", "Form interactions", "Hover states"],
      "variations": ["SCALE", "COLOR", "BOUNCE", "PULSE"]
    },
    "LOADING_INDICATOR": {
      "description": "Animations that indicate content or data is being loaded",
      "complexity": "SIMPLE to INTERMEDIATE",
      "performance_impact": "LOW to MEDIUM",
      "use_cases": ["API requests", "Content loading", "Processing states"],
      "variations": ["SPINNER", "PROGRESS", "SKELETON", "DOTS"]
    },
    "CONTENT_REVEAL": {
      "description": "Animations that introduce content elements as they enter viewport",
      "complexity": "INTERMEDIATE",
      "performance_impact": "MEDIUM",
      "use_cases": ["Scrolling content", "Progressive disclosure", "Narrative sites"],
      "variations": ["FADE_IN", "SLIDE_UP", "STAGGERED", "TRANSFORM"]
    },
    "DATA_VISUALIZATION": {
      "description": "Animations for charts, graphs, and data representations",
      "complexity": "ADVANCED",
      "performance_impact": "HIGH",
      "use_cases": ["Charts", "Dashboards", "Infographics"],
      "variations": ["PROGRESSIVE", "REACTIVE", "MORPHING", "COMPARATIVE"]
    }
  }
}
```

### 7.2 Implementation Decision Trees

#### 7.2.1 Page Transition Decision Tree

```
START PAGE_TRANSITION
├── Is this a Single Page Application (SPA)?
│   ├── YES
│   │   ├── Does the framework have built-in transition mechanisms?
│   │   │   ├── YES (e.g. Vue, Svelte)
│   │   │   │   └── IMPLEMENT: Framework-specific transition + anime.js enhancement
│   │   │   └── NO (e.g. React without additional libraries)
│   │   │       └── IMPLEMENT: Router integration pattern
│   │   └── Are nested transitions required?
│   │       ├── YES
│   │       │   └── IMPLEMENT: Nested timeline pattern
│   │       └── NO
│   │           └── IMPLEMENT: Basic exit/enter pattern
│   └── NO (Multi-page site)
│       ├── Is JavaScript navigation used (prevent default)?
│       │   ├── YES
│       │   │   └── IMPLEMENT: Full page transition pattern
│       │   └── NO
│       │       └── IMPLEMENT: Enter-only animation pattern
└── END
```

**Implementation Template for SPA Basic Exit/Enter Pattern:**

```javascript
/**
 * Creates a reusable page transition system for SPAs
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Page transition controller
 */
function createPageTransitionSystem(options = {}) {
  // Step 1: Import required functions
  import { animate, createTimeline } from 'animejs';
  
  // Step 2: Merge default options with provided options
  const config = {
    // Selectors
    oldPageSelector: options.oldPageSelector || '.page-content.active',
    newPageSelector: options.newPageSelector || '.page-content.next',
    
    // Animation properties
    exitDuration: options.exitDuration || 300,
    enterDuration: options.enterDuration || 500,
    exitEasing: options.exitEasing || 'easeInQuad',
    enterEasing: options.enterEasing || 'easeOutQuad',
    
    // Animation style
    transition: options.transition || 'FADE_SLIDE', // FADE, SLIDE, FADE_SLIDE, SCALE
    stagger: options.stagger || 0,
    
    // Callback hooks
    onBeforeExit: options.onBeforeExit || (() => {}),
    onAfterExit: options.onAfterExit || (() => {}),
    onBeforeEnter: options.onBeforeEnter || (() => {}),
    onAfterEnter: options.onAfterEnter || (() => {}),
    
    ...options
  };
  
  // Step 3: Define transition animations based on selected style
  const transitions = {
    FADE: {
      exit: { opacity: [1, 0] },
      enter: { opacity: [0, 1] }
    },
    SLIDE: {
      exit: { translateX: [0, -50] },
      enter: { translateX: [50, 0] }
    },
    FADE_SLIDE: {
      exit: { opacity: [1, 0], translateY: [0, -20] },
      enter: { opacity: [0, 1], translateY: [20, 0] }
    },
    SCALE: {
      exit: { opacity: [1, 0], scale: [1, 0.9] },
      enter: { opacity: [0, 1], scale: [0.9, 1] }
    }
  };
  
  // Get the appropriate transition style
  const transitionStyle = transitions[config.transition] || transitions.FADE_SLIDE;
  
  // Step 4: Create transition methods
  const controller = {
    /**
     * Animate exiting the current page
     * @returns {Promise} Promise that resolves when exit is complete
     */
    exitPage: async () => {
      // Call pre-exit hook
      config.onBeforeExit();
      
      // Get current page element
      const currentPage = document.querySelector(config.oldPageSelector);
      if (!currentPage) return Promise.resolve();
      
      // Create and run exit animation
      const exitAnimation = animate(currentPage, {
        ...transitionStyle.exit,
        duration: config.exitDuration,
        easing: config.exitEasing
      });
      
      // Wait for animation to complete
      await exitAnimation.finished;
      
      // Call post-exit hook
      config.onAfterExit();
      
      return exitAnimation;
    },
    
    /**
     * Animate entering the new page
     * @returns {Promise} Promise that resolves when enter is complete
     */
    enterPage: async () => {
      // Call pre-enter hook
      config.onBeforeEnter();
      
      // Get new page element
      const newPage = document.querySelector(config.newPageSelector);
      if (!newPage) return Promise.resolve();
      
      // Set initial state
      newPage.style.opacity = '0';
      newPage.classList.remove('next');
      newPage.classList.add('active');
      
      // Create and run enter animation
      const enterAnimation = animate(newPage, {
        ...transitionStyle.enter,
        duration: config.enterDuration,
        easing: config.enterEasing
      });
      
      // Wait for animation to complete
      await enterAnimation.finished;
      
      // Call post-enter hook
      config.onAfterEnter();
      
      return enterAnimation;
    },
    
    /**
     * Perform a complete page transition
     * @param {Function} updateContentFn - Function to update page content
     * @returns {Promise} Promise that resolves when transition is complete
     */
    transition: async (updateContentFn) => {
      await controller.exitPage();
      
      // Update content between animations
      if (typeof updateContentFn === 'function') {
        await updateContentFn();
      }
      
      return controller.enterPage();
    }
  };
  
  return controller;
}

// Example usage:
const pageTransition = createPageTransitionSystem({
  transition: 'FADE_SLIDE',
  exitDuration: 300,
  enterDuration: 500
});

// Integration with router or navigation system
document.querySelectorAll('a[data-transition]').forEach(link => {
  link.addEventListener('click', async (e) => {
    e.preventDefault();
    const href = link.getAttribute('href');
    
    await pageTransition.transition(async () => {
      // Fetch new content
      const response = await fetch(href);
      const html = await response.text();
      
      // Update DOM with new content
      const tempContainer = document.createElement('div');
      tempContainer.innerHTML = html;
      
      const newContent = tempContainer.querySelector('.page-content');
      newContent.classList.add('next');
      
      document.body.appendChild(newContent);
    });
    
    // Update history
    window.history.pushState({}, '', href);
  });
});

// Handle browser back/forward navigation
window.addEventListener('popstate', async () => {
  await pageTransition.transition(async () => {
    // Fetch content for current URL
    const response = await fetch(window.location.href);
    const html = await response.text();
    
    // Update DOM with new content
    const tempContainer = document.createElement('div');
    tempContainer.innerHTML = html;
    
    const newContent = tempContainer.querySelector('.page-content');
    newContent.classList.add('next');
    
    document.body.appendChild(newContent);
  });
});
```

#### 7.2.2 Micro-Interaction Implementation Template

```javascript
/**
 * Factory function to create reusable micro-interactions
 * 
 * @param {string|Element} targets - Elements to apply interactions to
 * @param {Object} options - Configuration options
 * @returns {Object} Micro-interaction controller
 */
function createMicroInteraction(targets, options = {}) {
  // Step 1: Import required function
  import { animate } from 'animejs';
  
  // Step 2: Configure with sensible defaults
  const config = {
    // Interaction types
    type: options.type || 'hover', // hover, click, focus
    
    // Animation properties
    duration: {
      in: options.duration?.in || 250,
      out: options.duration?.out || 200
    },
    easing: {
      in: options.easing?.in || 'easeOutQuad',
      out: options.easing?.out || 'easeInOutQuad'
    },
    
    // Animation effects
    effects: options.effects || [{
      property: 'scale',
      from: 1,
      to: 1.05
    }],
    
    // Additional options
    autoplay: options.autoplay !== undefined ? options.autoplay : false,
    event: options.event || null, // Custom event name
    threshold: options.threshold || 0, // For interaction observers
    
    ...options
  };
  
  // Step 3: Store element references
  const elements = typeof targets === 'string' 
    ? document.querySelectorAll(targets) 
    : [targets].flat();
  
  // Step 4: Create animations based on effects
  const createAnimationConfig = (direction) => {
    const animationConfig = {
      duration: config.duration[direction],
      easing: config.easing[direction]
    };
    
    // Apply each effect to the configuration
    config.effects.forEach(effect => {
      const [fromValue, toValue] = direction === 'in' 
        ? [effect.from, effect.to] 
        : [effect.to, effect.from];
      
      // Handle array values or single values
      animationConfig[effect.property] = Array.isArray(fromValue) 
        ? [fromValue, toValue] 
        : [fromValue, toValue];
    });
    
    return animationConfig;
  };
  
  // Step 5: Initialize and return interaction controller
  const controller = {
    elements,
    animations: {
      in: [],
      out: []
    },
    active: false,
    
    /**
     * Animate elements to active state
     * @returns {Array} Animation objects
     */
    animateIn: () => {
      if (controller.active) return;
      controller.active = true;
      
      controller.animations.in = Array.from(elements).map(element => 
        animate(element, createAnimationConfig('in'))
      );
      
      return controller.animations.in;
    },
    
    /**
     * Animate elements to inactive state
     * @returns {Array} Animation objects
     */
    animateOut: () => {
      if (!controller.active) return;
      controller.active = false;
      
      controller.animations.out = Array.from(elements).map(element => 
        animate(element, createAnimationConfig('out'))
      );
      
      return controller.animations.out;
    },
    
    /**
     * Toggle animation state
     * @returns {Array} Animation objects
     */
    toggle: () => {
      return controller.active ? controller.animateOut() : controller.animateIn();
    },
    
    /**
     * Initialize interaction event listeners
     * @returns {Object} Controller object for chaining
     */
    init: () => {
      // Add appropriate event listeners based on interaction type
      if (config.type === 'hover') {
        elements.forEach(element => {
          element.addEventListener('mouseenter', controller.animateIn);
          element.addEventListener('mouseleave', controller.animateOut);
          element.addEventListener('touchstart', controller.animateIn, { passive: true });
          element.addEventListener('touchend', controller.animateOut);
        });
      } else if (config.type === 'click') {
        elements.forEach(element => {
          element.addEventListener('click', controller.toggle);
        });
      } else if (config.type === 'focus') {
        elements.forEach(element => {
          element.addEventListener('focus', controller.animateIn);
          element.addEventListener('blur', controller.animateOut);
        });
      } else if (config.event) {
        // Custom event support
        elements.forEach(element => {
          element.addEventListener(config.event, controller.toggle);
        });
      }
      
      // Auto-play if configured
      if (config.autoplay) {
        controller.animateIn();
      }
      
      return controller;
    },
    
    /**
     * Remove event listeners and clean up
     * @returns {Object} Controller object for chaining
     */
    destroy: () => {
      if (config.type === 'hover') {
        elements.forEach(element => {
          element.removeEventListener('mouseenter', controller.animateIn);
          element.removeEventListener('mouseleave', controller.animateOut);
          element.removeEventListener('touchstart', controller.animateIn);
          element.removeEventListener('touchend', controller.animateOut);
        });
      } else if (config.type === 'click') {
        elements.forEach(element => {
          element.removeEventListener('click', controller.toggle);
        });
      } else if (config.type === 'focus') {
        elements.forEach(element => {
          element.removeEventListener('focus', controller.animateIn);
          element.removeEventListener('blur', controller.animateOut);
        });
      } else if (config.event) {
        elements.forEach(element => {
          element.removeEventListener(config.event, controller.toggle);
        });
      }
      
      return controller;
    }
  };
  
  // Initialize if not explicitly requested to delay
  if (!config.delayInit) {
    controller.init();
  }
  
  return controller;
}

// Example usage:
// Button hover effect
const buttonHover = createMicroInteraction('.button', {
  type: 'hover',
  effects: [
    { property: 'scale', from: 1, to: 1.05 },
    { property: 'boxShadow', from: '0 2px 4px rgba(0,0,0,0.1)', to: '0 4px 8px rgba(0,0,0,0.2)' }
  ],
  duration: { in: 250, out: 200 }
});

// Click ripple effect
const clickRipple = createMicroInteraction('.ripple-button', {
  type: 'click',
  effects: [
    { property: 'scale', from: 0, to: 1 },
    { property: 'opacity', from: 1, to: 0 }
  ],
  duration: { in: 400, out: 0 },
  easing: { in: 'easeOutQuad', out: 'linear' }
});
```

#### 7.2.3 Loading Animation Implementation Template

```javascript
/**
 * Factory function to create loading state animations
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Loading animation controller
 */
function createLoadingAnimation(options = {}) {
  // Step 1: Import required functions
  import { animate, stagger } from 'animejs';
  
  // Step 2: Configure with defaults
  const config = {
    // Animation target(s)
    target: options.target || '.loading-indicator',
    
    // Animation type
    type: options.type || 'dots', // dots, spinner, progress, pulse
    
    // Animation properties
    duration: options.duration || 800,
    loop: options.loop !== undefined ? options.loop : true,
    autoplay: options.autoplay !== undefined ? options.autoplay : true,
    
    // Element generation
    generateHTML: options.generateHTML !== undefined ? options.generateHTML : false,
    container: options.container || document.body,
    
    // Style configurations
    dotCount: options.dotCount || 3,
    size: options.size || 'medium', // small, medium, large
    color: options.color || '#3498db',
    
    // Callbacks
    onComplete: options.onComplete || null,
    
    ...options
  };
  
  // Step 3: Generate HTML if requested
  let targetElement;
  
  if (config.generateHTML) {
    targetElement = document.createElement('div');
    targetElement.classList.add('loading-indicator', `loading-${config.type}`);
    
    // Apply size class
    targetElement.classList.add(`size-${config.size}`);
    
    // Apply type-specific HTML structure
    if (config.type === 'dots') {
      for (let i = 0; i < config.dotCount; i++) {
        const dot = document.createElement('div');
        dot.classList.add('loading-dot');
        dot.style.backgroundColor = config.color;
        targetElement.appendChild(dot);
      }
    } else if (config.type === 'spinner') {
      const spinner = document.createElement('div');
      spinner.classList.add('spinner-circle');
      spinner.style.borderColor = `rgba(0, 0, 0, 0.1)`;
      spinner.style.borderTopColor = config.color;
      targetElement.appendChild(spinner);
    } else if (config.type === 'progress') {
      const bar = document.createElement('div');
      bar.classList.add('progress-bar');
      
      const fill = document.createElement('div');
      fill.classList.add('progress-fill');
      fill.style.backgroundColor = config.color;
      
      bar.appendChild(fill);
      targetElement.appendChild(bar);
    } else if (config.type === 'pulse') {
      const pulse = document.createElement('div');
      pulse.classList.add('pulse-circle');
      pulse.style.backgroundColor = config.color;
      targetElement.appendChild(pulse);
    }
    
    // Add to container
    config.container.appendChild(targetElement);
  } else {
    // Use existing element
    targetElement = typeof config.target === 'string' 
      ? document.querySelector(config.target)
      : config.target;
  }
  
  // Step 4: Define animations based on type
  let animation;
  
  const createAnimation = () => {
    switch (config.type) {
      case 'dots':
        return animate(`${config.target} .loading-dot`, {
          scale: [0, 1, 0],
          opacity: [0, 1, 0],
          delay: stagger(config.duration / 5),
          duration: config.duration,
          easing: 'easeInOutSine',
          loop: config.loop,
          direction: 'normal'
        });
        
      case 'spinner':
        return animate(`${config.target} .spinner-circle`, {
          rotate: 360,
          duration: config.duration,
          easing: 'linear',
          loop: config.loop
        });
        
      case 'progress':
        return animate(`${config.target} .progress-fill`, {
          width: ['0%', '100%'],
          duration: config.duration,
          easing: 'easeInOutQuad',
          loop: config.loop,
          direction: 'alternate'
        });
        
      case 'pulse':
        return animate(`${config.target} .pulse-circle`, {
          scale: [1, 1.2],
          opacity: [0.7, 0],
          duration: config.duration,
          easing: 'easeOutSine',
          loop: config.loop
        });
        
      default:
        return null;
    }
  };
  
  // Step 5: Initialize and return controller
  const controller = {
    /**
     * Start the loading animation
     * @returns {Object} Animation object
     */
    start: () => {
      // Create animation if it doesn't exist
      if (!animation) {
        animation = createAnimation();
      } else {
        animation.play();
      }
      
      // Show the element
      if (targetElement) {
        targetElement.style.display = '';
      }
      
      return animation;
    },
    
    /**
     * Pause the loading animation
     * @returns {Object} Animation object
     */
    pause: () => {
      if (animation) {
        animation.pause();
      }
      
      return animation;
    },
    
    /**
     * Stop and hide the loading animation
     * @param {Function} onComplete - Callback after hiding
     * @returns {Promise} Promise that resolves when complete
     */
    stop: async (onComplete) => {
      if (!targetElement) return Promise.resolve();
      
      // First pause the looping animation
      if (animation) {
        animation.pause();
      }
      
      // Create exit animation
      const exitAnimation = animate(targetElement, {
        opacity: 0,
        duration: 300,
        easing: 'easeOutQuad'
      });
      
      // Wait for exit to finish
      await exitAnimation.finished;
      
      // Hide element
      targetElement.style.display = 'none';
      
      // Reset opacity for future use
      targetElement.style.opacity = '1';
      
      // Call complete callback
      if (typeof onComplete === 'function') {
        onComplete();
      }
      
      if (typeof config.onComplete === 'function') {
        config.onComplete();
      }
      
      return exitAnimation;
    },
    
    /**
     * Update progress for progress-type loaders
     * @param {number} progress - Progress value from 0-1
     * @returns {Object} Controller object
     */
    updateProgress: (progress) => {
      if (config.type === 'progress' && targetElement) {
        const fill = targetElement.querySelector('.progress-fill');
        if (fill) {
          // Override animation with direct value
          animation.pause();
          fill.style.width = `${progress * 100}%`;
        }
      }
      
      return controller;
    },
    
    /**
     * Remove loading element and clean up
     * @returns {Object} Controller object
     */
    destroy: () => {
      if (animation) {
        animation.pause();
      }
      
      if (targetElement && config.generateHTML) {
        targetElement.parentNode.removeChild(targetElement);
      }
      
      animation = null;
      targetElement = null;
      
      return controller;
    }
  };
  
  // Start animation immediately if autoplay is true
  if (config.autoplay) {
    controller.start();
  }
  
  return controller;
}

// Example usage:
// Simple dots loading animation
const loadingIndicator = createLoadingAnimation({
  type: 'dots',
  generateHTML: true,
  container: document.querySelector('#loading-container'),
  color: '#5c6bc0',
  size: 'medium'
});

// Usage with async operation
async function fetchData() {
  loadingIndicator.start();
  
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    
    // Transition from loading to content
    await loadingIndicator.stop();
    
    // Display content with animation
    animate('#content', {
      opacity: [0, 1],
      translateY: [20, 0],
      duration: 500
    });
    
    return data;
  } catch (error) {
    console.error('Error:', error);
    loadingIndicator.stop();
    
    // Show error state
    animate('#error-message', {
      opacity: [0, 1],
      duration: 300
    });
  }
}
```

## 8. Implementation Planning Algorithm

### 8.1 Implementation Phase Planning Matrix

LLMs should use this structured implementation approach when planning anime.js integration:

```json
{
  "implementation_phases": [
    {
      "phase": "FOUNDATION",
      "timeline": "PHASE_1",
      "duration": "10_DAYS",
      "dependencies": [],
      "tasks": [
        {
          "id": "FOUNDATION_01",
          "name": "Library Integration",
          "description": "Implement anime.js into the build system and create import abstractions",
          "complexity": "LOW",
          "priority": "HIGH",
          "steps": [
            "Install anime.js via package manager",
            "Configure module bundler for optimal tree-shaking",
            "Create versioned CDN fallback for legacy environments",
            "Implement import abstractions for each framework context"
          ]
        },
        {
          "id": "FOUNDATION_02",
          "name": "Animation Utility Layer",
          "description": "Develop reusable animation utility functions",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Create framework-specific animation hooks/utilities",
            "Implement device capability detection module",
            "Develop animation registration system for management",
            "Build performance monitoring utilities"
          ]
        },
        {
          "id": "FOUNDATION_03", 
          "name": "Animation Style System",
          "description": "Define consistent animation parameters and styles",
          "complexity": "MEDIUM",
          "priority": "MEDIUM",
          "steps": [
            "Define standard easing presets",
            "Establish duration constants for consistency",
            "Create animation style token system",
            "Document animation style guidelines"
          ]
        }
      ]
    },
    {
      "phase": "CORE_IMPLEMENTATION",
      "timeline": "PHASE_2",
      "duration": "14_DAYS",
      "dependencies": ["FOUNDATION"],
      "tasks": [
        {
          "id": "CORE_01",
          "name": "Standard Animation Patterns",
          "description": "Implement common animation patterns",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Develop page transition system",
            "Create micro-interaction framework",
            "Build loading state animations",
            "Implement notification and toast animations"
          ]
        },
        {
          "id": "CORE_02",
          "name": "Responsive Animation System",
          "description": "Create device-adaptive animation framework",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Implement media query based animation adaptation",
            "Create reduced motion handling system",
            "Develop device capability-based adjustments",
            "Build viewport size adaptation utilities"
          ]
        }
      ]
    },
    {
      "phase": "ADVANCED_FEATURES",
      "timeline": "PHASE_3",
      "duration": "14_DAYS", 
      "dependencies": ["FOUNDATION", "CORE_IMPLEMENTATION"],
      "tasks": [
        {
          "id": "ADVANCED_01",
          "name": "Scroll-Based Animations",
          "description": "Implement scroll-triggered animation system",
          "complexity": "HIGH",
          "priority": "MEDIUM",
          "steps": [
            "Create intersection observer wrapper utility",
            "Build scroll position tracking system",
            "Develop scroll timeline synchronization",
            "Implement scroll-based animation triggers"
          ]
        },
        {
          "id": "ADVANCED_02",
          "name": "SVG Animations",
          "description": "Implement SVG-specific animation utilities",
          "complexity": "HIGH",
          "priority": "MEDIUM",
          "steps": [
            "Create SVG path drawing animations",
            "Develop SVG morphing utilities",
            "Build SVG filter animation system",
            "Implement SVG coordination with DOM animations"
          ]
        },
        {
          "id": "ADVANCED_03",
          "name": "Timeline Orchestration",
          "description": "Develop complex animation sequencing system",
          "complexity": "HIGH",
          "priority": "MEDIUM",
          "steps": [
            "Create nested timeline utilities",
            "Build animation sequence manager",
            "Develop animation state persistence",
            "Implement timeline debugging tools"
          ]
        }
      ]
    },
    {
      "phase": "OPTIMIZATION_TESTING",
      "timeline": "PHASE_4",
      "duration": "10_DAYS",
      "dependencies": ["FOUNDATION", "CORE_IMPLEMENTATION", "ADVANCED_FEATURES"],
      "tasks": [
        {
          "id": "OPTIMIZATION_01",
          "name": "Performance Testing",
          "description": "Implement comprehensive performance testing",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Develop automated performance benchmarking",
            "Create FPS monitoring system",
            "Implement memory usage tracking",
            "Build performance reporting dashboard"
          ]
        },
        {
          "id": "OPTIMIZATION_02",
          "name": "Cross-Browser Testing",
          "description": "Test and optimize for browser compatibility",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Create browser-specific test scenarios",
            "Implement visual regression testing",
            "Develop browser capability detection",
            "Build browser-specific fallbacks"
          ]
        },
        {
          "id": "OPTIMIZATION_03",
          "name": "Documentation",
          "description": "Create comprehensive animation system documentation",
          "complexity": "MEDIUM",
          "priority": "MEDIUM",
          "steps": [
            "Document animation utility API",
            "Create animation pattern cookbook",
            "Develop performance optimization guide",
            "Build animation style guide"
          ]
        }
      ]
    }
  ]
}
```

### 8.2 Implementation Decision Making Algorithm

LLMs should use this algorithm when making implementation decisions:

```javascript
/**
 * Algorithm for determining optimal anime.js implementation approach
 * 
 * @param {Object} context - Project context parameters
 * @returns {Object} Implementation recommendation
 */
function determineImplementationApproach(context) {
  // Step 1: Analyze project constraints
  const constraints = analyzeConstraints(context);
  
  // Step 2: Determine implementation priority
  const priorities = determinePriorities(constraints);
  
  // Step 3: Generate implementation plan
  const plan = generateImplementationPlan(priorities, context);
  
  return {
    constraints,
    priorities,
    plan
  };
  
  // Inner functions
  function analyzeConstraints(context) {
    return {
      // Performance constraints
      performance: {
        critical: context.performanceTarget === "LOW",
        important: context.performanceTarget === "MEDIUM",
        flexible: context.performanceTarget === "HIGH"
      },
      
      // Technical constraints
      technical: {
        // Framework limitations
        frameworkLimitations: {
          needsPolyfills: context.platformContext === "SERVER-RENDERED",
          usesVirtualDOM: context.platformContext.includes("React") || context.platformContext.includes("Vue"),
          requiresCustomIntegration: context.platformContext === "STATIC"
        },
        
        // Hosting limitations
        hostingLimitations: {
          limitedResources: context.hostingEnvironment === "SHARED_HOSTING",
          bundleSizeCritical: context.hostingEnvironment === "SHARED_HOSTING"
        }
      },
      
      // Animation complexity
      complexity: {
        simple: context.animationComplexity === "SIMPLE",
        moderate: context.animationComplexity === "INTERMEDIATE",
        complex: context.animationComplexity === "ADVANCED"
      }
    };
  }
  
  function determinePriorities(constraints) {
    // Calculate priority scores for different implementation aspects
    const priorityScores = {
      bundleSize: constraints.technical.hostingLimitations.bundleSizeCritical ? 10 : 
                  constraints.technical.hostingLimitations.limitedResources ? 8 : 5,
                  
      performance: constraints.performance.critical ? 10 :
                   constraints.performance.important ? 8 : 5,
                   
      featureRichness: constraints.complexity.complex ? 10 :
                        constraints.complexity.moderate ? 7 : 4,
                        
      compatibility: constraints.technical.frameworkLimitations.needsPolyfills ? 9 :
                     constraints.technical.frameworkLimitations.usesVirtualDOM ? 7 : 5,
                     
      developmentSpeed: 7 // Default value, could be adjusted based on other factors
    };
    
    // Sort priorities by score
    const sortedPriorities = Object.entries(priorityScores)
      .sort((a, b) => b[1] - a[1])
      .map(entry => entry[0]);
    
    return {
      primaryFocus: sortedPriorities[0],
      secondaryFocus: sortedPriorities[1],
      priorities: sortedPriorities,
      scores: priorityScores
    };
  }
  
  function generateImplementationPlan(priorities, context) {
    // Generate phase-based plan based on priorities
    const implementationPhases = [];
    
    // Determine integration approach
    const integrationApproach = determineIntegrationApproach(priorities, context);
    
    // Determine optimization strategy
    const optimizationStrategy = determineOptimizationStrategy(priorities, context);
    
    // Determine feature implementation order
    const featureImplementationOrder = determineFeatureOrder(priorities, context);
    
    // Create phase structure
    implementationPhases.push({
      phase: "Foundation",
      focus: "Setup core architecture",
      approach: integrationApproach,
      duration: "10-14 days"
    });
    
    implementationPhases.push({
      phase: "Core Features",
      focus: "Implement essential animations",
      features: featureImplementationOrder.slice(0, 3),
      duration: "14-21 days"
    });
    
    implementationPhases.push({
      phase: "Advanced Features",
      focus: "Implement complex animations",
      features: featureImplementationOrder.slice(3),
      duration: "14-21 days"
    });
    
    implementationPhases.push({
      phase: "Optimization",
      focus: "Performance tuning and testing",
      strategy: optimizationStrategy,
      duration: "10-14 days"
    });
    
    return {
      phases: implementationPhases,
      totalEstimatedDuration: "48-70 days",
      approachSummary: `Focus on ${priorities.primaryFocus} with ${integrationApproach.name} integration approach`
    };
  }
  
  function determineIntegrationApproach(priorities, context) {
    // Determine the best integration approach based on priorities and context
    if (priorities.primaryFocus === "bundleSize" || priorities.scores.bundleSize >= 8) {
      return {
        name: "Minimal Import",
        description: "Import only essential anime.js functions",
        implementation: "import { animate } from 'animejs'",
        benefits: "Smallest possible bundle size through tree-shaking"
      };
    } else if (priorities.primaryFocus === "developmentSpeed" || context.platformContext === "STATIC") {
      return {
        name: "Full Import",
        description: "Import complete anime.js library for simplicity",
        implementation: "import anime from 'animejs'",
        benefits: "Simplest implementation, all features available"
      };
    } else {
      return {
        name: "Balanced Import",
        description: "Import commonly used functions selectively",
        implementation: "import { animate, stagger, createTimeline } from 'animejs'",
        benefits: "Good balance between bundle size and development convenience"
      };
    }
  }
  
  function determineOptimizationStrategy(priorities, context) {
    // Determine optimization strategy based on priorities
    if (priorities.primaryFocus === "performance" || priorities.scores.performance >= 8) {
      return {
        name: "Performance-First",
        description: "Optimize heavily for performance, even at cost of features",
        techniques: [
          "Use WAAPI version where possible",
          "Limit concurrent animations",
          "Apply aggressive throttling on low-end devices",
          "Implement advanced performance monitoring"
        ]
      };
    } else if (priorities.primaryFocus === "featureRichness") {
      return {
        name: "Feature-Rich",
        description: "Implement full feature set with reasonable optimizations",
        techniques: [
          "Use standard optimization techniques",
          "Implement progressive enhancement",
          "Apply device capability detection",
          "Optimize critical animations first"
        ]
      };
    } else {
      return {
        name: "Balanced Optimization",
        description: "Balance performance and features",
        techniques: [
          "Apply standard performance best practices",
          "Use hardware acceleration where possible",
          "Implement basic device detection",
          "Optimize high-visibility animations"
        ]
      };
    }
  }
  
  function determineFeatureOrder(priorities, context) {
    // Base feature set
    const allFeatures = [
      "Basic Transitions",
      "Hover Effects",
      "Click Animations",
      "Page Transitions",
      "Scroll-Based Animations",
      "SVG Animations",
      "Complex Sequences",
      "3D Effects"
    ];
    
    // Determine order based on priorities
    if (priorities.primaryFocus === "performance" || context.performanceTarget === "LOW") {
      // Performance-critical: start with simplest animations
      return [
        "Basic Transitions",
        "Hover Effects",
        "Click Animations",
        "Page Transitions",
        "Scroll-Based Animations",
        "SVG Animations",
        "Complex Sequences",
        "3D Effects"
      ];
    } else if (priorities.primaryFocus === "featureRichness" || context.animationComplexity === "ADVANCED") {
      // Feature-focused: prioritize impressive animations
      return [
        "Page Transitions",
        "SVG Animations",
        "Complex Sequences",
        "3D Effects",
        "Scroll-Based Animations",
        "Hover Effects",
        "Click Animations",
        "Basic Transitions"
      ];
    } else {
      // Balanced approach: mix of important and impressive
      return [
        "Page Transitions",
        "Basic Transitions",
        "Hover Effects",
        "Scroll-Based Animations",
        "Click Animations",
        "SVG Animations",
        "Complex Sequences",
        "3D Effects"
      ];
    }
  }
}

// Example usage:
const projectContext = {
  performanceTarget: "MEDIUM",
  platformContext: "MODERN-FRAMEWORK",
  animationComplexity: "INTERMEDIATE",
  hostingEnvironment: "SHARED_HOSTING"
};

const implementationPlan = determineImplementationApproach(projectContext);
console.log('Implementation Plan:', implementationPlan);
```

## 9. Information Processing and Decision-Making Framework

### 9.1 Implementation Decision Tree For LLMs

```
START ANIME.JS_IMPLEMENTATION
├── Analyze Project Requirements
│   ├── Determine Performance Target (LOW/MEDIUM/HIGH)
│   ├── Identify Platform Context (SERVER-RENDERED/MODERN-FRAMEWORK/STATIC)
│   ├── Assess Animation Complexity (SIMPLE/INTERMEDIATE/ADVANCED)
│   └── Evaluate Hosting Environment (SHARED_HOSTING/DEDICATED/CLOUD)
│
├── Select Integration Approach
│   ├── Performance Critical? (LOW or SHARED_HOSTING)
│   │   ├── YES
│   │   │   └── USE: Minimal Import Strategy
│   │   │       └── import { animate, ... } from 'animejs'
│   │   └── NO
│   │       ├── Development Speed Priority? (STATIC)
│   │       │   ├── YES
│   │       │   │   └── USE: Full Import Strategy
│   │       │   │       └── import anime from 'animejs'
│   │       │   └── NO
│   │       │       └── USE: Balanced Import Strategy
│   │       │           └── import { animate, stagger, createTimeline } from 'animejs'
│
├── Apply Framework-Specific Integration
│   ├── Platform is SERVER-RENDERED?
│   │   ├── YES
│   │   │   └── IMPLEMENT: DOMContentLoaded Event Pattern
│   │   └── NO
│   │       ├── Platform is React?
│   │       │   ├── YES
│   │       │   │   └── IMPLEMENT: useEffect Hook Pattern
│   │       │   └── NO
│   │       │       ├── Platform is Vue?
│   │       │       │   ├── YES
│   │       │       │   │   └── IMPLEMENT: mounted() Lifecycle Pattern
│   │       │       │   └── NO
│   │       │       │       └── IMPLEMENT: Standard Module Pattern
│
├── Implement Device-Adaptive Strategy
│   ├── Performance Target is LOW?
│   │   ├── YES
│   │   │   └── IMPLEMENT: Minimal Animation Strategy
│   │   │       └── USE: WAAPI version where possible
│   │   └── NO
│   │       ├── Animation Complexity is ADVANCED?
│   │       │   ├── YES
│   │       │   │   └── IMPLEMENT: Progressive Enhancement
│   │       │   └── NO
│   │       │       └── IMPLEMENT: Standard Adaptive Strategy
│
├── Select Animation Patterns
│   ├── Implement Core Patterns First
│   │   └── Basic Transitions → Page Transitions → Hover Effects
│   ├── Then Add Intermediate Patterns
│   │   └── Scroll Animations → Click Effects → Loading Indicators
│   └── Finally Add Advanced Patterns (if needed)
│       └── SVG Animations → Timeline Sequences → 3D Effects
│
└── END ANIME.JS_IMPLEMENTATION
```

### 9.2 Final Recommendation Matrix

| Context Parameter | Primary Recommendation | Alternative Recommendation |
|-------------------|------------------------|---------------------------|
| LOW + SHARED_HOSTING | Use minimal anime.js import with WAAPI version for critical animations | Consider using pure CSS animations for simplest cases |
| MEDIUM + ANY_HOSTING | Use balanced anime.js implementation with device detection | GSAP if timeline sequences are extremely complex |
| HIGH + ANY_HOSTING | Use full anime.js feature set with progressive enhancement | Consider specialized libraries for specific effects (Three.js for 3D) |
| SERVER-RENDERED + ANY | Use anime.js with careful DOM-ready handling and minimal server impact | Pre-render animation states where possible |
| MODERN-FRAMEWORK + ANY | Integrate anime.js with framework lifecycle for optimal performance | Consider framework-specific animation libraries as alternatives |

By systematically analyzing the context parameters and following the implementation decision tree, LLMs can provide optimal anime.js implementation guidance for any web development scenario while ensuring high performance across all target devices.
# GSAP Implementation Guide For LLMs

## 1. Core Understanding Framework

This document is structured for Large Language Models (LLMs) to understand, reason about, and implement GSAP (GreenSock Animation Platform) animations in web development projects. It provides a systematic approach to decision-making and implementation across various contexts.

### 1.1 Library Definition

GSAP is a robust JavaScript animation library that provides high-performance tools for animating DOM elements, SVG, CSS properties, JavaScript objects, and canvas through a unified API. Version 3.12.2 is the current stable release as of May 2025.

### 1.2 Implementation Context Parameters

When implementing GSAP animations, reason through these parameters sequentially:

1. **Performance Target:**
   - LOW = Low-end devices, optimize for minimal resource usage
   - MEDIUM = Average devices, balanced animations, some visual enhancements
   - HIGH = High-end devices, complex animations, rich visual effects

2. **Platform Context:**
   - SERVER-RENDERED = PHP, Laravel, traditional frameworks
   - MODERN-FRAMEWORK = React, Vue, Svelte, Angular
   - STATIC = Plain HTML/JS implementation

3. **Animation Complexity Matrix:**
   - SIMPLE = Basic transitions, opacity, translation (Level 1)
   - INTERMEDIATE = Timeline sequences, staggered animations (Level 2)
   - ADVANCED = SVG manipulation, 3D effects, scroll-driven animations (Level 3)

4. **Hosting Environment Constraints:**
   - SHARED_HOSTING = Limited resources, optimization critical
   - DEDICATED = More resources available, optimization recommended
   - CLOUD = Scalable resources, standard optimization

## 2. Library Technical Framework

### 2.1 Architectural Model

GSAP operates through a modular object-based architecture with a core engine that powers specialized animation components. The architecture allows for plugin extension while maintaining optimal performance.

#### 2.1.1 Core Components:

```
gsap → Main namespace and core animation engine
gsap.to() → Primary animation function for animating to values
gsap.from() → Animates from specified values to current state
gsap.fromTo() → Precise control with explicit start and end values
gsap.timeline() → Creates animation sequences
gsap.set() → Instantly sets properties without animation
```

#### 2.1.2 Primary Plugins:

```
ScrollTrigger → Ties animations to scroll position
ScrollToPlugin → Smooth scrolling animation
Draggable → Creates draggable elements
MorphSVGPlugin → Morphs SVG paths (premium)
MotionPathPlugin → Animates along paths
SplitText → Text manipulation for character/word/line animations (premium)
```

### 2.2 Feature-Decision Matrix

When implementing GSAP, apply this decision matrix to determine which features to utilize:

| Context Parameter | Feature Selection | Implementation Approach |
|-------------------|------------------|-------------------------|
| LOW + SHARED_HOSTING | Core GSAP only | Use only transform/opacity, avoid unnecessary plugins |
| MEDIUM + SHARED_HOSTING | GSAP + essential plugins | Load ScrollTrigger only if needed, use lite version of plugins |
| HIGH + ANY_HOSTING | Full GSAP ecosystem | Leverage all capabilities and plugins as needed |
| ANY + SERVER-RENDERED | DOM-ready events | Ensure animations execute after DOM loads |
| ANY + MODERN-FRAMEWORK | Component lifecycle hooks | Initialize animations in mount/effect hooks |

### 2.3 Module Selection Tree

Follow this decision tree to select appropriate modules:

1. **Basic Animation Required Only?**
   - YES → Import core GSAP only
   - NO → Continue to step 2

2. **Scroll-Based Animations Needed?**
   - YES → Import GSAP + ScrollTrigger
   - NO → Continue to step 3

3. **Complex Timeline Sequences?**
   - YES → Import GSAP (timeline included in core)
   - NO → Continue to step 4

4. **SVG Animations Required?**
   - YES → Import GSAP + MorphSVGPlugin (if morphing needed)
   - NO → Continue to step 5

5. **Motion Path Animation Needed?**
   - YES → Import GSAP + MotionPathPlugin
   - NO → Continue to step 6

6. **Interactive Draggable Elements?**
   - YES → Import GSAP + Draggable
   - NO → Use only previously selected modules

### 2.4 Performance Analysis Dataset

GSAP performance characteristics can be modeled through these quantifiable metrics:

```json
{
  "performance_metrics": {
    "fps_capability": {
      "low_complexity": "60fps on low-end devices",
      "medium_complexity": "60fps on medium devices, 30-60fps on low-end",
      "high_complexity": "60fps on high-end, 30-40fps on medium, <30fps on low-end"
    },
    "element_thresholds": {
      "safe_threshold": "1000 elements (2000 tweens)",
      "medium_threshold": "2500 elements (5000 tweens)",
      "upper_threshold": "5000 elements (10000 tweens)"
    },
    "bundle_size": {
      "core_only": "23KB (minified+gzipped)",
      "common_setup": "40-50KB (core + ScrollTrigger)",
      "full_featured": "70-100KB (core + all common plugins)"
    }
  }
}
```

### 2.5 Library Selection Algorithm

When determining whether to use GSAP versus alternatives, apply this algorithm:

1. **Input:** Project requirements, target devices, animation complexity
2. **Process:**
   ```
   IF animation_complexity == ADVANCED OR elements_count > 1000 THEN
     recommended_library = "GSAP"
   ELSE IF bundle_size_critical == TRUE AND animation_complexity == SIMPLE THEN
     recommended_library = "anime.js OR motion-one"
   ELSE IF modern_framework_integration == PRIMARY_CONCERN THEN
     IF framework == "React" THEN
       recommended_library = "framer-motion OR GSAP" 
     ELSE IF framework == "Vue" THEN
       recommended_library = "vue-transition OR GSAP"
     ELSE
       recommended_library = "GSAP"
   ELSE
     recommended_library = "GSAP"
   END IF
   ```
3. **Output:** Recommended animation library

Rationale: GSAP provides superior performance for complex animations and large-scale projects, while smaller libraries may be more appropriate for simpler use cases where bundle size is critical.

## 3. Implementation Process Model

### 3.1 Installation Decision Tree

```
START
├── Is the project using a build system with NPM/Yarn?
│   ├── YES
│   │   ├── Is a complete solution with all plugins needed?
│   │   │   ├── YES
│   │   │   │   └── INSTALL METHOD: npm i gsap
│   │   │   └── NO
│   │   │       └── INSTALL METHOD: npm i gsap
│   │   │           └── Import only what you need (core has most features)
│   └── NO
│       ├── Is the project on a shared hosting environment?
│       │   ├── YES
│       │   │   └── INSTALL METHOD: CDN with version lock
│       │   │       └── <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
│       │   └── NO
│       │       └── INSTALL METHOD: Download and self-host
│       │           └── Download from https://greensock.com/get-started/#download
└── END
```

### 3.2 Framework Integration Models

Each framework requires specific integration patterns. LLMs should follow these exact implementation templates:

#### 3.2.1 React Implementation Template

```javascript
// Step 1: Import GSAP and any required plugins
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { useEffect, useRef } from 'react';

// Step 2: Register plugins (must be done once)
gsap.registerPlugin(ScrollTrigger);

// Step 3: Create component with animation logic
function AnimatedComponent() {
  // Step 3.1: Create references to DOM elements
  const elementRef = useRef(null);
  const timelineRef = useRef(null);
  
  // Step 3.2: Initialize animation in useEffect hook
  useEffect(() => {
    // Step 3.3: Create context for clean up with React 18+
    const ctx = gsap.context(() => {
      // Step 3.4: Create animation with configuration
      timelineRef.current = gsap.timeline({
        scrollTrigger: {
          trigger: elementRef.current,
          start: "top 80%",
          end: "bottom 20%",
          toggleActions: "play none none reverse"
        }
      })
      .to(elementRef.current, {
        x: 250,
        rotation: 360,
        duration: 1,
        ease: "power2.inOut"
      })
      .to(elementRef.current, {
        backgroundColor: "#8d44ad",
        duration: 0.5
      }, "-=0.25");
    }, elementRef); // Scope to our reference
    
    // Step 3.5: Return cleanup function
    return () => {
      ctx.revert(); // This handles all cleanup
    };
  }, []); // Empty dependency array means run once on mount
  
  // Step 4: Return component JSX with ref attached
  return <div ref={elementRef} className="element">Animated Box</div>;
}
```

#### 3.2.2 Vue Implementation Template

```javascript
<template>
  <!-- Step 1: Create element with ref attribute -->
  <div ref="elementRef" class="element">Animated Box</div>
</template>

<script>
// Step 2: Import GSAP and any plugins
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

// Step 3: Register plugins
gsap.registerPlugin(ScrollTrigger);

export default {
  data() {
    return {
      // Step 4: Store timeline if needed for controls
      timeline: null
    }
  },
  // Step 5: Initialize animation in mounted lifecycle hook
  mounted() {
    // Step 5.1: Create and store timeline
    this.timeline = gsap.timeline({
      scrollTrigger: {
        trigger: this.$refs.elementRef,
        start: "top 80%",
        end: "bottom 20%",
        toggleActions: "play none none reverse"
      }
    })
    .to(this.$refs.elementRef, {
      x: 250,
      rotation: 360,
      duration: 1,
      ease: "power2.inOut"
    })
    .to(this.$refs.elementRef, {
      backgroundColor: "#8d44ad",
      duration: 0.5
    }, "-=0.25");
  },
  // Step 6: Cleanup in beforeUnmount
  beforeUnmount() {
    // Step 6.1: Kill timeline to prevent memory leaks
    if (this.timeline) {
      this.timeline.kill();
    }
    
    // Step 6.2: Kill any ScrollTriggers associated with this component
    ScrollTrigger.getAll().forEach(trigger => {
      if (trigger.vars.trigger === this.$refs.elementRef) {
        trigger.kill();
      }
    });

#### 7.2.3 Loading Animation Implementation Template

```javascript
/**
 * Factory function to create loading state animations with GSAP
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Loading animation controller
 */
function createGSAPLoadingAnimation(options = {}) {
  // Step 1: Configure with defaults
  const config = {
    // Animation target(s)
    target: options.target || '.loading-indicator',
    
    // Animation type
    type: options.type || 'spinner', // spinner, dots, progress, pulse, custom
    
    // Animation properties
    duration: options.duration || 0.8,
    repeat: options.repeat !== undefined ? options.repeat : -1, // -1 = infinite
    repeatDelay: options.repeatDelay || 0,
    yoyo: options.yoyo !== undefined ? options.yoyo : true,
    
    // Element generation
    generateHTML: options.generateHTML !== undefined ? options.generateHTML : false,
    container: options.container || document.body,
    
    // Style configurations
    dotCount: options.dotCount || 3,
    size: options.size || 'medium', // small, medium, large
    color: options.color || '#3498db',
    
    // Callbacks
    onStart: options.onStart || null,
    onComplete: options.onComplete || null,
    
    ...options
  };
  
  // Step 2: Generate HTML if requested
  let targetElement;
  let generatedElements = [];
  
  if (config.generateHTML) {
    targetElement = document.createElement('div');
    targetElement.classList.add('gsap-loading-indicator', `loading-${config.type}`);
    targetElement.classList.add(`size-${config.size}`);
    
    // Apply type-specific HTML structure
    switch (config.type) {
      case 'spinner':
        targetElement.innerHTML = `<div class="spinner-circle"></div>`;
        const spinnerCircle = targetElement.querySelector('.spinner-circle');
        spinnerCircle.style.borderColor = `rgba(0, 0, 0, 0.1)`;
        spinnerCircle.style.borderTopColor = config.color;
        break;
        
      case 'dots':
        for (let i = 0; i < config.dotCount; i++) {
          const dot = document.createElement('div');
          dot.classList.add('loading-dot');
          dot.style.backgroundColor = config.color;
          targetElement.appendChild(dot);
          generatedElements.push(dot);
        }
        break;
        
      case 'progress':
        targetElement.innerHTML = `
          <div class="progress-track">
            <div class="progress-bar"></div>
          </div>
        `;
        targetElement.querySelector('.progress-track').style.backgroundColor = `rgba(0, 0, 0, 0.1)`;
        targetElement.querySelector('.progress-bar').style.backgroundColor = config.color;
        break;
        
      case 'pulse':
        targetElement.innerHTML = `<div class="pulse-circle"></div>`;
        targetElement.querySelector('.pulse-circle').style.backgroundColor = config.color;
        break;
    }
    
    // Add to container
    config.container.appendChild(targetElement);
  } else {
    // Use existing element
    targetElement = typeof config.target === 'string' 
      ? document.querySelector(config.target)
      : config.target;
  }
  
  // Step 3: Create GSAP animations based on type
  let animation;
  let timeline;
  
  const createAnimation = () => {
    switch (config.type) {
      case 'spinner':
        return gsap.to(targetElement.querySelector('.spinner-circle') || targetElement, {
          rotation: 360,
          duration: config.duration,
          ease: "none",
          repeat: config.repeat,
          paused: true
        });
        
      case 'dots':
        const dots = generatedElements.length > 0 
          ? generatedElements 
          : targetElement.querySelectorAll('.loading-dot');
        
        // Create timeline for dots
        const dotTimeline = gsap.timeline({
          repeat: config.repeat,
          repeatDelay: config.repeatDelay,
          paused: true
        });
        
        // Add animation for each dot with stagger
        dotTimeline.to(dots, {
          y: -10,
          opacity: 1,
          duration: config.duration / 2,
          stagger: config.duration / (dots.length * 2),
          yoyo: true,
          repeat: 1
        });
        
        return dotTimeline;
        
      case 'progress':
        const progressBar = targetElement.querySelector('.progress-bar') || targetElement;
        return gsap.fromTo(progressBar, 
          { width: "0%" },
          {
            width: "100%",
            duration: config.duration,
            ease: "power1.inOut",
            repeat: config.repeat,
            yoyo: config.yoyo,
            paused: true
          }
        );
        
      case 'pulse':
        const pulseElement = targetElement.querySelector('.pulse-circle') || targetElement;
        return gsap.timeline({
          repeat: config.repeat,
          repeatDelay: config.repeatDelay,
          paused: true
        })
        .fromTo(pulseElement, 
          { scale: 0.5, opacity: 1 },
          {
            scale: 1.5,
            opacity: 0,
            duration: config.duration,
            ease: "sine.out"
          }
        );
        
      case 'custom':
        // For custom type, we expect the user to provide the animation function
        if (typeof config.createCustomAnimation === 'function') {
          return config.createCustomAnimation(targetElement, config);
        } else {
          console.error("Custom loading animation requires createCustomAnimation function");
          return gsap.to(targetElement, { paused: true });
        }
    }
  };
  
  // Step 4: Initialize and return controller
  const controller = {
    /**
     * Start the loading animation
     * @returns {Object} Animation object
     */
    start: () => {
      // Create animation if it doesn't exist
      if (!animation) {
        animation = createAnimation();
      }
      
      // Show the element
      if (targetElement) {
        gsap.set(targetElement, { display: 'flex' });
        gsap.to(targetElement, { opacity: 1, duration: 0.3 });
      }
      
      // Play animation
      animation.play(0);
      
      // Call onStart callback
      if (typeof config.onStart === 'function') {
        config.onStart(controller);
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
     * @param {Function} callback - Callback after hiding
     * @returns {Promise} Promise that resolves when complete
     */
    stop: async (callback) => {
      if (!targetElement) return Promise.resolve();
      
      // First pause the looping animation
      if (animation) {
        animation.pause();
      }
      
      // Create exit animation
      const exitAnimation = gsap.to(targetElement, {
        opacity: 0,
        duration: 0.3,
        onComplete: () => {
          // Hide element
          gsap.set(targetElement, { display: 'none' });
          
          // Call complete callback
          if (typeof callback === 'function') {
            callback();
          }
          
          if (typeof config.onComplete === 'function') {
            config.onComplete(controller);
          }
        }
      });
      
      // Return promise for the exit animation
      return new Promise(resolve => {
        exitAnimation.eventCallback("onComplete", resolve);
      });
    },
    
    /**
     * Update progress for progress-type loaders
     * @param {number} progress - Progress value from 0-1
     * @returns {Object} Controller object
     */
    updateProgress: (progress) => {
      if (config.type === 'progress' && targetElement) {
        const progressBar = targetElement.querySelector('.progress-bar') || targetElement;
        
        // Update width directly
        gsap.to(progressBar, {
          width: `${progress * 100}%`,
          duration: 0.3,
          ease: "power1.out"
        });
        
        // Stop animation if it was running
        if (animation && animation.isActive()) {
          animation.pause();
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
        animation.kill();
      }
      
      if (targetElement && config.generateHTML) {
        gsap.to(targetElement, {
          opacity: 0,
          duration: 0.2,
          onComplete: () => {
            targetElement.parentNode.removeChild(targetElement);
          }
        });
      }
      
      animation = null;
      
      return controller;
    },
    
    /**
     * Get the target element
     * @returns {HTMLElement} The loading indicator element
     */
    getElement: () => targetElement
  };
  
  // Start animation immediately if autoplay is true
  if (config.autoplay) {
    controller.start();
  }
  
  return controller;
}

// Example usage:
// Simple spinner loading animation
const loadingSpinner = createGSAPLoadingAnimation({
  type: 'spinner',
  generateHTML: true,
  container: document.querySelector('#app-container'),
  color: '#5c6bc0',
  size: 'medium',
  autoplay: true
});

// Progress bar with updating
const progressLoader = createGSAPLoadingAnimation({
  type: 'progress',
  generateHTML: true,
  container: document.querySelector('#upload-container'),
  color: '#e74c3c',
  autoplay: false
});

// Usage with async operation
async function fetchData() {
  loadingSpinner.start();
  
  try {
    // Fake progress updates
    const progressIntervals = [0.1, 0.3, 0.5, 0.7, 0.9, 1.0];
    for (const progress of progressIntervals) {
      await new Promise(r => setTimeout(r, 500));
      progressLoader.updateProgress(progress);
    }
    
    const response = await fetch('/api/data');
    const data = await response.json();
    
    // Transition from loading to content
    await loadingSpinner.stop();
    
    // Display content with animation
    gsap.fromTo('#content', 
      { opacity: 0, y: 20 },
      { opacity: 1, y: 0, duration: 0.5 }
    );
    
    return data;
  } catch (error) {
    console.error('Error:', error);
    loadingSpinner.stop();
    
    // Show error state
    gsap.to('#error-message', {
      opacity: 1,
      duration: 0.3
    });
  }
}
```

#### 7.2.4 Scroll-Based Animation Implementation Template

```javascript
/**
 * Factory function to create scroll-based animations with GSAP ScrollTrigger
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Scroll animation controller
 */
function createGSAPScrollAnimations(options = {}) {
  // Step 1: Ensure ScrollTrigger is registered
  if (typeof ScrollTrigger === 'undefined') {
    console.error('ScrollTrigger plugin is required but not loaded.');
    return null;
  }
  
  // Step 2: Configure with defaults
  const config = {
    // Defaults for all animations
    defaults: {
      start: options.defaults?.start || "top 80%",
      end: options.defaults?.end || "bottom 20%",
      toggleActions: options.defaults?.toggleActions || "play none none reverse",
      markers: options.defaults?.markers || false,
      scrub: options.defaults?.scrub || false,
      once: options.defaults?.once || false
    },
    
    // Default animation settings
    animation: {
      duration: options.animation?.duration || 1,
      stagger: options.animation?.stagger || 0.1,
      ease: options.animation?.ease || "power2.out"
    },
    
    // Animation types
    types: {
      fadeIn: {
        fromVars: { opacity: 0, y: 30 },
        toVars: { opacity: 1, y: 0 }
      },
      slideInLeft: {
        fromVars: { opacity: 0, x: -50 },
        toVars: { opacity: 1, x: 0 }
      },
      slideInRight: {
        fromVars: { opacity: 0, x: 50 },
        toVars: { opacity: 1, x: 0 }
      },
      zoomIn: {
        fromVars: { opacity: 0, scale: 0.8 },
        toVars: { opacity: 1, scale: 1 }
      },
      ...options.types
    },
    
    // Enable/disable media query checking
    enableMediaQueries: options.enableMediaQueries !== undefined ? 
      options.enableMediaQueries : true,
    
    // Breakpoint definitions for responsive behavior
    breakpoints: {
      mobile: options.breakpoints?.mobile || "(max-width: 767px)",
      tablet: options.breakpoints?.tablet || "(min-width: 768px) and (max-width: 1023px)",
      desktop: options.breakpoints?.desktop || "(min-width: 1024px)"
    },
    
    // Disable on mobile option
    disableOnMobile: options.disableOnMobile !== undefined ? 
      options.disableOnMobile : false,
    
    // Respect reduced motion preference
    respectReducedMotion: options.respectReducedMotion !== undefined ? 
      options.respectReducedMotion : true,
      
    ...options
  };
  
  // Step 3: Store references to created animations
  const animations = [];
  
  // Step 4: Create utility functions
  
  /**
   * Check if reduced motion is preferred
   * @returns {boolean} True if reduced motion is preferred
   */
  const isReducedMotionPreferred = () => {
    return config.respectReducedMotion && 
      window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  };
  
  /**
   * Check if animation should be disabled based on current media query
   * @returns {boolean} True if animation should be disabled
   */
  const isDisabledByMediaQuery = () => {
    if (!config.enableMediaQueries) return false;
    
    if (config.disableOnMobile && 
        window.matchMedia(config.breakpoints.mobile).matches) {
      return true;
    }
    
    return false;
  };
  
  /**
   * Create a single scroll-triggered animation
   * @param {string|Element} element - Target element(s)
   * @param {string|Object} animationType - Animation type or custom config
   * @param {Object} options - Additional options
   * @returns {Object} ScrollTrigger instance
   */
  const createScrollAnimation = (element, animationType, options = {}) => {
    // Skip animation if reduced motion is preferred
    if (isReducedMotionPreferred()) {
      // Just show the element immediately
      if (typeof element === 'string') {
        gsap.set(document.querySelectorAll(element), { opacity: 1 });
      } else {
        gsap.set(element, { opacity: 1 });
      }
      return null;
    }
    
    // Skip animation if disabled by media query
    if (isDisabledByMediaQuery()) {
      // Just show the element immediately
      if (typeof element === 'string') {
        gsap.set(document.querySelectorAll(element), { opacity: 1 });
      } else {
        gsap.set(element, { opacity: 1 });
      }
      return null;
    }
    
    // Merge ScrollTrigger options
    const scrollTriggerOptions = {
      ...config.defaults,
      ...options.scrollTrigger
    };
    
    let timeline;
    
    // Handle different animation type formats
    if (typeof animationType === 'string' && config.types[animationType]) {
      // Predefined animation type
      const type = config.types[animationType];
      
      timeline = gsap.timeline({
        scrollTrigger: {
          trigger: element,
          ...scrollTriggerOptions
        }
      });
      
      // Create animation
      timeline.fromTo(element, 
        { ...type.fromVars },
        { 
          ...type.toVars,
          duration: options.duration || config.animation.duration,
          stagger: options.stagger || config.animation.stagger,
          ease: options.ease || config.animation.ease
        }
      );
    } else if (typeof animationType === 'object') {
      // Custom animation configuration
      timeline = gsap.timeline({
        scrollTrigger: {
          trigger: element,
          ...scrollTriggerOptions
        }
      });
      
      // Determine animation type (fromTo, from, to)
      if (animationType.fromVars && animationType.toVars) {
        // fromTo animation
        timeline.fromTo(element,
          animationType.fromVars,
          {
            ...animationType.toVars,
            duration: options.duration || config.animation.duration,
            stagger: options.stagger || config.animation.stagger,
            ease: options.ease || config.animation.ease
          }
        );
      } else if (animationType.fromVars) {
        // from animation
        timeline.from(element, {
          ...animationType.fromVars,
          duration: options.duration || config.animation.duration,
          stagger: options.stagger || config.animation.stagger,
          ease: options.ease || config.animation.ease
        });
      } else if (animationType.toVars) {
        // to animation
        timeline.to(element, {
          ...animationType.toVars,
          duration: options.duration || config.animation.duration,
          stagger: options.stagger || config.animation.stagger,
          ease: options.ease || config.animation.ease
        });
      }
    } else if (typeof animationType === 'function') {
      // Function that returns a timeline
      timeline = animationType(element, gsap, ScrollTrigger);
    }
    
    // Store animation reference
    if (timeline) {
      animations.push({
        element,
        timeline,
        scrollTrigger: timeline.scrollTrigger
      });
    }
    
    return timeline?.scrollTrigger;
  };
  
  /**
   * Create a batch of scroll animations with the same configuration
   * @param {string} selector - CSS selector for all targets
   * @param {string|Object} animationType - Animation type or custom config
   * @param {Object} options - Additional options
   * @returns {Array} Array of ScrollTrigger instances
   */
  const createBatchScrollAnimations = (selector, animationType, options = {}) => {
    const elements = document.querySelectorAll(selector);
    const triggers = [];
    
    elements.forEach((element, index) => {
      // Calculate staggered start position if needed
      let individualOptions = { ...options };
      
      if (options.staggerStart) {
        const startOffset = index * (options.staggerStart || 0.1);
        
        if (!individualOptions.scrollTrigger) {
          individualOptions.scrollTrigger = {};
        }
        
        // Adjust start position
        if (typeof individualOptions.scrollTrigger.start === 'string' && 
            individualOptions.scrollTrigger.start.includes('%')) {
          // Parse percentage value
          const startMatch = individualOptions.scrollTrigger.start.match(/(\d+)%/);
          if (startMatch && startMatch[1]) {
            const startPercent = parseInt(startMatch[1], 10);
            const newStart = Math.max(0, startPercent - (startOffset * 10)); // Arbitrary scaling
            individualOptions.scrollTrigger.start = 
              individualOptions.scrollTrigger.start.replace(/\d+%/, `${newStart}%`);
          }
        }
      }
      
      const trigger = createScrollAnimation(element, animationType, individualOptions);
      if (trigger) {
        triggers.push(trigger);
      }
    });
    
    return triggers;
  };
  
  /**
   * Create a parallax scroll effect
   * @param {string|Element} element - Target element
   * @param {Object} options - Parallax options
   * @returns {Object} ScrollTrigger instance
   */
  const createParallaxEffect = (element, options = {}) => {
    const parallaxOptions = {
      speed: options.speed || 0.5,
      direction: options.direction || 'vertical',
      ...options
    };
    
    // Skip effect if reduced motion is preferred
    if (isReducedMotionPreferred()) {
      return null;
    }
    
    let animation;
    const scrollTriggerOptions = {
      trigger: element,
      start: options.start || "top bottom",
      end: options.end || "bottom top",
      scrub: options.scrub !== undefined ? options.scrub : true,
      markers: options.markers || false,
      ...options.scrollTrigger
    };
    
    if (parallaxOptions.direction === 'vertical') {
      animation = gsap.fromTo(element,
        { y: parallaxOptions.startY || 0 },
        {
          y: parallaxOptions.endY || (parallaxOptions.speed * 100),
          ease: "none",
          scrollTrigger: scrollTriggerOptions
        }
      );
    } else if (parallaxOptions.direction === 'horizontal') {
      animation = gsap.fromTo(element,
        { x: parallaxOptions.startX || 0 },
        {
          x: parallaxOptions.endX || (parallaxOptions.speed * 100),
          ease: "none",
          scrollTrigger: scrollTriggerOptions
        }
      );
    }
    
    if (animation) {
      animations.push({
        element,
        animation,
        scrollTrigger: animation.scrollTrigger
      });
    }
    
    return animation?.scrollTrigger;
  };
  
  /**
   * Create a scroll-triggered timeline
   * @param {Object} options - Timeline options
   * @returns {Object} Timeline and ScrollTrigger
   */
  const createScrollTimeline = (options) => {
    const {
      trigger,
      animation: animationFn,
      start,
      end,
      scrub,
      pin,
      anticipatePin,
      markers,
      ...otherOptions
    } = options;
    
    // Skip if reduced motion is preferred
    if (isReducedMotionPreferred() && !options.ignoreReducedMotion) {
      // Show all elements immediately
      if (typeof trigger === 'string') {
        gsap.set(document.querySelectorAll(trigger), { opacity: 1 });
      } else if (trigger) {
        gsap.set(trigger, { opacity: 1 });
      }
      return null;
    }
    
    // Create ScrollTrigger
    const scrollTriggerOptions = {
      trigger,
      start: start || config.defaults.start,
      end: end || config.defaults.end,
      scrub: scrub !== undefined ? scrub : config.defaults.scrub,
      pin: pin !== undefined ? pin : false,
      anticipatePin: anticipatePin || 0,
      markers: markers !== undefined ? markers : config.defaults.markers,
      ...otherOptions
    };
    
    // Create timeline
    const timeline = gsap.timeline({
      scrollTrigger: scrollTriggerOptions
    });
    
    // Add animations to timeline via callback
    if (typeof animationFn === 'function') {
      animationFn(timeline, gsap);
    }
    
    // Store animation reference
    animations.push({
      trigger,
      timeline,
      scrollTrigger: timeline.scrollTrigger
    });
    
    return {
      timeline,
      scrollTrigger: timeline.scrollTrigger
    };
  };
  
  // Step 5: Initialize and return controller
  return {
    // Core functions
    createScrollAnimation,
    createBatchScrollAnimations,
    createParallaxEffect,
    createScrollTimeline,
    
    // Utility methods
    refresh: () => {
      ScrollTrigger.refresh();
    },
    
    getAnimations: () => [...animations],
    
    // Kill all animations created by this controller
    kill: () => {
      animations.forEach(anim => {
        if (anim.scrollTrigger) {
          anim.scrollTrigger.kill();
        }
        if (anim.timeline) {
          anim.timeline.kill();
        }
        if (anim.animation) {
          anim.animation.kill();
        }
      });
      
      animations.length = 0;
    },
    
    // Update configuration
    updateConfig: (newConfig) => {
      Object.assign(config, newConfig);
    },
    
    // Check status
    isReducedMotionActive: isReducedMotionPreferred,
    isDisabledByMediaQuery: isDisabledByMediaQuery
  };
}

// Example usage:
const scrollAnimator = createGSAPScrollAnimations({
  defaults: {
    once: true,
    markers: false
  },
  // Custom animation types
  types: {
    reveal: {
      fromVars: { opacity: 0, y: 50, scale: 0.9 },
      toVars: { opacity: 1, y: 0, scale: 1 }
    }
  }
});

// Simple animations with predefined types
scrollAnimator.createBatchScrollAnimations('.fade-in-element', 'fadeIn', {
  stagger: 0.15,
  scrollTrigger: {
    start: 'top 75%'
  }
});

scrollAnimator.createBatchScrollAnimations('.slide-in-left', 'slideInLeft');
scrollAnimator.createBatchScrollAnimations('.slide-in-right', 'slideInRight');

// Custom animation
scrollAnimator.createScrollAnimation('#hero-image', {
  fromVars: {
    scale: 0.8,
    opacity: 0,
    rotation: -5
  },
  toVars: {
    scale: 1,
    opacity: 1,
    rotation: 0
  }
}, {
  duration: 1.2,
  ease: 'power3.out'
});

// Parallax effects
scrollAnimator.createParallaxEffect('.parallax-bg', {
  speed: 0.3,
  direction: 'vertical'
});

// Complex timeline
scrollAnimator.createScrollTimeline({
  trigger: '#section-features',
  pin: true,
  scrub: 0.5,
  start: 'top top',
  end: '+=300%',
  animation: (timeline, gsap) => {
    timeline.from('.feature-1', { opacity: 0, x: -100 })
           .from('.feature-2', { opacity: 0, y: 100 }, '+=0.5')
           .from('.feature-3', { opacity: 0, x: 100 }, '+=0.5')
           .from('.feature-cta', { opacity: 0, scale: 0.5 }, '+=1');
  }
});

// Refresh ScrollTrigger when content changes
function onContentLoaded() {
  // After content loads or changes
  scrollAnimator.refresh();
}
```

#### 7.2.5 SVG Animation Implementation Template

```javascript
/**
 * Factory function to create SVG animations with GSAP
 * 
 * @param {string|SVGElement} svg - SVG element or selector
 * @param {Object} options - Configuration options
 * @returns {Object} SVG animation controller
 */
function createGSAPSVGAnimations(svg, options = {}) {
  // Step 1: Get SVG element
  const svgElement = typeof svg === 'string' ? document.querySelector(svg) : svg;
  
  if (!svgElement || svgElement.tagName.toLowerCase() !== 'svg') {
    console.error('Invalid SVG element provided');
    return null;
  }
  
  // Step 2: Configure with defaults
  const config = {
    // Default durations
    duration: {
      draw: options.duration?.draw || 1.5,
      morph: options.duration?.morph || 1,
      fill: options.duration?.fill || 0.7
    },
    
    // Default easings
    ease: {
      draw: options.ease?.draw || 'power2.inOut',
      morph: options.ease?.morph || 'power2.inOut',
      fill: options.ease?.fill || 'power1.in'
    },
    
    // Default colors
    colors: options.colors || {
      stroke: svgElement.getAttribute('stroke') || '#000000',
      fill: svgElement.getAttribute('fill') || '#000000'
    },
    
    // Plugin detection
    plugins: {
      drawSVG: typeof DrawSVGPlugin !== 'undefined',
      morphSVG: typeof MorphSVGPlugin !== 'undefined'
    },
    
    // Default draw options
    drawSequential: options.drawSequential !== undefined ? options.drawSequential : true,
    stagger: options.stagger || 0.05,
    
    ...options
  };
  
  // Step 3: Store references to animations
  const animations = {
    timelines: {},
    tweens: {}
  };
  
  // Step 4: Create utility functions
  
  /**
   * Draw SVG paths sequentially or all at once
   * @param {Object} options - Draw animation options
   * @returns {Object} Timeline
   */
  const drawSVG = (options = {}) => {
    const {
      paths = svgElement.querySelectorAll('path, line, polyline, polygon, rect, circle, ellipse'),
      duration = config.duration.draw,
      ease = config.ease.draw,
      stagger = config.stagger,
      sequential = config.drawSequential,
      reverse = false,
      onComplete
    } = options;
    
    // Create timeline
    const timeline = gsap.timeline({
      onComplete,
      paused: options.paused !== undefined ? options.paused : false
    });
    
    // Use different techniques based on plugin availability
    if (config.plugins.drawSVG) {
      // Use DrawSVG plugin
      if (sequential) {
        // Draw each path sequentially
        Array.from(paths).forEach((path, index) => {
          timeline.fromTo(path, 
            { drawSVG: "0%" },
            { 
              drawSVG: "100%", 
              duration: duration,
              ease: ease
            },
            index === 0 ? 0 : (reverse ? "-=" + stagger : "+=" + stagger)
          );
        });
      } else {
        // Draw all paths with stagger
        timeline.fromTo(paths, 
          { drawSVG: "0%" },
          { 
            drawSVG: "100%", 
            duration: duration,
            stagger: stagger,
            ease: ease
          }
        );
      }
    } else {
      // Fallback to strokeDasharray/strokeDashoffset technique
      const pathArray = Array.from(paths);
      
      // Set initial state - measure path lengths and set dasharray/offset
      pathArray.forEach(path => {
        // Get length or approximate if not a path
        let length;
        if (path.getTotalLength) {
          length = path.getTotalLength();
        } else {
          // Approximate length for non-path elements
          const box = path.getBBox();
          length = (box.width + box.height) * 2;
        }
        
        gsap.set(path, {
          strokeDasharray: length,
          strokeDashoffset: length,
          stroke: path.getAttribute('stroke') || config.colors.stroke,
          fill: 'rgba(0,0,0,0)' // Start with no fill
        });
      });
      
      // Create animation
      if (sequential) {
        // Draw each path sequentially
        pathArray.forEach((path, index) => {
          timeline.to(path, { 
            strokeDashoffset: 0, 
            duration: duration,
            ease: ease
          }, index === 0 ? 0 : (reverse ? "-=" + stagger : "+=" + stagger));
        });
      } else {
        // Draw all paths with stagger
        timeline.to(paths, { 
          strokeDashoffset: 0, 
          duration: duration,
          stagger: stagger,
          ease: ease
        });
      }
    }
    
    // Store animation
    animations.timelines.draw = timeline;
    
    return timeline;
  };
  
  /**
   * Morph between SVG paths
   * @param {string|SVGPathElement} fromPath - Source path or selector
   * @param {string|SVGPathElement} toPath - Target path or selector
   * @param {Object} options - Morph animation options
   * @returns {Object} Tween
   */
  const morphSVG = (fromPath, toPath, options = {}) => {
    const fromElement = typeof fromPath === 'string' ? svgElement.querySelector(fromPath) : fromPath;
    const toElement = typeof toPath === 'string' ? svgElement.querySelector(toPath) : toPath;
    
    if (!fromElement || !toElement) {
      console.error('Invalid paths for morphing');
      return null;
    }
    
    const {
      duration = config.duration.morph,
      ease = config.ease.morph,
      onComplete
    } = options;
    
    let morphAnimation;
    
    // Use different techniques based on plugin availability
    if (config.plugins.morphSVG) {
      // Use MorphSVG plugin
      morphAnimation = gsap.to(fromElement, {
        morphSVG: {
          shape: toElement,
          type: options.morphType || "rotational",
          shapeIndex: options.shapeIndex
        },
        duration: duration,
        ease: ease,
        onComplete,
        paused: options.paused !== undefined ? options.paused : false
      });
    } else {
      // Fallback - just change opacity between elements
      console.warn('MorphSVGPlugin not available, using opacity transition fallback');
      
      morphAnimation = gsap.timeline({
        onComplete,
        paused: options.paused !== undefined ? options.paused : false
      })
        .to(fromElement, { opacity: 0, duration: duration / 2, ease: ease })
        .set(toElement, { opacity: 0, display: 'block' })
        .to(toElement, { opacity: 1, duration: duration / 2, ease: ease });
    }
    
    // Store animation
    animations.tweens.morph = morphAnimation;
    
    return morphAnimation;
  };
  
  /**
   * Animate fill color for SVG elements
   * @param {string|SVGElement|NodeList} elements - Elements to animate
   * @param {string} color - Target color
   * @param {Object} options - Fill animation options
   * @returns {Object} Tween
   */
  const fillSVG = (elements, color, options = {}) => {
    const targetElements = typeof elements === 'string' ? 
      svgElement.querySelectorAll(elements) : elements;
    
    if (!targetElements) {
      console.error('Invalid elements for fill animation');
      return null;
    }
    
    const {
      duration = config.duration.fill,
      ease = config.ease.fill,
      delay = 0,
      stagger = 0,
      onComplete
    } = options;
    
    const fillAnimation = gsap.to(targetElements, {
      fill: color || config.colors.fill,
      duration: duration,
      delay: delay,
      stagger: stagger,
      ease: ease,
      onComplete,
      paused: options.paused !== undefined ? options.paused : false
    });
    
    // Store animation
    animations.tweens.fill = fillAnimation;
    
    return fillAnimation;
  };
  
  /**
   * Create an SVG motion path animation
   * @param {string|SVGElement} element - Element to animate along path
   * @param {string|SVGPathElement} path - Path to follow
   * @param {Object} options - Motion path options
   * @returns {Object} Tween
   */
  const followPath = (element, path, options = {}) => {
    const targetElement = typeof element === 'string' ? 
      svgElement.querySelector(element) : element;
      
    const pathElement = typeof path === 'string' ? 
      svgElement.querySelector(path) : path;
    
    if (!targetElement || !pathElement) {
      console.error('Invalid element or path for motion path animation');
      return null;
    }
    
    const {
      duration = 2,
      ease = "power1.inOut",
      align = true,
      alignOrigin = [0.5, 0.5],
      autoRotate = true,
      start = 0,
      end = 1
    } = options;
    
    // Check if MotionPathPlugin is available
    if (typeof MotionPathPlugin === 'undefined') {
      console.error('MotionPathPlugin required for path following');
      return null;
    }
    
    // Register plugin if needed
    if (gsap.registerPlugin) {
      gsap.registerPlugin(MotionPathPlugin);
    }
    
    const pathAnimation = gsap.fromTo(targetElement,
      { motionPath: { path: pathElement, align: align, alignOrigin: alignOrigin, autoRotate: autoRotate, start: start } },
      { 
        motionPath: { path: pathElement, align: align, alignOrigin: alignOrigin, autoRotate: autoRotate, end: end },
        duration: duration,
        ease: ease,
        paused: options.paused !== undefined ? options.paused : false
      }
    );
    
    // Store animation
    animations.tweens.path = pathAnimation;
    
    return pathAnimation;
  };
  
  /**
   * Create a complete animation sequence for the SVG
   * @param {Object} options - Sequence options
   * @returns {Object} Timeline
   */
  const createSequence = (options = {}) => {
    const {
      drawPaths = true,
      fillElements = true,
      morphPaths = null,
      stagger = config.stagger,
      totalDuration,
      paused = true
    } = options;
    
    // Create master timeline
    const masterTimeline = gsap.timeline({
      paused: paused
    });
    
    // Add draw animation if requested
    if (drawPaths) {
      const drawOptions = { 
        ...options.drawOptions,
        paused: true // Create paused and add to master
      };
      
      const drawTimeline = drawSVG(drawOptions);
      masterTimeline.add(drawTimeline);
    }
    
    // Add morph animation if paths provided
    if (morphPaths && morphPaths.from && morphPaths.to) {
      const morphOptions = {
        ...options.morphOptions,
        paused: true
      };
      
      const morphTimeline = morphSVG(morphPaths.from, morphPaths.to, morphOptions);
      
      if (options.morphPosition) {
        masterTimeline.add(morphTimeline, options.morphPosition);
      } else if (drawPaths) {
        masterTimeline.add(morphTimeline, `>-${stagger * 2}`); // Slight overlap
      } else {
        masterTimeline.add(morphTimeline);
      }
    }
    
    // Add fill animation if requested
    if (fillElements) {
      const elements = options.fillTargets || 'path, rect, circle, ellipse, polygon, polyline';
      const color = options.fillColor || config.colors.fill;
      
      const fillOptions = {
        ...options.fillOptions,
        paused: true
      };
      
      const fillTimeline = fillSVG(elements, color, fillOptions);
      
      if (options.fillPosition) {
        masterTimeline.add(fillTimeline, options.fillPosition);
      } else if (drawPaths || morphPaths) {
        masterTimeline.add(fillTimeline, `>-${stagger * 3}`); // Overlap
      } else {
        masterTimeline.add(fillTimeline);
      }
    }
    
    // Add any custom animations
    if (options.customAnimations && typeof options.customAnimations === 'function') {
      options.customAnimations(masterTimeline, gsap, svgElement);
    }
    
    // Set total duration if provided
    if (totalDuration) {
      masterTimeline.totalDuration(totalDuration);
    }
    
    // Store animation
    animations.timelines.master = masterTimeline;
    
    return masterTimeline;
  };
  
  // Step 5: Return controller
  return {
    // Core animation functions
    drawSVG,
    morphSVG,
    fillSVG,
    followPath,
    createSequence,
    
    // Utility functions
    getSVGElement: () => svgElement,
    getConfig: () => ({...config}),
    getAnimations: () => ({...animations}),
    
    // Update configuration
    updateConfig: (newConfig) => {
      Object.assign(config, newConfig);
    },
    
    // Kill all animations
    kill: () => {
      // Kill all timelines
      Object.values(animations.timelines).forEach(timeline => {
        if (timeline && timeline.kill) {
          timeline.kill();
        }
      });
      
      // Kill all tweens
      Object.values(animations.tweens).forEach(tween => {
        if (tween && tween.kill) {
          tween.kill();
        }
      });
      
      // Clear animations references
      animations.timelines = {};
      animations.tweens = {};
    }
  };
}

// Example usage:
const logoAnimation = createGSAPSVGAnimations('#logo-svg', {
  duration: { 
    draw: 2,
    fill: 0.5
  },
  colors: {
    stroke: '#3498db',
    fill: '#2980b9'
  }
});

// Create a complete animation sequence
const sequence = logoAnimation.createSequence({
  drawPaths: true,
  fillElements: true,
  drawOptions: {
    stagger: 0.1,
    sequential: true
  },
  fillOptions: {
    stagger: 0.05,
    delay: 0.3
  },
  customAnimations: (timeline, gsap, svg) => {
    // Add custom animations to the timeline
    timeline.to(svg.querySelector('#icon-element'), {
      scale: 1.2,
      duration: 0.5,
      repeat: 1,
      yoyo: true
    }, ">-0.2");
  }
});

// Play the sequence
sequence.play();

// Alternate approach with scroll trigger
ScrollTrigger.create({
  trigger: '#logo-section',
  start: 'top center',
  onEnter: () => sequence.play(),
  onLeaveBack: () => sequence.reverse()
});
```

## 8. Implementation Planning Algorithm

### 8.1 Implementation Phase Planning Matrix

LLMs should use this structured implementation approach when planning GSAP integration:

```json
{
  "implementation_phases": [
    {
      "phase": "FOUNDATION",
      "timeline": "PHASE_1",
      "duration": "5_DAYS",
      "dependencies": [],
      "tasks": [
        {
          "id": "FOUNDATION_01",
          "name": "Library Integration",
          "description": "Implement GSAP and necessary plugins into the build system",
          "complexity": "LOW",
          "priority": "HIGH",
          "steps": [
            "Install GSAP via package manager",
            "Import and register core plugins (ScrollTrigger, etc.)",
            "Configure with global defaults for the project",
            "Set up breakpoint detection for responsive animations"
          ]
        },
        {
          "id": "FOUNDATION_02",
          "name": "Framework Integration",
          "description": "Create framework-specific wrappers and utilities",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Implement React hook/component for GSAP",
            "Create Vue directive/composable for GSAP",
            "Develop context system for scoped animations",
            "Build cleanup utilities to prevent memory leaks"
          ]
        },
        {
          "id": "FOUNDATION_03", 
          "name": "Animation Design System",
          "description": "Define consistent animation parameters across project",
          "complexity": "MEDIUM",
          "priority": "MEDIUM",
          "steps": [
            "Define standard durations and easing functions",
            "Create animation presets for common transitions",
            "Establish responsive animation behavior rules",
            "Document animation guidelines and usage"
          ]
        }
      ]
    },
    {
      "phase": "CORE_IMPLEMENTATION",
      "timeline": "PHASE_2",
      "duration": "10_DAYS",
      "dependencies": ["FOUNDATION"],
      "tasks": [
        {
          "id": "CORE_01",
          "name": "Basic Animations",
          "description": "Implement standard UI animations",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Create button and interactive element animations",
            "Implement modal/drawer transitions",
            "Develop form element animations",
            "Build notification and alert animations"
          ]
        },
        {
          "id": "CORE_02",
          "name": "Page Transitions",
          "description": "Implement smooth transitions between pages/views",
          "complexity": "HIGH",
          "priority": "HIGH",
          "steps": [
            "Create exit/enter animation system",
            "Integrate with router/navigation",
            "Implement shared element transitions",
            "Build history state handling"
          ]
        },
        {
          "id": "CORE_03",
          "name": "Scroll Animations",
          "description": "Implement scroll-triggered animations",
          "complexity": "HIGH",
          "priority": "MEDIUM",
          "steps": [
            "Set up ScrollTrigger for content reveals",
            "Create parallax effects system",
            "Implement scroll-based timelines",
            "Build scrolling progress indicators"
          ]
        }
      ]
    },
    {
      "phase": "ADVANCED_FEATURES",
      "timeline": "PHASE_3",
      "duration": "10_DAYS", 
      "dependencies": ["FOUNDATION", "CORE_IMPLEMENTATION"],
      "tasks": [
        {
          "id": "ADVANCED_01",
          "name": "Complex Sequences",
          "description": "Implement multi-step animation sequences",
          "complexity": "HIGH",
          "priority": "MEDIUM",
          "steps": [
            "Create timeline orchestration system",
            "Implement animation state management",
            "Develop complex staggered animations",
            "Build animation triggers and callbacks system"
          ]
        },
        {
          "id": "ADVANCED_02",
          "name": "SVG Animations",
          "description": "Implement SVG-specific animations",
          "complexity": "HIGH",
          "priority": "MEDIUM",
          "steps": [
            "Create SVG path animations",
            "Implement SVG morphing system",
            "Develop SVG drawing sequences",
            "Build SVG interaction animations"
          ]
        },
        {
          "id": "ADVANCED_03",
          "name": "Performance Optimizations",
          "description": "Optimize animations for performance",
          "complexity": "HIGH",
          "priority": "HIGH",
          "steps": [
            "Implement device capability detection",
            "Create fallbacks for low-end devices",
            "Optimize for reduced motion preferences",
            "Build animation batching and throttling"
          ]
        }
      ]
    },
    {
      "phase": "TESTING_REFINEMENT",
      "timeline": "PHASE_4",
      "duration": "5_DAYS",
      "dependencies": ["FOUNDATION", "CORE_IMPLEMENTATION", "ADVANCED_FEATURES"],
      "tasks": [
        {
          "id": "TESTING_01",
          "name": "Cross-browser Testing",
          "description": "Test animations across browsers and devices",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Test on Chrome, Firefox, Safari, Edge",
            "Verify on iOS and Android devices",
            "Test on low-powered devices",
            "Address browser-specific issues"
          ]
        },
        {
          "id": "TESTING_02",
          "name": "Performance Profiling",
          "description": "Profile and optimize animation performance",
          "complexity": "HIGH",
          "priority": "HIGH",
          "steps": [
            "Measure FPS and jank across animations",
            "Identify and fix bottlenecks",
            "Test with slowdown simulation",
            "Optimize CPU and memory usage"
          ]
        },
        {
          "id": "TESTING_03",
          "name": "Accessibility Review",
          "description": "Ensure animations respect accessibility needs",
          "complexity": "MEDIUM",
          "priority": "HIGH",
          "steps": [
            "Implement reduced motion support",
            "Test with screen readers",
            "Verify keyboard navigation during animations",
            "Ensure animations don't cause vestibular issues"
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
 * Algorithm for determining optimal GSAP implementation approach
 * 
 * @param {Object} context - Project context parameters
 * @returns {Object} Implementation recommendation
 */
function determineGSAPImplementationApproach(context) {
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
          needsCleanup: context.platformContext.includes("React") || context.platformContext.includes("Vue"),
          requiresCustomIntegration: context.platformContext === "STATIC" || context.platformContext === "SERVER-RENDERED"
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
      performance: constraints.performance.critical ? 10 :
                  constraints.performance.important ? 8 : 5,
                  
      bundleSize: constraints.technical.hostingLimitations.bundleSizeCritical ? 9 : 
                 constraints.technical.hostingLimitations.limitedResources ? 7 : 4,
                 
      featureRichness: constraints.complexity.complex ? 10 :
                      constraints.complexity.moderate ? 7 : 4,
                      
      maintainability: constraints.technical.frameworkLimitations.needsCleanup ? 8 : 6,
                     
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
    
    // Determine plugin selection
    const pluginSelection = determinePluginSelection(priorities, context);
    
    // Determine optimization strategy
    const optimizationStrategy = determineOptimizationStrategy(priorities, context);
    
    // Determine framework integration approach
    const frameworkIntegration = determineFrameworkIntegration(context);
    
    // Create phase structure
    implementationPhases.push({
      phase: "Initial Setup",
      focus: "Core configuration and essential plugins",
      plugins: pluginSelection.essentialPlugins,
      approach: frameworkIntegration,
      duration: "3-5 days"
    });
    
    implementationPhases.push({
      phase: "Basic Animations",
      focus: "Common UI animations and transitions",
      features: ["Basic transitions", "UI interactions", "Simple sequences"],
      duration: "5-8 days"
    });
    
    // Add appropriate advanced phases based on priorities
    if (priorities.primaryFocus === "featureRichness" || pluginSelection.advancedPlugins.length > 0) {
      implementationPhases.push({
        phase: "Advanced Features",
        focus: "Complex animations and special effects",
        plugins: pluginSelection.advancedPlugins,
        features: ["Complex sequences", "Scroll-driven animations", "SVG animations"],
        duration: "7-10 days"
      });
    }
    
    implementationPhases.push({
      phase: "Optimization",
      focus: "Performance tuning and testing",
      strategy: optimizationStrategy,
      duration: "5-7 days"
    });
    
    return {
      phases: implementationPhases,
      pluginSelection,
      optimizationStrategy,
      frameworkIntegration,
      totalEstimatedDuration: "20-30 days",
      approachSummary: `Focus on ${priorities.primaryFocus} with ${frameworkIntegration.name} integration approach`
    };
  }
  
  function determinePluginSelection(priorities, context) {
    // Essential plugins based on requirements
    const essentialPlugins = ["Core GSAP"];
    
    // Determine if ScrollTrigger is needed
    if (context.animationComplexity !== "SIMPLE" || 
        context.description?.toLowerCase().includes("scroll")) {
      essentialPlugins.push("ScrollTrigger");
    }
    
    // Determine advanced plugins based on complexity and priorities
    const advancedPlugins = [];
    
    if (context.animationComplexity === "ADVANCED") {
      if (context.description?.toLowerCase().includes("svg") ||
          context.description?.toLowerCase().includes("morph")) {
        advancedPlugins.push("MorphSVGPlugin");
      }
      
      if (context.description?.toLowerCase().includes("draw") ||
          context.description?.toLowerCase().includes("path")) {
        advancedPlugins.push("DrawSVGPlugin");
      }
      
      if (context.description?.toLowerCase().includes("follow path") ||
          context.description?.toLowerCase().includes("motion path")) {
        advancedPlugins.push("MotionPathPlugin");
      }
      
      if (context.description?.toLowerCase().includes("draggable") ||
          context.description?.toLowerCase().includes("drag")) {
        advancedPlugins.push("Draggable");
      }
      
      if (context.description?.toLowerCase().includes("scroll smooth") ||
          context.description?.toLowerCase().includes("smooth scroll")) {
        advancedPlugins.push("ScrollToPlugin");
      }
      
      if (context.description?.toLowerCase().includes("flip") ||
          context.description?.toLowerCase().includes("layout change")) {
        advancedPlugins.push("Flip");
      }
    }
    
    // If bundle size is critical, limit plugins
    if (priorities.primaryFocus === "bundleSize" || 
        priorities.scores.bundleSize > 8) {
      // Keep only the most essential advanced plugins
      const necessaryPlugins = advancedPlugins.filter(plugin => 
        context.description?.toLowerCase().includes(plugin.toLowerCase().replace("plugin", "")));
      
      return {
        essentialPlugins,
        advancedPlugins: necessaryPlugins.slice(0, 2) // Limit to at most 2
      };
    }
    
    return {
      essentialPlugins,
      advancedPlugins
    };
  }
  
  function determineOptimizationStrategy(priorities, context) {
    // Determine optimization strategy based on priorities
    if (priorities.primaryFocus === "performance" || 
        priorities.scores.performance >= 8) {
      return {
        name: "Performance-First",
        description: "Optimize heavily for performance, even at cost of features",
        techniques: [
          "Use transform/opacity exclusively",
          "Limit concurrent animations",
          "Apply different animation complexity based on device capability",
          "Implement batching and throttling",
          "Set gsap.ticker.lagSmoothing(0) in production",
          "Implement aggressive cleanup"
        ]
      };
    } else if (priorities.primaryFocus === "bundleSize" || 
              priorities.scores.bundleSize >= 8) {
      return {
        name: "Minimal Bundle",
        description: "Minimize GSAP footprint",
        techniques: [
          "Import only core GSAP",
          "Use only essential plugins",
          "Lazy load advanced plugins",
          "Implement fallbacks for plugin features",
          "Prefer CSS animations for simple transitions"
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
          "Implement responsive animations",
          "Utilize GSAP's full plugin ecosystem"
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
          "Optimize high-visibility animations",
          "Use a limited set of plugins"
        ]
      };
    }
  }
  
  function determineFrameworkIntegration(context) {
    // Determine integration approach based on framework
    if (context.platformContext.includes("React")) {
      return {
        name: "React Integration",
        description: "Integrate GSAP with React component lifecycle",
        implementation: "Use useRef, useEffect, and gsap.context() for proper scoping",
        examples: [
          "Custom hooks for common animations",
          "Component wrapper for GSAP animations",
          "Cleanup on component unmount",
          "Integration with React Router"
        ]
      };
    } else if (context.platformContext.includes("Vue")) {
      return {
        name: "Vue Integration",
        description: "Integrate GSAP with Vue component lifecycle",
        implementation: "Use refs, mounted/unmounted lifecycle hooks, and directives",
        examples: [
          "Custom directives for animations",
          "Composables for reusable animations",
          "Proper cleanup in beforeUnmount",
          "Integration with Vue Router"
        ]
      };
    } else if (context.platformContext.includes("SERVER-RENDERED")) {
      return {
        name: "Server-Rendered Integration",
        description: "Ensure GSAP works with server-rendered HTML",
        implementation: "Initialize after DOM content loaded, handle hydration",
        examples: [
          "DOM-ready event listeners",
          "Script loading after content",
          "Hydration-safe animations",
          "Progressive enhancement approach"
        ]
      };
    } else {
      return {
        name: "Vanilla JS Integration",
        description: "Pure JavaScript implementation without framework dependencies",
        implementation: "Modular functions and classes for reusable animations",
        examples: [
          "Event-based animation triggers",
          "Element selector utilities",
          "Animation factory functions",
          "DOM mutation observers for dynamic content"
        ]
      };
    }
  }
}

// Example usage:
const projectContext = {
  performanceTarget: "MEDIUM",
  platformContext: "MODERN-FRAMEWORK React",
  animationComplexity: "ADVANCED",
  hostingEnvironment: "CLOUD",
  description: "Interactive dashboard with scroll animations, SVG charts, and page transitions"
};

const implementationPlan = determineGSAPImplementationApproach(projectContext);
console.log('Implementation Plan:', implementationPlan);
```

## 9. Information Processing and Decision-Making Framework

### 9.1 Implementation Decision Tree For LLMs

```
START GSAP_IMPLEMENTATION
├── Analyze Project Requirements
│   ├── Determine Performance Target (LOW/MEDIUM/HIGH)
│   ├── Identify Platform Context (SERVER-RENDERED/MODERN-FRAMEWORK/STATIC)
│   ├── Assess Animation Complexity (SIMPLE/INTERMEDIATE/ADVANCED)
│   └── Evaluate Hosting Environment (SHARED_HOSTING/DEDICATED/CLOUD)
│
├── Select Core GSAP Components
│   ├── Is performance critical?
│   │   ├── YES
│   │   │   └── USE: Core GSAP only with minimal plugins
│   │   └── NO
│   │       ├── Is animation complexity ADVANCED?
│   │       │   ├── YES
│   │       │   │   └── USE: Full GSAP with necessary plugins
│   │       │   └── NO
│   │       │       └── USE: Core GSAP + essential plugins only
│
├── Determine ScrollTrigger Requirement
│   ├── Are scroll animations needed?
│   │   ├── YES
│   │   │   └── INCLUDE: ScrollTrigger plugin
│   │   └── NO
│   │       └── EXCLUDE: ScrollTrigger plugin
│
├── Apply Framework-Specific Integration
│   ├── Platform is React?
│   │   ├── YES
│   │   │   └── IMPLEMENT: useRef + useEffect + gsap.context pattern
│   │   └── NO
│   │       ├── Platform is Vue?
│   │       │   ├── YES
│   │       │   │   └── IMPLEMENT: mounted + beforeUnmount lifecycle pattern
│   │       │   └── NO
│   │       │       ├── Platform is SERVER-RENDERED?
│   │       │       │   ├── YES
│   │       │       │   │   └── IMPLEMENT: DOMContentLoaded pattern
│   │       │       │   └── NO
│   │       │       │       └── IMPLEMENT: Standard module pattern
│
├── Select Animation Patterns
│   ├── Determine Required Animation Types:
│   │   ├── UI transitions → GSAP tweens
│   │   ├── Sequences → GSAP timeline
│   │   ├── Scroll animations → ScrollTrigger
│   │   ├── SVG animations → Select appropriate SVG plugins
│   │   └── Physics/draggable → Draggable/physics plugins
│
├── Implement Performance Optimizations
│   ├── Performance Target is LOW?
│   │   ├── YES
│   │   │   └── IMPLEMENT: Maximum optimization strategy
│   │   │       ├── Force 3D rendering
│   │   │       ├── Use transform/opacity only
│   │   │       ├── Implement simplified animations for low-end devices
│   │   │       └── Batch DOM operations
│   │   └── NO
│   │       └── IMPLEMENT: Standard optimization strategy
│   │           ├── Prefer transform/opacity
│   │           ├── Use timeline for related animations
│   │           ├── Implement device detection
│   │           └── Respect reduced motion preferences
│
└── END GSAP_IMPLEMENTATION
```

### 9.2 Final Recommendation Matrix

| Context Parameter | Primary Recommendation | Alternative Recommendation |
|-------------------|------------------------|---------------------------|
| LOW + SHARED_HOSTING | Use core GSAP only, focus on transform/opacity, minimal plugins | Consider CSS animations for simplest cases, load GSAP only when needed |
| MEDIUM + ANY_HOSTING | Use GSAP with essential plugins, implement device detection | Consider anime.js for very small projects with simple needs |
| HIGH + ANY_HOSTING | Use full GSAP ecosystem with optimization, implement advanced features | Consider specialized libraries (Three.js) alongside GSAP for specific effects |
| REACT + ANY | Use GSAP with gsap.context() and React hooks, proper cleanup in useEffect | Consider Framer Motion for simpler UI animations if already using React |
| VUE + ANY | Use GSAP with Vue lifecycle hooks, implement proper cleanup | Consider dedicated Vue animation libraries for simpler needs |
| SERVER-RENDERED + ANY | Use GSAP with careful DOM-ready handling, consider hydration impact | Implement progressive enhancement with basic CSS animations as fallback |

By systematically analyzing the context parameters and following the implementation decision tree, LLMs can provide optimal GSAP implementation guidance for any web development scenario while ensuring high performance across all target devices.
  }
}
</script>
```

#### 3.2.3 Server-Rendered (PHP/Laravel) Implementation Template

```php
<!-- Step 1: Create target element with unique ID -->
<div id="gsap-element-<?php echo $uniqueId; ?>" class="element">Animated Box</div>

<!-- Step 2: Add script after element definition -->
<script>
  // Step 2.1: Wait for DOM to be ready
  document.addEventListener('DOMContentLoaded', () => {
    // Step 2.2: Create animation sequence
    const timeline = gsap.timeline({
      scrollTrigger: {
        trigger: '#gsap-element-<?php echo $uniqueId; ?>',
        start: "top 80%",
        end: "bottom 20%",
        toggleActions: "play none none reverse"
      }
    });
    
    // Step 2.3: Add animation steps
    timeline.from('#gsap-element-<?php echo $uniqueId; ?>', {
      opacity: 0,
      y: 50,
      duration: 1,
      ease: "power3.out"
    }).to('#gsap-element-<?php echo $uniqueId; ?>', {
      x: 100,
      backgroundColor: "#3498db",
      duration: 0.8
    }, "+=0.2");
  });
</script>
```

#### 3.2.4 Astro Implementation Template

```astro
---
// Step 1: Set up component properties if needed
const { id = 'gsap-element' } = Astro.props;
---

<!-- Step 2: Create target element -->
<div id={id} class="element">Animated Box</div>

<!-- Step 3: Add client-side script with directives -->
<script>
  // Step 3.1: Import GSAP and plugins
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  
  // Step 3.2: Register plugins
  gsap.registerPlugin(ScrollTrigger);
  
  // Step 3.3: Initialize after DOM is ready
  document.addEventListener('DOMContentLoaded', () => {
    const elementId = document.currentScript.previousElementSibling.id;
    
    // Step 3.4: Create animation
    gsap.timeline({
      scrollTrigger: {
        trigger: `#${elementId}`,
        start: "top 80%",
        end: "bottom 20%",
        toggleActions: "play none none reverse"
      }
    })
    .from(`#${elementId}`, {
      opacity: 0,
      y: 50,
      duration: 1,
      ease: "power3.out"
    })
    .to(`#${elementId}`, {
      x: 100,
      backgroundColor: "#3498db",
      duration: 0.8
    }, "+=0.2");
  });
</script>
```

### 3.3 Optimization Algorithm for Performance Tuning

LLMs should follow this deterministic algorithm when implementing performance optimizations:

```
function determineGSAPOptimizationStrategy(context) {
  // Initialize optimization array
  const optimizations = [];
  
  // Step 1: Always use hardware-accelerated properties
  optimizations.push({
    type: "HARDWARE_ACCELERATION",
    implementation: "Use transform and opacity properties whenever possible",
    example: `
      // Preferred
      gsap.to(element, { 
        x: 100,
        opacity: 0.5
      });
      
      // Avoid
      gsap.to(element, {
        left: 100,
        opacity: 0.5
      });
    `
  });
  
  // Step 2: Analyze device target and adjust accordingly
  if (context.performanceTarget === "LOW") {
    optimizations.push({
      type: "FORCE_3D",
      implementation: "Force GPU rendering for all animations",
      example: `
        // Add force3D to the defaults
        gsap.defaults({force3D: true});
        
        // Or on individual tweens
        gsap.to(element, {
          x: 100,
          force3D: true
        });
      `
    });
    
    optimizations.push({
      type: "REDUCE_ANIMATIONS",
      implementation: "Limit number and complexity of animations",
      thresholds: {
        maxActiveTimelines: 3,
        maxElementsPerTimeline: 10,
        maxDuration: 1.5
      }
    });
  }
  
  // Step 3: Apply framework-specific optimizations
  if (context.platformContext === "MODERN-FRAMEWORK") {
    optimizations.push({
      type: "USE_GSAP_CONTEXT",
      implementation: "Use gsap.context() for clean scope and automatic cleanup",
      example: `
        // React example
        useEffect(() => {
          const ctx = gsap.context(() => {
            gsap.to(".box", {...});
          }, parentRef);
          
          return () => ctx.revert();
        }, []);
      `
    });
    
    optimizations.push({
      type: "BATCH_DOM_OPERATIONS",
      implementation: "Batch DOM reads and writes to avoid layout thrashing",
      example: `
        // Read phase - get all positions first
        const positions = elements.map(el => {
          const bounds = el.getBoundingClientRect();
          return {x: bounds.left, y: bounds.top};
        });
        
        // Write phase - update all at once
        elements.forEach((el, i) => {
          gsap.set(el, {x: positions[i].x, y: positions[i].y});
        });
      `
    });
  }
  
  // Step 4: Scroll optimization patterns
  if (context.animationComplexity.includes("SCROLL")) {
    optimizations.push({
      type: "SCROLL_OPTIMIZATION",
      implementation: "Optimize ScrollTrigger configuration",
      example: `
        // Use markers only in development
        ScrollTrigger.create({
          trigger: element,
          markers: process.env.NODE_ENV === 'development',
          
          // Invalidate on refresh for responsive layouts
          invalidateOnRefresh: true,
          
          // Only animate when in viewport for performance
          fastScrollEnd: true,
          
          // For multiple scroll animations, use a selector to batch them
          trigger: ".animate-section",
          
          // For animations on many elements, use a single ScrollTrigger with multiple animations
          onEnter: () => {
            gsap.to(".multiple-elements", {
              stagger: 0.1,
              y: 0,
              opacity: 1
            });
          }
        });
      `
    });
    
    optimizations.push({
      type: "PINNING_OPTIMIZATION",
      implementation: "Optimize pinned elements for smooth scrolling",
      example: `
        ScrollTrigger.create({
          trigger: section,
          pin: true,
          
          // Force hardware acceleration on pinned elements
          pinReparent: true,
          
          // Prevent overlapping pin conflicts
          anticipatePin: 1,
          
          // For long sections, use batching
          fastScrollEnd: true
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
function detectGSAPDeviceCapability() {
  // Create performance detection object
  const deviceCapability = {
    tier: "UNKNOWN",
    canRunComplexAnimations: false,
    recommendedMaxElements: 0,
    useHardwareAcceleration: true,
    recommendedPlugins: []
  };
  
  // Step 1: Check for baseline performance indicators
  const memory = navigator.deviceMemory || 4; // Default to middle value if not available
  const processors = navigator.hardwareConcurrency || 4;
  const isReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  const isLowPowerMode = 'getBattery' in navigator && (await navigator.getBattery()).charging === false;
  
  // Step 2: Run quick benchmark
  const benchmark = () => {
    const startTime = performance.now();
    const testElement = document.createElement('div');
    document.body.appendChild(testElement);
    
    // Create and immediately kill 100 tweens as benchmark
    for (let i = 0; i < 100; i++) {
      gsap.to(testElement, {x: 100, duration: 0.001, ease: "none"}).kill();
    }
    
    const endTime = performance.now();
    document.body.removeChild(testElement);
    
    return endTime - startTime;
  };
  
  const benchmarkScore = benchmark();
  
  // Step 3: Determine device tier
  if (isReducedMotion) {
    deviceCapability.tier = "ACCESSIBILITY";
    deviceCapability.canRunComplexAnimations = false;
    deviceCapability.recommendedMaxElements = 0;
    deviceCapability.recommendedPlugins = [];
  } else if (memory <= 2 || processors <= 2 || benchmarkScore > 200 || isLowPowerMode) {
    deviceCapability.tier = "LOW";
    deviceCapability.canRunComplexAnimations = false;
    deviceCapability.recommendedMaxElements = 100;
    deviceCapability.recommendedPlugins = ["core only"];
  } else if ((memory <= 4 && processors <= 4) || benchmarkScore > 100) {
    deviceCapability.tier = "MEDIUM";
    deviceCapability.canRunComplexAnimations = true;
    deviceCapability.recommendedMaxElements = 500;
    deviceCapability.recommendedPlugins = ["ScrollTrigger", "basic plugins"];
  } else {
    deviceCapability.tier = "HIGH";
    deviceCapability.canRunComplexAnimations = true;
    deviceCapability.recommendedMaxElements = 2000;
    deviceCapability.recommendedPlugins = ["All plugins as needed"];
  }
  
  // Step 4: Adjust GSAP defaults based on capability
  if (deviceCapability.tier === "LOW") {
    gsap.defaults({
      ease: "power1.out", // Simpler easing functions
      duration: 0.5,      // Shorter animations
      force3D: true       // Force GPU acceleration
    });
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
      "implementation": "gsap.to(), gsap.from(), gsap.fromTo()"
    },
    "SEQUENCE_ANIMATION": {
      "description": "Multiple animations in sequence or parallel",
      "complexity": "INTERMEDIATE",
      "use_case": "Multi-step animations, complex UI transitions",
      "implementation": "gsap.timeline()"
    },
    "STAGGERED_ANIMATION": {
      "description": "Offset timing across multiple elements",
      "complexity": "INTERMEDIATE",
      "use_case": "Lists, grids, particle effects",
      "implementation": "stagger property in tween"
    },
    "SCROLL_ANIMATION": {
      "description": "Animations triggered by scroll position",
      "complexity": "INTERMEDIATE TO ADVANCED", 
      "use_case": "Content reveals, parallax effects, pinned sections",
      "implementation": "ScrollTrigger plugin"
    },
    "SVG_ANIMATION": {
      "description": "Manipulating SVG paths and properties",
      "complexity": "ADVANCED",
      "use_case": "Logos, illustrations, data visualizations",
      "implementation": "DrawSVG or MorphSVG plugins"
    },
    "PHYSICS_ANIMATION": {
      "description": "Physics-based motion and interactions",
      "complexity": "ADVANCED",
      "use_case": "Natural motion, draggable elements",
      "implementation": "Physics2D plugin or custom easing/callbacks"
    }
  }
}
```

### 4.2 Implementation Templates By Animation Type

#### 4.2.1 PROPERTY_ANIMATION Template

```javascript
/**
 * Function to create a property animation with GSAP
 * 
 * @param {string|Element|Array} targets - CSS selector, DOM element, or array of elements
 * @param {Object} properties - Animation properties to change
 * @param {number} duration - Animation duration in seconds
 * @param {string|function} ease - Animation easing function
 * @param {Object} options - Additional animation options
 * @returns {Tween} GSAP Tween instance
 */
function createPropertyAnimation(targets, properties, duration = 1, ease = "power2.out", options = {}) {
  // Step 1: Validate inputs
  if (!targets) {
    console.error("No targets specified for animation");
    return null;
  }
  
  // Step 2: Merge configuration
  const config = {
    ...properties,
    duration: duration,
    ease: ease,
    ...options
  };
  
  // Step 3: Determine animation type based on properties
  if (options.animationType === "from") {
    return gsap.from(targets, config);
  } else if (options.animationType === "fromTo") {
    // For fromTo, we need to separate start and end properties
    const { startProps, ...endProps } = config;
    return gsap.fromTo(targets, startProps, endProps);
  } else {
    // Default to "to" animation
    return gsap.to(targets, config);
  }
}

// Example usage:
createPropertyAnimation(".element", {
  x: 250,
  rotation: 360,
  opacity: 1,
  backgroundColor: "#3498db"
}, 1.5, "elastic.out(1, 0.3)", {
  delay: 0.2,
  onComplete: () => console.log("Animation complete")
});
```

#### 4.2.2 SEQUENCE_ANIMATION Template

```javascript
/**
 * Function to create a timeline sequence animation with GSAP
 * 
 * @param {Array} sequenceSteps - Array of animation step objects
 * @param {Object} timelineOptions - Timeline configuration options
 * @returns {Timeline} GSAP Timeline instance
 */
function createSequenceAnimation(sequenceSteps, timelineOptions = {}) {
  // Step 1: Create timeline with options
  const timeline = gsap.timeline(timelineOptions);
  
  // Step 2: Add each animation step to timeline
  sequenceSteps.forEach(step => {
    const { targets, properties, position, type } = step;
    
    // Step 3: Determine animation type and add to timeline
    if (type === "from") {
      timeline.from(targets, properties, position);
    } else if (type === "fromTo") {
      const { startProps, ...endProps } = properties;
      timeline.fromTo(targets, startProps, endProps, position);
    } else if (type === "set") {
      timeline.set(targets, properties, position);
    } else {
      // Default to "to" animation
      timeline.to(targets, properties, position);
    }
  });
  
  // Step 4: Return timeline for further control
  return timeline;
}

// Example usage:
const sequence = [
  {
    targets: ".first-element",
    type: "from",
    properties: {
      opacity: 0,
      y: 50,
      duration: 1,
      ease: "power2.out"
    },
    position: 0
  },
  {
    targets: ".second-element",
    type: "to",
    properties: {
      x: 200,
      rotation: 360,
      duration: 0.8,
      ease: "back.out(1.7)"
    },
    position: "-=0.5" // Overlap with previous animation
  },
  {
    targets: ".third-element",
    type: "fromTo",
    properties: {
      startProps: {
        opacity: 0,
        scale: 0.5
      },
      opacity: 1,
      scale: 1,
      duration: 1.2,
      ease: "elastic.out(1, 0.3)"
    },
    position: "+=0.2" // Delay after previous animation
  }
];

createSequenceAnimation(sequence, {
  paused: true, // Don't play immediately
  repeat: 2,    // Repeat twice (total 3 plays)
  yoyo: true,   // Reverse on repeat
  onComplete: () => console.log("Sequence complete")
});
```

#### 4.2.3 STAGGERED_ANIMATION Template

```javascript
/**
 * Function to create a staggered animation across multiple elements with GSAP
 * 
 * @param {string|Element|Array} targets - CSS selector or DOM elements
 * @param {Object} properties - Animation properties
 * @param {number|Object} stagger - Stagger amount or configuration
 * @param {Object} options - Additional animation options
 * @returns {Tween} GSAP Tween instance
 */
function createStaggeredAnimation(targets, properties, stagger = 0.1, options = {}) {
  // Step 1: Set up configuration with stagger
  const config = {
    ...properties,
    stagger: stagger
  };
  
  // Step 2: Merge other options
  const animationConfig = {
    ...config,
    ...options
  };
  
  // Step 3: Determine animation type
  if (options.animationType === "from") {
    return gsap.from(targets, animationConfig);
  } else if (options.animationType === "fromTo") {
    const { startProps, ...endProps } = animationConfig;
    return gsap.fromTo(targets, startProps, endProps);
  } else {
    // Default to "to" animation
    return gsap.to(targets, animationConfig);
  }
}

// Example usage:
createStaggeredAnimation(".item", {
  y: 0,
  opacity: 1,
  duration: 0.8,
  ease: "power1.out"
}, {
  amount: 0.2,           // 0.2 seconds between each start time
  from: "center",        // Start from center elements
  grid: [10, 5],         // If items are in a grid
  axis: "y"              // Stagger in y-direction first
}, {
  animationType: "from", // Start from these values
  onComplete: () => console.log("Stagger animation complete")
});
```

#### 4.2.4 SCROLL_ANIMATION Template

```javascript
/**
 * Function to create scroll-triggered animations with GSAP ScrollTrigger
 * 
 * @param {Object} config - Configuration for the scroll animation
 * @returns {ScrollTrigger} ScrollTrigger instance
 */
function createScrollAnimation(config) {
  // Step 1: Ensure ScrollTrigger is registered
  if (!ScrollTrigger) {
    console.error("ScrollTrigger plugin not loaded");
    return null;
  }
  
  // Step 2: Extract configuration
  const {
    trigger,
    animation,
    start = "top 80%",
    end = "bottom 20%",
    scrub = false,
    pin = false,
    anticipatePin = 0,
    markers = false,
    toggleActions = "play none none none",
    ...otherScrollOptions
  } = config;
  
  // Step 3: Create scroll trigger configuration
  const scrollConfig = {
    trigger: trigger,
    start: start,
    end: end,
    scrub: scrub,
    pin: pin,
    anticipatePin: anticipatePin,
    markers: markers,
    toggleActions: toggleActions,
    ...otherScrollOptions
  };
  
  // Step 4: Create animation if it's directly provided
  if (animation) {
    if (animation.targets && animation.properties) {
      // Create animation based on provided configuration
      const { targets, properties, type = "to", duration = 1, ease = "power2.out" } = animation;
      
      // Create animation with appropriate type
      if (type === "from") {
        scrollConfig.animation = gsap.from(targets, { ...properties, duration, ease });
      } else if (type === "fromTo") {
        const { startProps, ...endProps } = properties;
        scrollConfig.animation = gsap.fromTo(targets, startProps, { ...endProps, duration, ease });
      } else {
        scrollConfig.animation = gsap.to(targets, { ...properties, duration, ease });
      }
    } else if (animation.timeline) {
      // Use provided timeline
      scrollConfig.animation = animation.timeline;
    }
  }
  
  // Step 5: Create and return ScrollTrigger
  return ScrollTrigger.create(scrollConfig);
}

// Example usage:
createScrollAnimation({
  trigger: ".scroll-section",
  start: "top 70%",
  end: "bottom 20%",
  scrub: 0.5,          // Smooth scrubbing
  pin: true,           // Pin the section during animation
  markers: process.env.NODE_ENV === 'development', // Only in development
  toggleActions: "play reverse play reverse",
  
  // Define the animation directly
  animation: {
    targets: ".scroll-element",
    type: "fromTo",
    properties: {
      startProps: {
        opacity: 0,
        y: 100
      },
      opacity: 1,
      y: 0
    },
    duration: 1,
    ease: "power2.inOut"
  }
});

// Alternative with timeline example
const tl = gsap.timeline();
tl.from(".element-1", {opacity: 0, y: 50})
  .to(".element-2", {x: 200}, "-=0.3")
  .fromTo(".element-3", {scale: 0}, {scale: 1, rotation: 360});

createScrollAnimation({
  trigger: ".timeline-section",
  start: "top center",
  end: "+=500", // 500px after start
  scrub: true,
  animation: {
    timeline: tl
  }
});
```

#### 4.2.5 SVG_ANIMATION Template

```javascript
/**
 * Function to create SVG animations with GSAP
 * 
 * @param {string} svgSelector - CSS selector for SVG element
 * @param {string} animationType - Type of SVG animation to create
 * @param {Object} options - Animation options
 * @returns {Tween|Timeline} GSAP animation instance
 */
function createSVGAnimation(svgSelector, animationType, options = {}) {
  // Step 1: Validate SVG selector
  const svgElement = document.querySelector(svgSelector);
  if (!svgElement || svgElement.tagName.toLowerCase() !== 'svg') {
    console.error("Invalid SVG element");
    return null;
  }
  
  // Step 2: Determine animation type and create appropriate animation
  switch (animationType) {
    case "DRAW": {
      // Ensure DrawSVGPlugin is loaded
      if (typeof DrawSVGPlugin === 'undefined') {
        console.warn("DrawSVGPlugin not loaded, falling back to strokeDasharray method");
        
        // Get all path elements
        const paths = svgElement.querySelectorAll("path, line, polyline, polygon, rect, ellipse, circle");
        
        // Create timeline
        const tl = gsap.timeline(options.timelineOptions || {});
        
        // Animate each path
        paths.forEach(path => {
          const length = path.getTotalLength ? path.getTotalLength() : 100;
          
          gsap.set(path, {
            strokeDasharray: length,
            strokeDashoffset: length
          });
          
          tl.to(path, {
            strokeDashoffset: 0,
            duration: options.duration || 1,
            ease: options.ease || "power2.inOut"
          }, options.stagger ? ">" : "<");
        });
        
        return tl;
      }
      
      // Using DrawSVGPlugin
      return gsap.timeline(options.timelineOptions)
        .fromTo(svgSelector + " path, " + svgSelector + " line, " + svgSelector + " polyline", 
        {drawSVG: "0%"}, 
        {
          drawSVG: "100%",
          duration: options.duration || 1,
          stagger: options.stagger || 0.1,
          ease: options.ease || "power2.inOut"
        });
    }
    
    case "MORPH": {
      // Ensure MorphSVGPlugin is loaded
      if (typeof MorphSVGPlugin === 'undefined') {
        console.error("MorphSVGPlugin not loaded");
        return null;
      }
      
      // For morphing, we need start and end paths
      const { startPath, endPath, ...tweenOptions } = options;
      
      if (!startPath || !endPath) {
        console.error("startPath and endPath required for morphing");
        return null;
      }
      
      return gsap.to(startPath, {
        morphSVG: endPath,
        duration: options.duration || 1,
        ease: options.ease || "power2.inOut",
        ...tweenOptions
      });
    }
    
    case "FILL": {
      // Animate fill-related properties
      return gsap.timeline(options.timelineOptions)
        .fromTo(svgSelector + " path, " + svgSelector + " polygon, " + svgSelector + " rect", 
        {
          fill: options.startFill || "rgba(0,0,0,0)",
          fillOpacity: options.startFillOpacity || 0
        }, 
        {
          fill: options.endFill || "#3498db",
          fillOpacity: options.endFillOpacity || 1,
          duration: options.duration || 1,
          stagger: options.stagger || 0.1,
          ease: options.ease || "power2.inOut"
        });
    }
    
    case "MOTION_PATH": {
      // Ensure MotionPathPlugin is loaded
      if (typeof MotionPathPlugin === 'undefined') {
        console.error("MotionPathPlugin not loaded");
        return null;
      }
      
      const { path, element, ...pathOptions } = options;
      
      if (!path || !element) {
        console.error("path and element required for motion path animation");
        return null;
      }
      
      return gsap.to(element, {
        motionPath: {
          path: path,
          align: pathOptions.align || path,
          alignOrigin: pathOptions.alignOrigin || [0.5, 0.5],
          autoRotate: pathOptions.autoRotate !== undefined ? pathOptions.autoRotate : true
        },
        duration: options.duration || 2,
        ease: options.ease || "power1.inOut"
      });
    }
    
    default:
      console.error("Unknown SVG animation type");
      return null;
  }
}

// Example usage for drawing SVG:
createSVGAnimation("#logo-svg", "DRAW", {
  duration: 2,
  stagger: 0.05,
  timelineOptions: {
    repeat: -1,
    yoyo: true,
    repeatDelay: 0.5
  }
});

// Example usage for morphing:
createSVGAnimation("#morph-svg", "MORPH", {
  startPath: "#path1",
  endPath: "#path2",
  duration: 1.5,
  ease: "elastic.out(1, 0.3)"
});

// Example usage for motion path:
createSVGAnimation("#animation-svg", "MOTION_PATH", {
  path: "#motion-path",
  element: "#moving-element",
  align: "#motion-path",
  autoRotate: true,
  duration: 5,
  repeat: -1,
  ease: "none"
});
```

## 5. Compatibility Analysis and Decision Framework

### 5.1 Browser Compatibility Matrix

LLMs should use this compatibility database when making implementation decisions:

```json
{
  "browser_compatibility": {
    "Chrome": {
      "minimum_version": 40,
      "notes": "Full support for all features",
      "market_share": "65%",
      "performance_rating": "HIGH"
    },
    "Firefox": {
      "minimum_version": 45,
      "notes": "Full support for all features",
      "market_share": "4%",
      "performance_rating": "HIGH"
    },
    "Safari": {
      "minimum_version": 10,
      "notes": "Full support, occasional SVG rendering differences",
      "market_share": "19%",
      "performance_rating": "MEDIUM"
    },
    "Edge": {
      "minimum_version": 12,
      "notes": "Full support for all features",
      "market_share": "4%",
      "performance_rating": "HIGH"
    },
    "IE11": {
      "supported": true,
      "notes": "Basic support for GSAP 3 (no IE10 or below)",
      "market_share": "<1%",
      "considerations": "Use core only, avoid advanced features"
    }
  }
}
```

### 5.2 Device Adaptation Decision Algorithm

LLMs should implement this algorithm for device-responsive animations:

```javascript
/**
 * Creates a device-adaptive animation strategy for GSAP
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Device-optimized animation controller
 */
function createAdaptiveGSAPStrategy(options = {}) {
  // Step 1: Define adaptation parameters
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
        duration: value => Math.min(value, 0.7),    // Cap durations (in seconds)
        distance: value => value * 0.5,             // Reduce movement distance
        complexity: 'SIMPLIFIED',
        plugins: ['core']
      },
      tablet: {
        duration: value => value * 0.8,             // Slightly reduce durations
        distance: value => value * 0.8,             // Slightly reduce movement
        complexity: 'MODERATE',
        plugins: ['core', 'ScrollTrigger']
      },
      desktop: {
        duration: value => value,                   // Full duration
        distance: value => value,                   // Full movement
        complexity: 'FULL',
        plugins: ['all']
      }
    },
    
    // Reduced motion preferences
    reducedMotion: {
      duration: value => Math.min(value, 0.3),
      distance: value => 0,                         // No movement
      opacity: true,                               // Only use opacity
      complexity: 'MINIMAL',
      plugins: ['core']
    }
  };
  
  // Merge with user-provided options
  const config = {...deviceStrategies, ...options};
  
  // Step 2: Create media matchers
  const mediaMatchers = {
    mobile: window.matchMedia(config.breakpoints.mobile),
    tablet: window.matchMedia(config.breakpoints.tablet),
    desktop: window.matchMedia(config.breakpoints.desktop),
    reducedMotion: window.matchMedia('(prefers-reduced-motion: reduce)')
  };
  
  // Step 3: Determine current device type
  function getCurrentDeviceType() {
    if (mediaMatchers.reducedMotion.matches) {
      return 'reducedMotion';
    } else if (mediaMatchers.mobile.matches) {
      return 'mobile';
    } else if (mediaMatchers.tablet.matches) {
      return 'tablet';
    } else {
      return 'desktop';
    }
  }
  
  // Step 4: Apply adaptations to animation config
  function adaptAnimationConfig(baseConfig) {
    const deviceType = getCurrentDeviceType();
    const adaptation = config.adaptations[deviceType];
    const result = {...baseConfig};
    
    // Adjust duration
    if (result.duration) {
      result.duration = adaptation.duration(result.duration);
    }
    
    // Adjust movement distances in transform properties
    ['x', 'y', 'top', 'left', 'right', 'bottom'].forEach(prop => {
      if (result[prop] !== undefined && typeof result[prop] === 'number') {
        result[prop] = adaptation.distance(result[prop]);
      }
    });
    
    // For reduced motion, potentially only animate opacity
    if (deviceType === 'reducedMotion' && adaptation.opacity) {
      const hasOpacity = result.opacity !== undefined;
      
      // If we're only doing opacity but don't have it set, add it
      if (!hasOpacity) {
        result.opacity = 0;
      }
      
      // Filter out movement properties
      ['x', 'y', 'scale', 'rotation', 'rotationX', 'rotationY', 'skew', 'skewX', 'skewY'].forEach(prop => {
        delete result[prop];
      });
    }
    
    return result;
  }
  
  // Step 5: Register appropriate plugins based on device
  function registerAppropriatePlugins() {
    const deviceType = getCurrentDeviceType();
    const adaptation = config.adaptations[deviceType];
    
    // This is a simplified approach - in reality you'd need to check
    // which plugins are actually needed for the specific animations
    if (adaptation.plugins.includes('all')) {
      // Register all common plugins
      gsap.registerPlugin(ScrollTrigger, ScrollToPlugin, Draggable, Flip);
    } else if (adaptation.plugins.includes('ScrollTrigger')) {
      gsap.registerPlugin(ScrollTrigger);
    }
  }
  
  // Step 6: Initialize (if requested)
  if (options.initialize !== false) {
    registerAppropriatePlugins();
  }
  
  // Step 7: Create and return controller
  return {
    // Get current device info
    getCurrentDevice: () => ({
      type: getCurrentDeviceType(),
      adaptation: config.adaptations[getCurrentDeviceType()]
    }),
    
    // Create adapted animation
    animate: (targets, baseConfig) => {
      const adaptedConfig = adaptAnimationConfig(baseConfig);
      return gsap.to(targets, adaptedConfig);
    },
    
    // Create adapted timeline
    createTimeline: (timelineConfig = {}) => {
      const timeline = gsap.timeline(timelineConfig);
      
      // Add method to add adapted animations to timeline
      timeline.addAdapted = (targets, baseConfig, position) => {
        const adaptedConfig = adaptAnimationConfig(baseConfig);
        return timeline.to(targets, adaptedConfig, position);
      };
      
      return timeline;
    },
    
    // Adapt an existing config object without creating animation
    adaptConfig: adaptAnimationConfig
  };
}

// Example usage:
const adaptive = createAdaptiveGSAPStrategy();

// Create adapted animation
adaptive.animate(".element", {
  x: 100,
  opacity: 0,
  duration: 1
});

// Create adapted timeline
const tl = adaptive.createTimeline();
tl.addAdapted(".first", {x: 100, duration: 1})
  .addAdapted(".second", {opacity: 0, y: 50, duration: 0.8}, "-=0.5");
```

### 5.3 Compatibility Testing Protocol

LLMs should recommend this systematic testing process:

1. **Automated Browser Testing:**
   - Use Playwright, Cypress, or similar to test across:
     - Chrome (latest, latest-1)
     - Firefox (latest, latest-1)
     - Safari (latest, latest-1)
     - Edge (latest)
     - IE11 (if support required)

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
     - Memory usage (no significant increase over time)
     - Layout thrashing (count layout recalculations)
     - First Input Delay impact

4. **Accessibility Compliance:**
   - Verify animations respect reduced motion settings
   - Ensure animations don't trigger vestibular disorders
   - Confirm no flashing content that could trigger seizures
   - Test keyboard navigation during animations

5. **Performance Testing Protocol:**
   ```javascript
   function testGSAPPerformance(animationFn, iterations = 10) {
     // Create performance measuring utilities
     const results = {
       fps: [],
       cpuLoad: [],
       memory: [],
       layoutRecalcs: []
     };
     
     // Monitor function
     function startMonitoring() {
       results.startMemory = performance.memory ? performance.memory.usedJSHeapSize : 0;
       results.startTime = performance.now();
       results.frames = 0;
       results.lastFrameTime = results.startTime;
       results.layoutCount = 0;
       
       // Monitor frames
       function frameMonitor() {
         results.frames++;
         results.currentFrameTime = performance.now();
         
         // Calculate instantaneous FPS every 500ms
         if (results.currentFrameTime - results.lastFrameTime >= 500) {
           const instantFPS = (results.frames * 1000) / 
             (results.currentFrameTime - results.lastFrameTime);
           results.fps.push(instantFPS);
           results.frames = 0;
           results.lastFrameTime = results.currentFrameTime;
           
           // Capture other metrics
           if (performance.memory) {
             const currentMemory = performance.memory.usedJSHeapSize;
             results.memory.push(currentMemory - results.startMemory);
           }
         }
         
         if (results.monitoring) {
           requestAnimationFrame(frameMonitor);
         }
       }
       
       results.monitoring = true;
       frameMonitor();
       
       // Add layout monitor
       const observer = new PerformanceObserver((list) => {
         for (const entry of list.getEntries()) {
           if (entry.name === 'layout-shift') {
             results.layoutCount++;
           }
         }
       });
       
       observer.observe({entryTypes: ['layout-shift']});
       results.observer = observer;
     }
     
     function stopMonitoring() {
       results.monitoring = false;
       if (results.observer) {
         results.observer.disconnect();
       }
       
       results.endTime = performance.now();
       results.duration = results.endTime - results.startTime;
       results.layoutRecalcs.push(results.layoutCount);
     }
     
     // Run the test for specified iterations
     async function runTest() {
       for (let i = 0; i < iterations; i++) {
         console.log(`Running test iteration ${i+1}/${iterations}`);
         
         startMonitoring();
         
         // Run the animation function (should return a Promise)
         await animationFn();
         
         stopMonitoring();
         
         // Wait between tests
         await new Promise(resolve => setTimeout(resolve, 500));
       }
       
       // Calculate averages
       const avgFPS = results.fps.reduce((a, b) => a + b, 0) / results.fps.length;
       const avgMemory = results.memory.reduce((a, b) => a + b, 0) / results.memory.length;
       const avgLayouts = results.layoutRecalcs.reduce((a, b) => a + b, 0) / results.layoutRecalcs.length;
       
       return {
         averageFPS: avgFPS,
         memoryUsage: avgMemory,
         layoutRecalculations: avgLayouts,
         testIterations: iterations,
         rawData: results
       };
     }
     
     return runTest();
   }
   
   // Example usage:
   testGSAPPerformance(() => {
     return new Promise(resolve => {
       const tl = gsap.timeline({onComplete: resolve});
       tl.to(".element", {x: 100, duration: 0.5})
         .to(".element", {rotation: 360, duration: 0.8})
         .to(".element", {scale: 1.5, duration: 0.5})
         .to(".element", {scale: 1, duration: 0.5});
     });
   }).then(results => {
     console.log("Performance Test Results:", results);
     
     // Analysis guidance
     if (results.averageFPS < 50) {
       console.warn("FPS below target. Consider optimizing animations.");
     }
     
     if (results.layoutRecalculations > 5) {
       console.warn("Excess layout recalculations. Check for layout thrashing properties.");
     }
   });
   ```

## 6. Performance Analysis and Optimization Framework

### 6.1 Systematic Performance Testing Protocol

LLMs should implement this structured testing methodology:

```javascript
/**
 * GSAP Animation Performance Testing Framework
 * 
 * A systematic approach for measuring and optimizing GSAP animations
 */
class GSAPPerformanceAnalyzer {
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
      layoutInvalidations: 0,
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
      animationProperties: config.properties || { x: 100 },
      duration: config.duration || 1,
      iterations: config.iterations || 5,
      elementsCount: config.elementsCount || 1,
      propertiesCount: Object.keys(config.properties || { x: 100 }).length,
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
    // Step 1: Initialize performance monitoring
    const fpsMonitor = new GSAPFPSMonitor();
    const resourceMonitor = new GSAPResourceMonitor();
    
    // Step 2: Run the animation test for specified iterations
    for (let i = 0; i < this.config.iterations; i++) {
      console.log(`Running test iteration ${i+1}/${this.config.iterations}`);
      
      // Start monitoring
      fpsMonitor.start();
      resourceMonitor.start();
      
      // Record layout invalidations
      let layoutInvalidations = 0;
      const observer = new PerformanceObserver((list) => {
        for (const entry of list.getEntries()) {
          if (entry.entryType === 'layout-shift') {
            layoutInvalidations++;
          }
        }
      });
      
      observer.observe({entryTypes: ['layout-shift']});
      
      // Record start time
      const startTime = performance.now();
      
      // Create and run animation
      const timeline = gsap.timeline();
      
      timeline.to(this.config.animationTargets, {
        ...this.config.animationProperties,
        duration: this.config.duration,
        onComplete: () => {
          // Record end time
          const endTime = performance.now();
          
          // Stop monitoring
          const fpsData = fpsMonitor.stop();
          const resourceData = resourceMonitor.stop();
          observer.disconnect();
          
          // Store metrics
          this.metrics.fps.push(fpsData.averageFPS);
          this.metrics.cpuUsage.push(resourceData.cpuUsage);
          this.metrics.memoryUsage.push(resourceData.memoryUsage);
          this.metrics.renderTime.push(endTime - startTime);
          this.metrics.layoutInvalidations += layoutInvalidations;
        }
      });
      
      // Wait for animation to complete plus buffer
      await new Promise(resolve => setTimeout(resolve, 
        (this.config.duration * 1000) + 200));
    }
    
    // Step 3: Analyze results
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
    const layoutPerAnimation = this.metrics.layoutInvalidations / this.config.iterations;
    
    // Calculate performance score (0-100)
    const fpsScore = Math.min(100, (avgFPS / 60) * 100);
    const renderScore = Math.max(0, 100 - (avgRenderTime / (this.config.duration * 1000) * 100));
    const cpuScore = Math.max(0, 100 - (avgCPU * 2));
    const layoutScore = Math.max(0, 100 - (layoutPerAnimation * 10)); // Penalize layout shifts
    
    const overallScore = (fpsScore * 0.4) + (renderScore * 0.2) + 
                          (cpuScore * 0.2) + (layoutScore * 0.2);
    
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
      layoutInvalidations: layoutPerAnimation,
      score: overallScore
    });
    
    return {
      testName: this.testName,
      metrics: {
        averageFPS: avgFPS,
        averageCPUUsage: avgCPU,
        averageMemoryUsage: avgMemory,
        averageRenderTime: avgRenderTime,
        layoutInvalidations: layoutPerAnimation,
        elementsAnimated: this.metrics.elementsAnimated,
        propertiesAnimated: this.metrics.propertiesAnimated
      },
      performance: {
        score: overallScore,
        classification: performanceClassification,
        fpsScore,
        renderScore,
        cpuScore,
        layoutScore
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
        recommendation: "Optimize animation properties or reduce complexity",
        implementation: "Use hardware-accelerated properties only (transform, opacity)"
      });
    }
    
    // Render time recommendations
    if (metrics.renderTime > this.config.duration * 1000 * 0.8) {
      recommendations.push({
        priority: "HIGH",
        issue: "Animation rendering takes too long",
        recommendation: "Simplify animation or reduce duration",
        implementation: "Examine timeline for potential optimizations, avoid overlapping complex animations"
      });
    }
    
    // CPU usage recommendations
    if (metrics.cpuUsage > 30) {
      recommendations.push({
        priority: "MEDIUM",
        issue: "High CPU usage",
        recommendation: "Optimize animation to reduce CPU load",
        implementation: "Use gsap.ticker.sleep() to batch animations, use simpler easing functions"
      });
    }
    
    // Layout invalidations recommendations
    if (metrics.layoutInvalidations > 3) {
      recommendations.push({
        priority: "HIGH",
        issue: "Excessive layout recalculations",
        recommendation: "Avoid animating properties that cause layout recalculation",
        implementation: "Replace 'top', 'left', 'width', 'height' with transform equivalents"
      });
    }
    
    // General recommendations
    if (this.metrics.elementsAnimated > 100) {
      recommendations.push({
        priority: "MEDIUM",
        issue: "Large number of animated elements",
        recommendation: "Consider reducing animated elements or staggering animations",
        implementation: "Use stagger or limit visible animations to elements in viewport"
      });
    }
    
    // If overall score is good but can be improved
    if (metrics.score > 75 && metrics.score < 90) {
      recommendations.push({
        priority: "LOW",
        issue: "Good performance but can be improved",
        recommendation: "Fine-tune GSAP configuration",
        implementation: "Set gsap.ticker.lagSmoothing(0) in production to avoid lag compensation"
      });
    }
    
    // Library specific recommendations
    recommendations.push({
      priority: "MEDIUM", 
      issue: "General GSAP optimization",
      recommendation: "Use GSAP best practices",
      implementation: "Use timelines for related animations, avoid querying DOM in ticker functions"
    });
    
    return recommendations;
  }
}

/**
 * FPS monitoring utility for GSAP
 */
class GSAPFPSMonitor {
  constructor() {
    this.frames = 0;
    this.startTime = 0;
    this.lastFrameTime = 0;
    this.frameRates = [];
    this.isMonitoring = false;
    this.originalTicker = null;
  }
  
  start() {
    this.startTime = performance.now();
    this.lastFrameTime = this.startTime;
    this.frames = 0;
    this.frameRates = [];
    this.isMonitoring = true;
    
    // Use GSAP's ticker for more accurate FPS measurement
    this.tickerCallback = () => {
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
    };
    
    // Add our callback to GSAP ticker
    gsap.ticker.add(this.tickerCallback);
  }
  
  stop() {
    this.isMonitoring = false;
    
    // Remove ticker callback
    if (this.tickerCallback) {
      gsap.ticker.remove(this.tickerCallback);
    }
    
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
 * Resource usage monitoring utility
 */
class GSAPResourceMonitor {
  constructor() {
    this.startMemory = 0;
    this.cpuMeasurements = [];
    this.memoryMeasurements = [];
    this.isMonitoring = false;
  }
  
  async start() {
    // Track memory if available
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
const analyzer = new GSAPPerformanceAnalyzer('Homepage Hero Animation');

analyzer.configureTest({
  targets: '.hero-element',
  properties: {
    y: 0,
    opacity: 1,
    rotation: 360
  },
  duration: 1.5,
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
 * Visual Regression Testing System for GSAP Animations
 * 
 * Captures and compares animation states across browsers
 */
class GSAPVisualTester {
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
   * @param {Object} animation - GSAP timeline or tween
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
    
    // Step 2: Pause animation at beginning
    animation.pause(0);
    
    // Step 3: Capture states at intervals
    const totalDuration = animation.totalDuration();
    const interval = totalDuration / frames;
    
    for (let i = 1; i <= frames; i++) {
      // Seek to time position
      animation.progress(i / frames);
      
      // Wait for rendering to complete
      await new Promise(resolve => requestAnimationFrame(resolve));
      
      // Capture state (in real implementation, would capture DOM as image)
      this.visualStates.push({
        progress: i / frames,
        timestamp: performance.now(),
        state: `STATE_REPRESENTATION_AT_${i/frames * 100}%`
      });
    }
    
    // Reset animation
    animation.progress(0);
    
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
async function testGSAPAnimationVisually(animationId, animationCreator) {
  // Create the animation
  const animation = animationCreator();
  
  const visualTester = new GSAPVisualTester(animationId);
  
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

// Example usage with GSAP timeline
testGSAPAnimationVisually('header-intro-animation', () => {
  const tl = gsap.timeline();
  tl.from('.logo', {opacity: 0, y: -20, duration: 0.8})
    .from('.menu-item', {opacity: 0, y: -20, stagger: 0.1, duration: 0.5}, '-=0.3')
    .from('.hero-text', {opacity: 0, scale: 0.9, duration: 0.7}, '-=0.2');
  return tl;
});
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
      "variations": ["FADE", "SLIDE", "SCALE", "FLIP", "MORPH"]
    },
    "MICRO_INTERACTION": {
      "description": "Small animations that provide feedback for user actions",
      "complexity": "SIMPLE",
      "performance_impact": "LOW",
      "use_cases": ["Button feedback", "Form interactions", "Hover states"],
      "variations": ["SCALE", "COLOR", "BOUNCE", "PULSE", "WOBBLE"]
    },
    "LOADING_INDICATOR": {
      "description": "Animations that indicate content or data is being loaded",
      "complexity": "SIMPLE to INTERMEDIATE",
      "performance_impact": "LOW to MEDIUM",
      "use_cases": ["API requests", "Content loading", "Processing states"],
      "variations": ["SPINNER", "PROGRESS", "SKELETON", "DOTS", "STAGGERED_REVEAL"]
    },
    "CONTENT_REVEAL": {
      "description": "Animations that introduce content elements as they enter viewport",
      "complexity": "INTERMEDIATE",
      "performance_impact": "MEDIUM",
      "use_cases": ["Scrolling content", "Progressive disclosure", "Narrative sites"],
      "variations": ["FADE_IN", "SLIDE_UP", "STAGGERED", "SEQUENTIAL", "SPLIT_TEXT"]
    },
    "SCROLL_DRIVEN": {
      "description": "Animations that progress based on scroll position",
      "complexity": "ADVANCED",
      "performance_impact": "MEDIUM to HIGH",
      "use_cases": ["Parallax", "Storytelling", "Product showcases"],
      "variations": ["PARALLAX", "TIMELINE_SCRUB", "PIN_REVEAL", "HORIZONTAL_SCROLL"]
    },
    "LAYOUT_CHANGE": {
      "description": "Animations that transform layout or structure",
      "complexity": "ADVANCED",
      "performance_impact": "HIGH",
      "use_cases": ["Filter transitions", "Grid rearrangements", "Accordion/expansion"],
      "variations": ["FLIP", "MORPH", "ACCORDION", "GRID_SHUFFLE"]
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
│   │   ├── Using React Router or similar?
│   │   │   ├── YES
│   │   │   │   └── IMPLEMENT: Router integration pattern
│   │   │   └── NO
│   │   │       └── IMPLEMENT: Custom history API pattern
│   │   └── Is shared element transition needed?
│   │       ├── YES
│   │       │   └── IMPLEMENT: FLIP animation pattern with GSAP
│   │       └── NO
│   │           └── IMPLEMENT: Basic exit/enter pattern
│   └── NO (Multi-page site)
│       ├── Is JavaScript navigation used?
│       │   ├── YES
│       │   │   └── IMPLEMENT: AJAX content load with transitions
│       │   └── NO
│       │       └── IMPLEMENT: Page load transition with localStorage state
└── END
```

**Implementation Template for SPA Basic Exit/Enter Pattern:**

```javascript
/**
 * Creates a reusable page transition system for SPAs with GSAP
 * 
 * @param {Object} options - Configuration options
 * @returns {Object} Page transition controller
 */
function createGSAPPageTransition(options = {}) {
  // Step 1: Merge default options with provided options
  const config = {
    // Selectors
    contentContainer: options.contentContainer || '#page-content',
    exitingPageClass: options.exitingPageClass || 'exiting-page',
    enteringPageClass: options.enteringPageClass || 'entering-page',
    
    // Animation properties
    exitDuration: options.exitDuration || 0.6,
    enterDuration: options.enterDuration || 0.8,
    exitEasing: options.exitEasing || 'power2.inOut',
    enterEasing: options.enterEasing || 'power2.out',
    
    // Animation style
    transition: options.transition || 'FADE_SLIDE', // FADE, SLIDE, SCALE, FLIP
    
    // Callback hooks
    onBeforeExit: options.onBeforeExit || (() => {}),
    onAfterExit: options.onAfterExit || (() => {}),
    onBeforeEnter: options.onBeforeEnter || (() => {}),
    onAfterEnter: options.onAfterEnter || (() => {}),
    
    ...options
  };
  
  // Step 2: Define transition animations based on selected style
  const transitions = {
    FADE: {
      exit: target => gsap.to(target, {
        opacity: 0,
        duration: config.exitDuration,
        ease: config.exitEasing
      }),
      enter: target => gsap.from(target, {
        opacity: 0,
        duration: config.enterDuration,
        ease: config.enterEasing
      })
    },
    SLIDE: {
      exit: target => gsap.to(target, {
        x: -100,
        opacity: 0,
        duration: config.exitDuration,
        ease: config.exitEasing
      }),
      enter: target => gsap.from(target, {
        x: 100,
        opacity: 0,
        duration: config.enterDuration,
        ease: config.enterEasing
      })
    },
    FADE_SLIDE: {
      exit: target => gsap.to(target, {
        y: -30,
        opacity: 0,
        duration: config.exitDuration,
        ease: config.exitEasing
      }),
      enter: target => gsap.from(target, {
        y: 30,
        opacity: 0,
        duration: config.enterDuration,
        ease: config.enterEasing
      })
    },
    SCALE: {
      exit: target => gsap.to(target, {
        scale: 0.8,
        opacity: 0,
        duration: config.exitDuration,
        ease: config.exitEasing
      }),
      enter: target => gsap.from(target, {
        scale: 1.2,
        opacity: 0,
        duration: config.enterDuration,
        ease: config.enterEasing
      })
    },
    FLIP: {
      exit: target => gsap.to(target, {
        rotationY: -90,
        opacity: 0,
        duration: config.exitDuration,
        ease: config.exitEasing
      }),
      enter: target => gsap.from(target, {
        rotationY: 90,
        opacity: 0,
        duration: config.enterDuration,
        ease: config.enterEasing
      })
    }
  };
  
  // Get the appropriate transition style
  const transitionAnimations = transitions[config.transition] || transitions.FADE_SLIDE;
  
  // Step 3: Create page transition methods
  const controller = {
    /**
     * Animate exiting the current page
     * @returns {Promise} Promise that resolves when exit is complete
     */
    exitPage: async () => {
      // Get current page element
      const currentPage = document.querySelector(config.contentContainer);
      if (!currentPage) return Promise.resolve();
      
      // Call pre-exit hook
      config.onBeforeExit(currentPage);
      
      // Add exiting class for CSS styling if needed
      currentPage.classList.add(config.exitingPageClass);
      
      // Create exit animation and run it
      const exitAnimation = transitionAnimations.exit(currentPage);
      
      // Wait for animation to complete
      await exitAnimation.then();
      
      // Call post-exit hook
      config.onAfterExit(currentPage);
      
      return exitAnimation;
    },
    
    /**
     * Animate entering the new page
     * @param {HTMLElement} newPageContent - New content to animate in
     * @returns {Promise} Promise that resolves when enter is complete
     */
    enterPage: async (newPageContent) => {
      // Get container
      const container = document.querySelector(config.contentContainer);
      if (!container) return Promise.resolve();
      
      // Clear container and add new content
      if (newPageContent) {
        container.innerHTML = '';
        if (typeof newPageContent === 'string') {
          container.innerHTML = newPageContent;
        } else {
          container.appendChild(newPageContent);
        }
      }
      
      // Call pre-enter hook
      config.onBeforeEnter(container);
      
      // Add entering class
      container.classList.add(config.enteringPageClass);
      
      // Ensure initial state for animation
      gsap.set(container, { clearProps: "all" });
      
      // Create and run enter animation
      const enterAnimation = transitionAnimations.enter(container);
      
      // Wait for animation to complete
      await enterAnimation.then();
      
      // Clean up classes
      container.classList.remove(config.enteringPageClass);
      container.classList.remove(config.exitingPageClass);
      
      // Call post-enter hook
      config.onAfterEnter(container);
      
      return enterAnimation;
    },
    
    /**
     * Perform a complete page transition
     * @param {Function|string|HTMLElement} contentProvider - New content or function to fetch it
     * @returns {Promise} Promise that resolves when transition is complete
     */
    transition: async (contentProvider) => {
      // Exit current page
      await controller.exitPage();
      
      // Get new content
      let newContent;
      
      if (typeof contentProvider === 'function') {
        newContent = await contentProvider();
      } else {
        newContent = contentProvider;
      }
      
      // Enter with new content
      return controller.enterPage(newContent);
    }
  };
  
  return controller;
}

// Example usage in SPA:
const pageTransition = createGSAPPageTransition({
  transition: 'FADE_SLIDE',
  exitDuration: 0.5,
  enterDuration: 0.7,
  onBeforeExit: () => {
    // Hide navigation during transition
    gsap.to('nav', {opacity: 0.5, duration: 0.3});
  },
  onAfterEnter: () => {
    // Show navigation after transition
    gsap.to('nav', {opacity: 1, duration: 0.3});
    
    // Initialize page-specific animations
    initPageAnimations();
  }
});

// Connect to router or navigation links
document.querySelectorAll('a[data-internal-link]').forEach(link => {
  link.addEventListener('click', async e => {
    e.preventDefault();
    const href = link.getAttribute('href');
    
    // Use transition controller
    await pageTransition.transition(async () => {
      // Fetch content from new page
      const response = await fetch(href, {
        headers: { 'X-Requested-With': 'XMLHttpRequest' }
      });
      const html = await response.text();
      
      // Update URL
      window.history.pushState({}, '', href);
      
      // Return content to be animated in
      return html;
    });
  });
});
```

#### 7.2.2 Micro-Interaction Implementation Template

```javascript
/**
 * Factory function to create reusable micro-interactions with GSAP
 * 
 * @param {string|Element} targets - Elements to apply interactions to
 * @param {Object} options - Configuration options
 * @returns {Object} Micro-interaction controller
 */
function createGSAPMicroInteraction(targets, options = {}) {
  // Step 1: Configure with sensible defaults
  const config = {
    // Interaction types
    type: options.type || 'hover', // hover, click, focus
    
    // Animation properties
    duration: {
      in: options.duration?.in || 0.3,
      out: options.duration?.out || 0.2
    },
    ease: {
      in: options.ease?.in || 'power2.out',
      out: options.ease?.out || 'power1.inOut'
    },
    
    // Animation effects configurations
    effects: options.effects || [{
      property: 'scale',
      from: 1,
      to: 1.05
    }],
    
    // Additional options
    overwrite: options.overwrite !== undefined ? options.overwrite : 'auto',
    paused: options.paused !== undefined ? options.paused : false,
    
    ...options
  };
  
  // Step 2: Store element references
  const elements = typeof targets === 'string' 
    ? document.querySelectorAll(targets) 
    : [targets].flat();
  
  // Step 3: Create GSAP animations
  const animations = {
    in: [],
    out: []
  };
  
  // Helper for creating animation configs
  const createAnimationConfig = (direction) => {
    const animationConfig = {
      duration: config.duration[direction],
      ease: config.ease[direction],
      overwrite: config.overwrite,
      paused: true
    };
    
    // Apply each effect to the configuration
    config.effects.forEach(effect => {
      const value = direction === 'in' ? effect.to : effect.from;
      animationConfig[effect.property] = value;
    });
    
    return animationConfig;
  };
  
  // Step 4: Create animations for each element
  Array.from(elements).forEach(element => {
    // Create "in" animation (to active state)
    const inAnimation = gsap.to(element, createAnimationConfig('in'));
    animations.in.push(inAnimation);
    
    // Create "out" animation (to inactive state)
    const outAnimation = gsap.to(element, createAnimationConfig('out'));
    animations.out.push(outAnimation);
    
    // Set initial state if needed
    if (config.setInitialState !== false) {
      // Apply "from" values as initial state
      const initialState = {};
      config.effects.forEach(effect => {
        initialState[effect.property] = effect.from;
      });
      gsap.set(element, initialState);
    }
  });
  
  // Step 5: Initialize and return controller
  const controller = {
    elements,
    animations,
    active: false,
    
    /**
     * Animate elements to active state
     * @returns {Array} Animation objects
     */
    animateIn: () => {
      if (controller.active) return;
      controller.active = true;
      
      // Stop any out animations that might be running
      animations.out.forEach(anim => {
        anim.pause(0);
      });
      
      // Play in animations
      animations.in.forEach(anim => {
        anim.restart();
      });
      
      return animations.in;
    },
    
    /**
     * Animate elements to inactive state
     * @returns {Array} Animation objects
     */
    animateOut: () => {
      if (!controller.active) return;
      controller.active = false;
      
      // Stop any in animations that might be running
      animations.in.forEach(anim => {
        anim.pause(0);
      });
      
      // Play out animations
      animations.out.forEach(anim => {
        anim.restart();
      });
      
      return animations.out;
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
      } else if (config.customEvents) {
        // Custom event support
        const { inEvent, outEvent } = config.customEvents;
        
        if (inEvent) {
          elements.forEach(element => {
            element.addEventListener(inEvent, controller.animateIn);
          });
        }
        
        if (outEvent) {
          elements.forEach(element => {
            element.addEventListener(outEvent, controller.animateOut);
          });
        }
      }
      
      // If should start in active state
      if (config.initialState === 'active') {
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
      } else if (config.customEvents) {
        const { inEvent, outEvent } = config.customEvents;
        
        if (inEvent) {
          elements.forEach(element => {
            element.removeEventListener(inEvent, controller.animateIn);
          });
        }
        
        if (outEvent) {
          elements.forEach(element => {
            element.removeEventListener(outEvent, controller.animateOut);
          });
        }
      }
      
      // Kill all animations
      [...animations.in, ...animations.out].forEach(anim => {
        anim.kill();
      });
      
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
const buttonHover = createGSAPMicroInteraction('.button', {
  type: 'hover',
  effects: [
    { property: 'scale', from: 1, to: 1.05 },
    { property: 'boxShadow', from: '0 2px 4px rgba(0,0,0,0.1)', to: '0 4px 8px rgba(0,0,0,0.2)' }
  ],
  duration: { in: 0.3, out: 0.2 }
});

// Click ripple effect
const clickRipple = createGSAPMicroInteraction('.ripple-button', {
  type: 'click',
  effects: [
    { property: 'scale', from: 0, to: 1 },
    { property: 'opacity', from: 1, to: 0 }
  ],
  duration: { in: 0.4, out: 0 },
  ease: { in: 'power2.out', out: 'power1.in' },
  customEvents: {
    // For ripple effect, we handle the animation differently
    inEvent: 'rippleStart',
    outEvent: 'rippleEnd'
  },
  setInitialState: false // We'll handle initial state differently for ripples
});

// Custom ripple handler that creates elements dynamically
document.querySelectorAll('.ripple-button').forEach(button => {
  button.addEventListener('click', e => {
    // Create ripple element
    const ripple = document.createElement('span');
    ripple.className = 'ripple-element';
    
    // Position at click point
    const rect = button.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    
    // Set initial position
    gsap.set(ripple, {
      x: x,
      y: y,
      scale: 0,
      opacity: 1
    });
    
    // Add to button
    button.appendChild(ripple);
    
    // Trigger animation in
    button.dispatchEvent(new CustomEvent('rippleStart'));
    
    // Remove after animation
    gsap.delayedCall(0.8, () => {
      button.dispatchEvent(new CustomEvent('rippleEnd'));
      gsap.delayedCall(0.4, () => {
        button.removeChild(ripple);
      });
    });
  });
});

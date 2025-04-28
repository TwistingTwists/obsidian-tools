# Browser Internals Knowledge Assessment

I'll create a series of increasingly difficult questions about how browsers work, with follow-ups to test deeper understanding. Let me start by outlining the topics we'll cover:

## Topic Index

1. **Browser Components and Architecture**
    
    - Core components (UI, browser engine, rendering engine, networking, JS engine, etc.)
    - Browser process model (single vs multi-process architecture)
    - Threads vs processes in browser context
2. **Networking and Resource Loading**
    
    - DNS resolution
    - TCP/IP connections
    - HTTP/HTTPS protocols
    - Resource prioritization
    - Network optimizations (preloading, prefetching)
3. **Parsing and DOM Construction**
    
    - HTML parsing
    - CSS parsing
    - DOM tree construction
    - CSSOM construction
    - Error handling and recovery
4. **Rendering Pipeline**
    
    - Render tree construction
    - Layout/reflow
    - Paint
    - Compositing
    - GPU acceleration
5. **JavaScript Execution**
    
    - JS engines (V8, SpiderMonkey, etc.)
    - JIT compilation
    - Event loop and task queues
    - Web APIs
    - Memory management and garbage collection
6. **Browser Security**
    
    - Same-origin policy
    - Content Security Policy
    - CORS
    - Sandboxing
    - Browser exploits and mitigations
7. **Performance Optimization**
    
    - Critical rendering path
    - Progressive rendering
    - Reflows and repaints
    - Memory and CPU profiling
    - Web performance APIs
8. **Modern Browser Features**
    
    - Service Workers
    - Web Assembly
    - Progressive Web Apps
    - WebRTC
    - Browser storage mechanisms
9. **Browser Differences and Standards**
    
    - Rendering engine differences
    - Standards compliance
    - Vendor prefixes
    - Experimental features
10. **Browser Extensions and DevTools**
    
    - Extension architecture
    - DevTools internals
    - Debugging capabilities
    - Performance analysis tools

Let's begin with the questions, starting with fundamentals and progressively increasing in difficulty.

## Question 1: Browser Components

What are the main components of a modern web browser and what is the role of each?
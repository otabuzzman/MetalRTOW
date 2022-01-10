# MetalRT
Ray tracing with Metal. The lab aims to get Peter Shirley's ray tracer from his mini-book [Ray Tracing in one weekend](https://github.com/RayTracing/raytracing.github.io/) (RTOW) with Metal up and running.

Due to the lack of Xcode capable equipment development happened on an iPad Pro using Swift Playgrounds 4 and to a limited extent with [Swift on Windows](https://www.swift.org/blog/swift-on-windows/).

### Tools
Apps used on iPad
- [Swift Playgrounds 4](https://apps.apple.com/de/app/swift-playgrounds/id908519492)
- [Working Copy](https://workingcopyapp.com/)
- [Textastic](https://www.textasticapp.com/)
- [GitHub](https://apps.apple.com/us/app/github/id1477376905)

Apps used on Winos 10
- [Swift on Windows](https://www.swift.org/blog/swift-on-windows/) 5.5

### Concept
[Metal](https://developer.apple.com/metal/) is a compute shader (and more) API that utilizes GPUs in Apple devices. The [OptiX port](https://github.com/otabuzzman/RTXplay/tree/main/optx) (RTWO) of RTOW uses GPUs on NVIDIA graphic cards. The obvious idea was a port of RTWO to iOS. Sounds simple but the devil's in the details: Swift instead of C/C++, Playgrounds instead of an IDE, and working on tablet instead of PC to name just a few. The approach was to explore the new concepts one at a time.

### Setup
- Install Cygwin with development tools (for what it's good for)
- Swift on Windows [installation](https://www.swift.org/getting-started/) (according to *Traditional Installation' section)
- GitHub repository initialization
  ```
  git init MetalRT
  cd MetalRT
  
  swift package init
  ```
  In first line of `Package.swift` change Swift version from 5.5 to 5.3, the version supported by Swift Playgrounds 4.
  ```
  // swift-tools-version:5.3
  ...
  ```
  ```
  git add -A
  git push origin main
  ```

### Which file for what
|File|Comment|
|:---|:------|
|`Object.swift`|A class to read in a single scene object from a Waveform OBJ file.|

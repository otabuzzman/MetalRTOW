# MetalRTOW
Ray tracing with Metal. The lab aims to get Peter Shirley's ray tracer from his mini-book [Ray Tracing in one weekend](https://github.com/RayTracing/raytracing.github.io/) (RTOW) with [Metal](https://developer.apple.com/metal/) up and running.

### Tools
Apps used on iPad
- [Swift Playgrounds 4](https://apps.apple.com/de/app/swift-playgrounds/id908519492)
- [Working Copy](https://workingcopyapp.com/)
- [Textastic](https://www.textasticapp.com/) (can edit SP4 `.swiftpm` files)
- [GitHub](https://apps.apple.com/us/app/github/id1477376905)

Apps used on Winos 10
- [Swift on Windows](https://www.swift.org/blog/swift-on-windows/) 5.6

### Setup
- Install [Cygwin](https://cygwin.com/install.html) with development tools (for what it's good for)
- [Install Swift on Windows](https://www.swift.org/getting-started/) (according to *Traditional Installation* section)
- Create repository on GitHub (default settings)
- Clone repository from GitHub

  **Cygwin command prompt (bash)**
  ```
  git clone https://github.com/otabuzzman/MetalRTOW
  # use SSH (optional)
  # git clone git://github.com/otabuzzman/MetalRTOW

  cd MetalRTOW
  ```
- Create SSH keys (optional)
  ```
  ssh-keygen -t ed25519 -C iuergen.schuck@gmail.com -f ~/.ssh/github.com.MetalRTOW
  chmod 400 ~/.ssh/github.com.MetalRTOW
  ```
- Swift package initialization

  **Winos command prompt (CMD)**
  ```
  cd MetalRTOW

  swift package init

  rem check
  swift build
  ```
- Sync package with GitHub

  **Cygwin command prompt (bash)**
  ```
  # enable SSH key usage for session
  # eval "$(ssh-agent -s)"
  # ssh-add ~/.ssh/github.com.MetalRTOW

  git add -A
  git push origin main

  # tag required to load package with SP4
  git tag -a 0.1.0 -m 'initial commit'
  git push origin 0.1.0
  ```

### Which file for what
|File|Comment|
|:---|:------|
|`MUIView.swift`|A SwiftUI wrapper for MRTView.|
|`MRTView.swift`|A Metal ray tracer view protocol derived from [MTKView](https://developer.apple.com/documentation/metalkit/mtkview).|
|`MRTRenderer.swift`|A Metal ray tracing renderer protocol.|
|`MRTObject.swift`|A class to read a Waveform OBJ file.|
|`MTLDevice.swift`|A protocol extension for [MTLDevice](https://developer.apple.com/documentation/metal/mtldevice) to read Metal files from resource files in SP4.|

### Usage
- Setup new app in SP4
- Delete `*.swift` files
- Add `MetalRTOW` package
- Create file `MetalRTOW.swift` :

  ```
  import SwiftUI
  import MetalRTOW

  struct ContentView: View {
      var body: some View {
          MUIView {
              RtwmView()
          }
      }
  }

  @main
  struct MyApp: App {
      var body: some Scene {
          WindowGroup {
              ContentView()
          }
      }
  }
  ```
- Create file `RtwmView.swift` :

  ```
  import MetalKit
  import MetalRTOW

  class RtwmView: MTKView, MRTView {
      var renderer: MRTRenderer!

      required init() {
          guard let device = MTLCreateSystemDefaultDevice() else {
              let error: Error = MRTError.noDefaultDevice
              fatalError(error.localizedDescription)
          }
          super.init(frame: .zero, device: device)

          tune()
      }

      required init(coder: NSCoder) {
          super.init(coder: coder)
      }

      func tuneMTKView() {
          colorPixelFormat = .bgra8Unorm
          clearColor = MTLClearColor(red: 0.3, green: 0.1, blue: 0.2, alpha: 1.0)
      }

      func tuneMRTView() {
          renderer = try? RtwmRenderer(view: self)
      }
  }
  ```
- Create file `RtwmRenderer.swift` :

  ```
  import MetalKit
  import MetalRTOW

  final class RtwmRenderer: NSObject, MRTRenderer {
      var mtlLibrary: [MTLLibrary]!

      func makeAccelerationStructure() {
      }

      func makeShaderPipeline() {
      }

      func mtkView(_ view: MTKView, drawableSizeWillChange size: CGSize) {
      }

      func draw(in view: MTKView) {
      }
  }
  ```
- Run app and check result :<br>
  A white display with no errors.

### Findings
- Using `MTLPrimitiveType point` for `renderEncoder.drawIndexedPrimitives.type` yields kind of *bricks* whereas `line` and `triangle` work.
- No `.metal` file support in SP4. Metal Shader Language (MSL) code via `String` class in Swift source files (e.g. ending on `.metal.swift`) works for SP4 playgrounds and apps. The latter allows MSL files as resources. File suffixes must have three characters (e.g. `.msl`). Otherwise (e.g. when using `.metal` suffix) SP4 will report an unknown resource error on app open.
- Full URL of repository including `.git` required by package import in a SP4 app.
- Opening source files from imported packages yields various compiler errors. App will not run as long as the file is open but works when the file is closed.
- Removing resource files will only make them disappear in the file browser, not on storage.
- Package name in app settings must not match name of a loaded package.

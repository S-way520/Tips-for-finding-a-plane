# Tips-for-finding-a-plane
A component used to load AR models.
# The first part
![IMG_1234](https://github.com/S-way520/Tips-for-finding-a-plane/assets/95877651/09e90606-3b97-45ee-bd2b-6d4e63caaed0)
# The second part
Code file 1
```swift
import SwiftUI
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            Reality()
        }
    }
}
```
Code file 2
```swift
import SwiftUI
import RealityKit
import ARKit
struct Reality: View {
    var body: some View {
        ARViewContainer().edgesIgnoringSafeArea(.all)
    }
}
struct ARViewContainer: UIViewRepresentable {
    func makeUIView(context: Context) -> ARView {
        let arView = ARView(frame: .zero, cameraMode: .ar, automaticallyConfigureSession: true)
        let anchorEntity = AnchorEntity(plane: .horizontal)
        arView.addCoaching()//Prompt to find the plane 1/2
        let box = MeshResource.generateBox(size: 0.2/2, cornerRadius: 0.05/5)
        let material = SimpleMaterial(color: .blue, isMetallic: true)
        let ARentity = ModelEntity(mesh: box, materials: [material])
        anchorEntity.addChild(ARentity)
        arView.scene.anchors.append(anchorEntity)
        return arView
    }
    func updateUIView(_ uiView: ARView, context: Context) {
    }
}
//Prompt to find the plane 2/2
extension ARView: ARCoachingOverlayViewDelegate {
    func addCoaching() {
        let coachingOverlay = ARCoachingOverlayView()
        coachingOverlay.delegate = self
        coachingOverlay.session = self.session
        coachingOverlay.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        coachingOverlay.goal = .anyPlane
        self.addSubview(coachingOverlay)
    }
    public func coachingOverlayViewDidDeactivate(_ coachingOverlayView: ARCoachingOverlayView) {
        //coachingOverlayView.activatesAutomatically = false//stop find
    }
}
```

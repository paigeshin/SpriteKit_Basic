# SpriteKit_Basic

```swift
class ViewController: UIViewController {

    // MARK: (1) CREATE SKVIEW
    private lazy var animationView = SKView()
    
    override func loadView() {
        super.loadView()
        self.view.addSubview(self.animationView)
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        // Make sure we don't recreate the scene when the view re-appears
        guard self.animationView.scene == nil else { return }
        
        // MARK: (2) MAKE A SCENE
        let minimumDimension = min(view.frame.width, view.frame.height)
        let size = CGSize(width: minimumDimension, height: minimumDimension)
        let scene = SKScene(size: size)
        scene.backgroundColor = .white
        
        // MARK: (3) CREATE NODES AND ADD IT TO SCENE
        let allEmoji: [Character] = ["🔥", "✅", "⭐️", "🚀"]
        let distance = floor(scene.size.width / 4)
        for (index, emoji) in allEmoji.enumerated() {
            let node = SKLabelNode()
            node.fontSize = 50
            node.text = String(emoji)
            node.verticalAlignmentMode = .center
            node.horizontalAlignmentMode = .center
            node.position.y = floor(scene.size.height / 2)
            node.position.x = distance * (CGFloat(index) + 0.5)
            scene.addChild(node)
        }
        
        // MARK: (4) SET UP SCENE AND PRESENT IT
        self.animationView.frame.size = scene.size
        self.animationView.presentScene(scene)
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        
        // MARK: (5) CENTER POSITION 
        self.animationView.center.x = self.view.bounds.midX
        self.animationView.center.y = self.view.bounds.midY
        
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
    
        let nodes = self.animationView.scene!.children
        for (index, node) in nodes.enumerated() {
            node.run(.sequence([
                .wait(forDuration: TimeInterval(index) * 0.2),
                .repeatForever(.sequence([
                    // A group of actions get performed simultaneously
                    .group([
                        .sequence([
                            .scale(to: 1.5, duration: 0.3),
                            .scale(to: 1, duration: 0.3)
                        ]),
                        // Rotate by 360 degrees (pi * 2 in radians)
                        .rotate(byAngle: .pi * 2, duration: 0.6)
                    ]),
                    .wait(forDuration: 2)
                ]))
            ]))
        }
        
    }

}


```

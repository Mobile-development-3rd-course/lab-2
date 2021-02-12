<div align = "center"><strong>
Міністерство освіти і науки України<br>
Національний технічний університет України<br>
«Київський політехнічний інститут ім. Ігоря Сікорського»<br>
Факультет інформатики та обчислювальної техніки<br>
Кафедра обчислювальної техніки<br><br><br><br>
</strong></div>

<div align = "center" bold = ""><strong>ЛАБОРАТОРНА РОБОТА № 2</strong><br><br>
з дисципліни «Розроблення клієнтських додатків для мобільних платформ»<br>
на тему «Drawing in a UIView: UIView, UIBezierPath, UISegmentedControl»</div><br><br><br><br>

<div align = "right" >Виконав:<br>
студент ІІІ курсу ФІОТ<br>                        
групи ІП-84<br>                                
Андрейченко Кирило<br>
номер залікової книжки: 8401<br></div><br><br><br><br>

<div align = "center">Київ – 2021</div><br><br><br><br>

<strong>Варіант роботи номер 2.</strong><br>
<strong>Скріншоти роботи застосунка:</strong><br>
1. Зображення графіка Portrait<br>
<p align = "center"><img src="https://i.imgur.com/7FGRRs5.png" alt="cos in portrait"/></p><br>

2. Зображення графіка Landscape<br>
<p align = "center"><img src="https://i.imgur.com/KEkCWNh.png" alt="cos in landscape"/></p><br>

3. Зображення діаграми Portrait<br>
<p align = "center"><img src="https://i.imgur.com/06t3sk6.png" alt="diagram in landscape"/></p><br>

4. Зображення графіка діаграми Landscape<br>
<p align = "center"><img src="https://i.imgur.com/gRQfz32.png" alt="diagram in landscape"/></p><br>

<strong>Лістинг коду:</strong><br>

<i>DrawingViewController.swift</i>
```swift
import UIKit

@IBDesignable
class DrawingViewController: UIViewController {
    
    @IBOutlet var diagram: DiagramView!
    @IBOutlet var cos: CosView!
    
    override func viewDidLoad() {
        diagram.alpha = 0
        super.viewDidLoad()
    }
    
    @IBAction func suitDidChange(_ sender: UISegmentedControl) {
        switch sender.selectedSegmentIndex {
        case 0:
            cos.alpha = 1
            diagram.alpha = 0
        case 1:
            cos.alpha = 0
            diagram.alpha = 1
        default:
            break
        }
        
    }
    
    
}
```

<i>ViewController.swift</i>
```swift
import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
    }
    
}
```

<i>AppDelegate.swift</i>
```swift
import UIKit

@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        return true
    }

    // MARK: UISceneSession Lifecycle
    func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        // Called when a new scene session is being created.
        // Use this method to select a configuration to create the new scene with.
        return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
    }

    func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
        // Called when the user discards a scene session.
        // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
        // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
    }

    
}
```

<i>CosView.swift</i>
```swift

import UIKit

class CosView: UIView {
    // MARK: - Colors
    let blackColor = UIColor.black
    let blueColor = UIColor.blue
    
    // MARK: - Main draw function
    override func draw(_ rect: CGRect) {
        let rectWidth = rect.width
        let rectHeight = rect.height
        
        designCoordinates(rect, rectWidth, rectHeight)
        designCos(rect, rectWidth, rectHeight)
    }
    
    // MARK: Cos(x)
    private func designCos(_ rect: CGRect, _ rectWidth: CGFloat, _ rectHeight: CGFloat) {
        let origin = CGPoint(x: rectWidth * (1 - 1.1 ) / 2, y: rectHeight * 0.50)
        let cosFunc = UIBezierPath()
        cosFunc.move(to: origin)
        
        for angle in stride(from: 5.0, through: 360.0, by: 5) {
            let x = origin.x + CGFloat(angle / 360.0) * rectWidth * 1.1
            let y = origin.y - CGFloat(-cos(angle / 180.0 * Double.pi)) * rectHeight * 0.2
            cosFunc.addLine(to: CGPoint(x: x, y: y))
        }
        
        blueColor.setStroke()
        cosFunc.stroke()
    }
    
    // MARK: - Coordinates
    private func designCoordinates (_ rect: CGRect, _ rectWidth: CGFloat, _ rectHeight: CGFloat) {
        let rectCenter: CGPoint = CGPoint(x: rectWidth / 2, y: rectHeight / 2)
        let coordinates = UIBezierPath()
        
        // Coordinate X
        coordinates.move(to: CGPoint(x: 0, y: rectCenter.y))
        coordinates.addLine(to: CGPoint(x: rectHeight, y: rectCenter.y))
        coordinates.addLine(to: CGPoint(x: rectWidth - 10, y: rectHeight / 2 - 7))
        
        coordinates.move(to: CGPoint(x: rectWidth, y: rectHeight / 2))
        coordinates.addLine(to: CGPoint(x: rectWidth - 10, y: rectHeight / 2 + 7))
        
        coordinates.move(to: CGPoint(x: rectCenter.x - 7, y: rectCenter.y - 58))
        coordinates.addLine(to: CGPoint(x: rectCenter.x + 7, y: rectCenter.y - 58))
        
        // Coordinate Y
        coordinates.move(to: CGPoint(x: rectCenter.x, y: 0))
        coordinates.addLine(to: CGPoint(x: rectCenter.x, y: rectWidth))
        
        coordinates.move(to: CGPoint(x: rectWidth / 2, y: 0))
        coordinates.addLine(to: CGPoint(x: rectWidth / 2 - 7, y: 10))
        
        coordinates.move(to: CGPoint(x: rectWidth / 2, y: 0))
        coordinates.addLine(to: CGPoint(x: rectWidth / 2 + 7, y: 10))
        coordinates.move(to: CGPoint(x: rectCenter.x + 58 , y: rectCenter.y + 7))
        coordinates.addLine(to: CGPoint(x: rectCenter.x + 58, y: rectCenter.y - 7))
        
        coordinates.lineWidth = 1
        blackColor.setStroke()
        coordinates.stroke()
    }
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        backgroundColor = .systemBackground
    }
    
    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
    }
    
    
}
```

<i>DiagramView.swift</i>
```swift
import UIKit

class DiagramView: UIView {
    
    override func draw(_ rect: CGRect) {
        
        let segmentsArray: [(UIColor, Double)] = [(.cyan, 0.45),
                                                  (.purple, 0.05),
                                                  (.yellow, 0.25),
                                                  (.gray, 0.25)]
        let rectWidth: CGFloat = rect.width
        let rectHeight: CGFloat = rect.width
        let center = CGPoint(x: rectWidth / 2, y: rectHeight / 2)
        let radius = rect.width / 2.2
        
        var startAngle = CGFloat(0)
        for (color, value) in segmentsArray {
            let segment = UIBezierPath()
            let endAngle = startAngle + (360 * CGFloat(value) * CGFloat(Double.pi) / 180)
            
            segment.addArc(withCenter: center,
                           radius: radius,
                           startAngle: startAngle,
                           endAngle: endAngle,
                           clockwise: true)
            
            segment.addArc(withCenter: center,
                           radius: radius / 2,
                           startAngle: endAngle,
                           endAngle: startAngle,
                           clockwise: false)
            
            color.setFill()
            color.setStroke()
            segment.fill()
            segment.stroke()
            
            startAngle = endAngle
        }
        
    }
    
    
    
}
```
<strong>Висновок:</strong><br>
На другій лабораторній роботі я познайомився з малюванням в UIView за допомогою UIBezierPath. Також я використав UISegmentedControl для того щоб перемикаться між графіком і діаграмою.

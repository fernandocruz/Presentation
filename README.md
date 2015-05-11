# Presentation

[![CI Status](http://img.shields.io/travis/hyperoslo/Presentation.svg?style=flat)](https://travis-ci.org/hyperoslo/Presentation)
[![Version](https://img.shields.io/cocoapods/v/Presentation.svg?style=flat)](http://cocoadocs.org/docsets/Presentation)
[![License](https://img.shields.io/cocoapods/l/Presentation.svg?style=flat)](http://cocoadocs.org/docsets/Presentation)
[![Platform](https://img.shields.io/cocoapods/p/Presentation.svg?style=flat)](http://cocoadocs.org/docsets/Presentation)

Looking for the easy way of creating magazine-like slides in your iOS app? Then you are in the right place. **Presentation** will help you to make your tutorials, release notes and any kind of animated pages with the minimum amount of effort.

*Presentation* includes the following features:

- Custom positioning: you can use [Position](https://github.com/hyperoslo/Presentation/blob/master/Source/Position.swift) for percentage-based position declaration.
- [Content](https://github.com/hyperoslo/Presentation/blob/master/Source/Content.swift): View model used for custom positioning and animations. It translated your percents to AutoLayout constraints behind the scene.
- Slides: You can use any kind of `UIViewController` as a slide. [SlideController](https://github.com/hyperoslo/Presentation/blob/master/Source/SlideController.swift) is your good friend if you want to use custom positioning and animation features on your pages.
- Background: You can add views that are visible across all the pages. Also it's possible to animate those views during the transition to the specific page.  
- [Animations](https://github.com/hyperoslo/Presentation/tree/master/Source/Animations): You can easily animate the appearance of a view on the specific page.

Presentation works both on the iPhone and the iPad. You can use it with both `Swift` and `Objective-C`.

Try one of our [demos](https://github.com/hyperoslo/Presentation/tree/master/Demos) to see how it works.

## Usage

### Presentation controller

```swift
import Presentation

let viewController1 = UIViewController()
viewController1.title = "Controller A"

let viewController2 = UIViewController()
viewController2.title = "Controller B"

let presentationController = PresentationController(pages: [viewController1, viewController2])

```

If it's the only thing you need mind looking at [Pages](https://github.com/hyperoslo/Pages), it does the same for you.

### Position

`Position` is percentage-based, you can use `left`, `right`, `top`, `bottom` to create a position.

```swift
let position = Position(left: 0.3, top: 0.4)
```

### Content view model

`Content` view model is a layer between `UIView` and `Position`. The current position is the center of a view by default, but could also be changed to the origin of a view.

```swift
let view = UIView(frame: CGRect(x: 0, y: 0, width: 200, height: 100))
let position = Position(left: 0.3, top: 0.4)

let centeredContent = Content(view: label, position: position)
let originContent = Content(view: label, position: position, centered: false)
```

### Slides

```swift
let label = UILabel(frame: CGRect(x: 0, y: 0, width: 200, height: 100))
label.text = "Slide 1"

let position = Position(left: 0.3, top: 0.4)
let content = Content(view: label, position: position)

let controller = SlideController(contents: [content])

presentationController.add([controller])
```

### Page animations

```swift
let contents = ["Slide 1", "Slide 2", "Slide 3"].map { title -> Content in
  let label = UILabel(frame: CGRect(x: 0, y: 0, width: 200, height: 100))
  label.text = title

  let position = Position(left: 0.3, top: 0.4)

  return Content(view: label, position: position)
}

var slides = [SlideController]()

for index in 0...4 {
  let content = contents[index]
  let controller = SlideController(contents: [content])
  let animation = TransitionAnimation(
    content: content,
    destination: Position(left: 0.5, top: content.initialPosition.top),
    duration: 2.0,
    dumping: 0.8,
    reflective: true)
  controller.addAnimations([animation])

  slides.append(controller)
}

presentationController.add(slides)
```

## Background views

```swift
let imageView = UIImageView(image: UIImage(named: "image"))
let content = Content(view: imageView, position: Position(left: -0.3, top: 0.2))

presentationController.addToBackground([content])

// Add pages animations
presentationController.addAnimations([
  TransitionAnimation(content: content, destination: Position(left: 0.2, top: 0.2))],
  forPage: 0)

presentationController.addAnimations([
  TransitionAnimation(content: content, destination: Position(left: 0.3, top: 0.2))],
  forPage: 1)
```

## Installation

**Presentation** is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'Presentation'
```

## Contributing

Please check our [playbook](https://github.com/hyperoslo/playbook/blob/master/GIT_AND_GITHUB.md) for guidelines on contributing.

## Credits

[Hyper](http://hyper.no) made this. We’re a digital communications agency with a passion for good code and delightful user experiences. If you’re using this library we probably want to [hire you](https://github.com/hyperoslo/iOS-playbook/blob/master/HYPER_RECIPES.md) (we consider remote employees too, the only requirement is that you’re awesome).

## License

Presentation is available under the MIT license. See the [LICENSE](https://github.com/hyperoslo/Presentation/blob/master/LICENSE.md).

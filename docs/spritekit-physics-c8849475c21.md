# 精神物理学

> 原文：<https://blog.devgenius.io/spritekit-physics-c8849475c21?source=collection_archive---------2----------------------->

![](img/ceda03ffddc870c55edb94c15f9d9979.png)

照片由[凯·皮尔格](https://unsplash.com/@kaip?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 绪论

截至本文撰写时，我已经使用苹果的 SpriteKit 框架大约 6 个月了。在这段时间里，我逐渐认识到，SpriteKit 是一种了解游戏开发的好方法，因为与使用 Unity 等游戏引擎相比，你会做更多的低级工作，我认为在转向全面的游戏引擎之前，掌握基础知识是很重要的。从我的理解来说，底层工作与动画、物理、音频、网络、渲染有关，而高层工作侧重于用户界面和游戏性。如果这是不正确的，并且你知道这是不正确的，请让我知道。

我坚持用 SpriteKit 开发我的第一个游戏，因为它给了我游戏开发的这一方面，加上我是一名 iOS 开发者，所以当专注于 iOS 游戏时，向游戏开发的过渡更容易一些。然而，我确实计划在未来转向 Unity 进行更有雄心的项目。

我写这篇博客的目的是告诉你关于 SpriteKit 中的物理系统，基于我在开发中使用它的情况。我要说的是，说到精神，我还有很多东西要学，所以请不要把这当成“一站式、无所不知”的指南。我鼓励你亲自阅读苹果文档，并通过构建一个游戏来体验 SpriteKit。

话虽如此，我们还是开始吧。(可以在这里找到所指项目的回购:[https://github . com/Andrew-lundy/balls-v-walls/tree/sprite kit-physics](https://github.com/andrew-lundy/balls-v-walls/tree/spritekit-physics))。

# 物理学导论

正如预期的那样，斯普里特物理系统的行为方式与我们在地球上所经历的类似。通过在游戏中加入物理学，你可以模拟重力、速度、碰撞检测等等。同样，SpriteKit 中有一些东西我还没有接触过，比如`SKPhysicsContact`和`SKField`节点，所以我将坚持使用我已经使用过的东西。截至目前，这包括`SKPhysicsWorld`、`SKPhysicsBody`、`SKPhysicsContatDelegate`。

# 物理学的 2D 世界

在 SpriteKit 中，SKPhysicsWorld 是场景中的驱动物理引擎。它可以让你设置重力和速度，在物理碰撞之间作出反应，并进行自定义点击测试。

每个场景都会自动创建一个 physicsWorld 属性，这意味着您不必担心自己创建一个`SKPhysicsWorld`对象。重力属性是一个以米/秒为单位的`CGVector`。重力的默认值是`(0.0, -9.8)`，代表地球的重力。你可以很容易地改变游戏的重心，创造一些很酷的效果。

在我的一个名为“Balls v. Walls”的废弃游戏中(这是连接到本博客的项目)，我通过将游戏设置为`(0.0, 9.8)`来反转游戏的重力。在这个例子中，这发生在玩家得了 2 分，并且“球”被拉向屏幕的顶部而不是被拉向底部之后。

```
if score == 2 {
    GlobalVariables.shared.gameState = GameState.antiGravity
    SKAction.run {
        SKAction.wait(forDuration: 1)
    }
    self.physicsWorld.gravity = CGVector(dx: 0, dy: 9.8)
}
```

`physicsWorld`也能让你增加或减少场景中模拟的速率。这个属性叫做速度，正如你所看到的，把它设置为“2”，游戏的速度就提高了。

```
override func didMove(to view: SKView) {
    physicsWorld.speed = 2 
}
```

我之前提到过，physicsWorld 允许游戏开发者对他们游戏中的碰撞做出反应。这是通过一个名为 contactDelegate 的属性和一个名为`SKPhysicsContactDelegate`的协议完成的。在进入这个话题之前，我们先来谈谈`SKPhysicsBody`。

**SKPhysicsBody**
An`SKPhysicsBody`为节点定义物理实体的形状和模拟参数。在`SKPhysicsBody`中，你可以改变物理实体的物理属性，改变力如何影响物理实体，管理物理实体的碰撞&接触，应用脉冲等等。

与`SKPhysicsWorld`不同，您必须为您的节点手动创建一个`SKPhysicsBody`。您创建一个`SKPhysicsBody`对象，将其分配给您为其创建物理实体的任何节点的`physicsBody`属性，然后为其设置属性。您可以从形状或纹理创建一个`SKPhysicsBody`。

SpriteKit 中有三种类型的物理实体——*动态*、*静态*和*边缘*。*动态*物体是一个物理物体，它会受到物理系统中的力和碰撞的影响。一个静态的物体是一个不受力和碰撞影响的物理物体。这些通常是您希望在场景中保持静止的节点。一个静止的物体仍然保持体积，因此其他物理物体仍然可以反弹。一个*边*是一个静态的无体积体。这些用于表示场景中的负空间，并且从不移动。很多时候，你会用一条边来表示场景的边界。

一个物理实体可以由三种形状和一种纹理组成。这些形状包括圆形、矩形和多边形。

在[苹果文档](https://developer.apple.com/documentation/spritekit/skphysicsbody)中阅读更多关于`SKPhysicsBody`的信息。

下面是一个简单的例子，一个`SKPhysicsBody`被创建并分配给“球对墙”中的球它是由一定的半径大小产生的。然后，设置两个属性；那些是`restitution`和`isDynamic`。物理物体的`restitution`基本上是它的弹性。`isDynamic`属性是一个布尔值，表示物理实体是否是动态的。如果此属性设置为 true，则物理实体将在物理模拟中移动。

```
func createBall() {
    // Create the SKPhysicsBody for the ball.
    ball.physicsBody = SKPhysicsBody(circleOfRadius: 50)
    // Set the restitution (bounciness) of the ball.
    ball.physicsBody?.restitution = 0.6
    // Set the ball to be dynamic.
    ball.physicsBody?.isDynamic = true
}
```

关于`SKPhysicsBody`，我想谈的下一件事是位掩码类型。位掩码是定义节点的`physicsBody`如何与场景中其他节点上的碰撞和接触进行交互的掩码。

下面是苹果对接触和碰撞的描述:
**1。)当你需要知道两个物体相互接触时，就要用到接触。在大多数情况下，当冲突发生时，当你需要改变游戏时，你可以使用隐形眼镜。

2.)碰撞用于防止两个物体互相穿透。当一个物体撞击另一个物体时，SpriteKit 会自动计算碰撞的结果，并将脉冲应用于碰撞中的物体。**

我们要讲的第一个面具是`categoryBitMask`。该遮罩定义了物理实体所属的类别。一个物理实体可以被分配到 32 个不同的类别。这取决于开发者在他们的游戏中定义掩码值。我使用了一个名为`CollisionTypes`的枚举来定义“球对墙”中的不同类别。

```
enum CollisionTypes: UInt32 {
    case ball = 1
    case wall = 2
    case ground = 4
    case scoreDetect = 8
}
```

下面是一个更新的`createBall`方法，包括设置球的`categoryBitMask`。

```
func createBall() {
    ball.physicsBody = SKPhysicsBody(circleOfRadius: 50)
    ball.physicsBody?.restitution = 0.6
    ball.physicsBody?.isDynamic = true
    // Set the categoryBitMask of the ball
    ball.physicsBody?.categoryBitMask = CollisionTypes.ball.rawValue
}
```

接下来是`contactTestBitMask`。该掩码定义了哪些类别会引起与节点物理实体的相交通知。请注意，两个节点可以接触，但它们并不总是会导致冲突。当两个物体最终共享同一个空间时，它们的类别位掩码会相互测试，如果它们产生非零值，就会创建一个`SKPhysicsContact`对象并将其传递给物理世界代理。在“球对墙”中，当球穿过墙的相应部分时，我用这个来增加分数。

```
func createBall() {
    ball.physicsBody = SKPhysicsBody(circleOfRadius: 50)
    ball.physicsBody?.restitution = 0.6
    ball.physicsBody?.isDynamic = true
    ball.physicsBody?.categoryBitMask = CollisionTypes.ball.rawValue
    // Set the contactTestBitMask of the ball. This will let the physics
    // system know that the ball and wall have made contact.
    ball.physicsBody?.contactTestBitMask = CollisionTypes.wall.rawValue
 }
```

最后，我们有了`collisionBitMask`。正如您可能期望的那样，该遮罩定义了可以与节点碰撞的物理实体的类别。它的工作方式与`contactTestBitMask`类似，当物理物体发生碰撞时，它会执行逻辑与运算。同样，如果结果是非零的值，物体会受到碰撞的影响。

下面，你可以看到我将球的`collisionBitMask`设置为两个值- `CollisionTypes.ground.rawValue`和`CollisionTypes.wall.rawValue`。这保证了球会与地面和墙壁发生碰撞。此处的管道符号允许您为`collisionBitMask`设置多个值。

```
func createBall() {
    ball.physicsBody = SKPhysicsBody(circleOfRadius: 50)
    ball.physicsBody?.restitution = 0.6
    ball.physicsBody?.isDynamic = true
    ball.physicsBody?.categoryBitMask = CollisionTypes.ball.rawValue
    ball.physicsBody?.contactTestBitMask = CollisionTypes.wall.rawValue
    // Set the collisionBitMask of the ball. This lets the ball collide with the ball and the wall.
    ball.physicsBody?.collisionBitMask = CollisionTypes.ground.rawValue | CollisionTypes.wall.rawValue
}
```

您可以设置所有您想要的位掩码，但是除非您集成了`SKPhysicsContactDelegate`，否则这些东西都不会工作。

**SKPhysicsContactDelegate**该方法赋予一致性对象响应基于其`contactTestBitMask`相互接触的物理实体的能力。为了使用该协议，您只需在一个类中采用它，就像任何其他 Swift 协议一样。在这种情况下，我的`GameScene`类采用了`SKPhysicsContactDelegate`。

```
class GameScene: SKScene, SKPhysicsContactDelegate {
    override func didMove(to view: SKView) {
        physicsWorld.contactDelegate = self
    }
}
```

我从`SKPhysicsContactDelegate`开始使用的主要方法是`didBegin(_ contact:)`方法，当两个物理实体相互接触时调用该方法。在“Balls v. Walls”中，我检查了发生冲突的两个节点，然后使用自定义方法来处理这些点击。

```
func didBegin(_ contact: SKPhysicsContact) {
    guard let nodeA = contact.bodyA.node else { return }
    guard let nodeB = contact.bodyB.node else { return } if nodeA == newBall {
        ballCollided(with: nodeB)
        } else if nodeB == newBall {
            ballCollided(with: nodeA)
        }
    }
}
```

# 结论

使用 SpriteKit 可以做很多事情，我真的鼓励你亲自去看看。如果你是一名 iOS 开发者，学习 SpriteKit 会相当顺利。如果你不是 iOS 开发者，也没有写过 Swift，我会建议你在开发游戏之前先把这种语言的基础弄好——但是我承认每个人学习的方式都不一样，所以请做你觉得舒服的事情！

除了 SpriteKit 还有很多其他的选择，比如 Unity、Godot、GameMaker 等等。使用最适合您的项目的方法——并尝试帮助社区中可能有疑问的人！

安德鲁
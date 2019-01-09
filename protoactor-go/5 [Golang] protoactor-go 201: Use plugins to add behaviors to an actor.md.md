# [[Golang] protoactor-go 201: Use plugins to add behaviors to an actor](https://blog.oklahome.net/2018/12/protoactor-go-use-plugin-to-add-behavior.html)使用插件为演员添加行为

The previous article, “[How middleware works to intercept incoming and outgoing messages](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html)”, introduced how middlewares can be used to add behaviors without modifying an actor’s implementation. This is a good AOP-ish approach for multiple types of actors to execute a common procedure such as logging on message receiving/sending. A plugin mechanism is implemented on top of this middleware mechanism to run a specific task on actor initialization and message reception to enhance an actor’s ability. This article covers the implementation of this plugin feature and how this can be used.

前一篇文章“  [中间件如何拦截传入和传出的消息](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html)  ”，介绍了如何在不修改actor的实现的情况下使用中间件来添加行为。  对于多种类型的actor来说，这是一种很好的AOP-ish方法，可以执行常见的过程，例如登录消息接收/发送。  在这个中间件机制之上实现了一个插件机制，用于在actor初始化和消息接收上运行特定任务，以增强actor的能力。  本文介绍了此插件功能的实现以及如何使用它。

-   [Under the Hood](https://blog.oklahome.net/2018/12/protoactor-go-use-plugin-to-add-behavior.html#toc_0)
-   [Example](https://blog.oklahome.net/2018/12/protoactor-go-use-plugin-to-add-behavior.html#toc_1)
-   [Conclusion](https://blog.oklahome.net/2018/12/protoactor-go-use-plugin-to-add-behavior.html#toc_2)

# Under the Hood

Implementing plugin is as easy as fulfilling the below  [`plugin.plugin`  interface](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/plugin/plugin.go#L7-L10).

```go
type plugin interface {
  OnStart(actor.Context)
  OnOtherMessage(actor.Context,  interface{})
}
```

When a  `plugin.plugin`  implementation is passed to  [`plugin.Use()`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/plugin/plugin.go#L12-L26), this wraps the given plugin and return in a form of  [`actor.InboundMiddleware`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/props.go#L5)  so this can be set to  `actor.Props`  as a middleware.

```go
func  Use(plugin  plugin)  func(next  actor.ActorFunc)  actor.ActorFunc  {
  return  func(next  actor.ActorFunc)  actor.ActorFunc  {
    fn  :=  func(context  actor.Context)  {
      switch  msg  :=  context.Message().(type)  {
      case  *actor.Started:
        plugin.OnStart(context)
      default:
        plugin.OnOtherMessage(context,  msg)
      }

      next(context)
    }
    return  fn
  }
}
```

As shown in the above code fragment,  `plugin.OnStart`  is called on actor initialization;  `plugin.OnOtherMessage`  is called on subsequent message receptions. A developer may initialize plugin on  `plugin.OnStart`  so its logic can run on other message receptions. Remember that  `next(context)`  is called at the end of its execution so the actor’s  `actor.Receive()`  is called after the plugin logic runs. A minimal implementation can be somewhat like below:

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"github.com/AsynkronIT/protoactor-go/plugin"

)

type myPlugin struct {

}

func (_ *myPlugin) OnStart(actor.Context) {

// Do nothing

}

func (_ *myPlugin) OnOtherMessage(actor.Context, interface{}) {

// Do nothing

}

func main() {

// Construct plugin implementation

myPlugin := &myPlugin{}

// Wrap plugin implementation in a form of InboundMiddleware

middleware := plugin.Use(myPlugin)

props := actor.

FromFunc(func(ctx actor.Context) {

// Some logic comes here

}).

WithMiddleware(middleware) // Set as a middleware

pid := actor.Spawn(props)

pid.Tell("dummy message")

}

[view raw](https://gist.github.com/oklahomer/04d44b1e5436d9727d5ee283247b223f/raw/a1facbb96e43438c46f2d7c1caaca77cbc770894/main.go)[main.go](https://gist.github.com/oklahomer/04d44b1e5436d9727d5ee283247b223f#file-main-go)  hosted with ❤ by  [GitHub](https://github.com/)

# Example

A good example should be  [a passivation plugin](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/plugin/passivation.go)  provided by protoactor-go itself. This is a plugin that enables an idle actor to stop when no message comes in for a certain amount of time. Such plugin comes in handy when a developer employs cluster grain architecture because a grain actor is automatically initialized on first message reception and this lives forever without such self destraction mechanism. When a message is received after the destraction, another grain actor is automatically created. This initializes a timer on actor initialization, resets a timer on every message reception and stops the actor when timer ticks.

One important thing to mention here is that this plugin makes an extra effort to let an actor implement  `plugin.PassivationAware`  by embedding  `plugin.PassivationHolder`  in actor struct so a developer does not have to implement  `plugin.PassivationAware`  by oneself.

package plugin

import (

"log"

"time"

"github.com/AsynkronIT/protoactor-go/actor"

)

type PassivationAware interface {

Init(*actor.PID, time.Duration)

Reset(time.Duration)

Cancel()

}

type PassivationHolder struct {

timer *time.Timer

done bool

}

func (state *PassivationHolder) Reset(duration time.Duration) {

if state.timer == nil {

log.Fatalf("Cannot reset passivation of a non-started actor")

}

if !state.done {

state.timer.Reset(duration)

}

}

func (state *PassivationHolder) Init(pid *actor.PID, duration time.Duration) {

state.timer = time.NewTimer(duration)

state.done = false

go func() {

select {

case <-state.timer.C:

pid.Stop()

state.done = true

break

}

}()

}

func (state *PassivationHolder) Cancel() {

if state.timer != nil {

state.timer.Stop()

}

}

type PassivationPlugin struct {

Duration time.Duration

}

func (pp *PassivationPlugin) OnStart(ctx actor.Context) {

if a, ok := ctx.Actor().(PassivationAware); ok {

a.Init(ctx.Self(), pp.Duration)

}

}

func (pp *PassivationPlugin) OnOtherMessage(ctx actor.Context, msg interface{}) {

if p, ok := ctx.Actor().(PassivationAware); ok {

switch msg.(type) {

case *actor.Stopped:

p.Cancel()

default:

p.Reset(pp.Duration)

}

}

}

[view raw](https://gist.github.com/oklahomer/7ee4d10c112230962ca4782b2e77aa1c/raw/d7b01516021dd9fd8ac86ce0c0f39e7f24a26b0c/passivation.go)[passivation.go](https://gist.github.com/oklahomer/7ee4d10c112230962ca4782b2e77aa1c#file-passivation-go)  hosted with ❤ by  [GitHub](https://github.com/)

Thanks for that effort, an actor implementation can be as simple as below. This is obvious that, because the passivation implementation itself is implemented by embedded  `plugin.PassivationHolder`,  `MyActor`  developer can separate the passivation procedure and concentrate on her own business.

```go
type MyActor struct {  
  // Implement plugin.PassivationAware by embedding its default implementation: plugin.PassivationHolder
  PassivationHolder  
}  
  
func (state *MyActor) Receive(context actor.Context) {  
  switch context.Message().(type) {  
    // Do its own business
  }  
}
```

# Conclusion

To add a pluggable behavior to an actor, a developer can provde a plugin by implementing  `plugin.plugin`  interface. By defining core interface and its embeddable default implementation of the plugin, it is quite easier to separate the areas of responsibility of a plugin and an actor.






<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzNjc2MTIzMywyOTc4MDUyMTFdfQ==
-->
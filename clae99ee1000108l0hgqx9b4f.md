# Yes, IO is possible in a functional world

Whenever I share my enthusiasm regarding functional programming and pure functions, other developers are often skeptical, and I've heard the following point possibly a million times:

> Functional programming might be cool in academic contexts, but in the real world, you need side effects! Or are you simply not going to have databases or user input? Good job writing your useless code!

It's cute. It's also misinformed. It's like saying:

> So a jigsaw is your favorite tool? Good luck getting a nail into a wall with a jigsaw

So if you would argue the same, then I am here to help you and possibly enlighten you. If you already know, you are welcome to share this with people who don't.

## Pure domain logic

The trick is to isolate your IO side effects.

![A gif of the annoying reading saying gotcha](https://media2.giphy.com/media/Xg5qpmzzzp10rt01l0/giphy.gif?cid=ecf05e47ex55hl9lp523p81rnfaa5d3fdrdiyz1fekwfdh2x&rid=giphy.gif&ct=g align="center")

I still have side effects then - I can hear you yell at your screen.

Well, back in my Advanced Programming course our professor had a great point:

>You can write a functional and pure framework, [enforcing/enabling only pure code], without the actual implementation thereof being functional

You still need to know which parts can contain IO and which cannot. [Haskell has a great language construct for exactly this](https://www.haskell.org/tutorial/io.html). In other languages, you are left with discipline and (possibly) doing some code analysis (maybe? I am not aware of any tools, but it'd be cool).


That might sound abstract so let's get down to something more concrete.

## Side effects in my project
As noted [I'm working on creating a micro-service kanban board](https://blog.cwb.dk/series/fanban) - and here I'll obviously also have IO. I need to save when a new [card](https://www.atlassian.com/agile/kanban/cards) is created or moved or well any change to the state.

So how do we do that?

Let's step back for a second. When writing applications, I (often) follow the [onion architecture / ports and adapters / clean architecture](https://blog.ploeh.dk/2013/12/03/layers-onions-ports-adapters-its-all-the-same/):

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668265408369/J7ISpavHj.png align="center")

Essentially dependencies (etc) should only go one way. You should be able to change the database technology, or web stack without changing a single line of code within your domain model.

And here I personally have a very strict perspective on my domain model. All the code in my domain should be completely side-effect free. 

That means whenever a function modifies a kanban board, it will not modify the instance but rather return a new one. I.e adding a card to the board is a function like this:

```fsharp
let SetBoardName (newName: Name) (this: Board) = { this with Name = newName }
```
*The `Name` type here is a [Value Object](https://en.wikipedia.org/wiki/Value_object) that ensures that it's not a non-empty string, and not too long)*

The above function is of type `Name -> Board -> Board` - It's a function that takes a Name and then returns a new function that takes a board and returns another. This is a general functional concept called [Partial Application](https://en.wikipedia.org/wiki/Partial_application). We can then use it like this:

```fsharp
let WithBoardName (newName: Name) (this: Board) = { this with Name = newName }
let WithBoardNameMyBoard = WithBoardName "My Board"

// We can then use it:
let myBoard = withBoardNameMyBoard yourBoard
```
*👻 spooky F# where I stole your board*

This doesn't affect the `yourBoard`, but simply copies the content to `myBoard`

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668272580900/03Zx61ceb.png align="center")

Anyway, that's an aside. Back to the topic. 
The domain and any processes in the domain remain pure - no side effects or anything.

## Accessing a database

But this doesn't write nor fetch anything to/from any database. So how do we actually do that? We accept impurities in the outer layers. 

I've simplified how the actual API looks, and only focused on the actual logic of the endpoint, however, it presents the concept:

```fsharp
let repository = InMemoryBoardRepository()

let setNameEndpoint (payload : SetNamePayload) = 
  let board = repository.Get payload.BoardId
  let updatedBoard = withBoardName payload.NewName board
  repository.Write payload.BoardId updatedBoard

  Ok200 ()
```

Oh no! we have a side-effect in our API. But it's isolated, and it's very very localized. 

Btw this is how the Repository looks:

```fsharp
type InMemoryBoardRepository() =
    let boards = Dictionary<BoardId, Board>()
    member this.GetAll() = boards.Values |> seq

    member this.Get(boardId: BoardId) =
        boards[boardId]

    member this.Write(board: Board) 
        // this just writes the value to the entry in the dictionary
        boards[board.Id] <- board
```

Yes, it has side effects, and anything that uses it will therefore also become tainted and unpure. But we can localize it and track it through dependencies. and we can keep the usage to a minimum. 

Furthermore, this pattern of "getting, updating, saving", is something we'll probably do quite a lot, we can abstract it away and add it to our Board Repository:

```fsharp
type InMemoryBoardRepository() =
    let boards = Dictionary<BoardId, Board>()
    member this.GetAll() = boards.Values |> seq

    member this.Get(boardId: BoardId) =
        boards[boardId]

    member this.Write(board: Board) =
        boards[board.Id] <- board

    member this.Update(mapper: Board -> Board) (boardId: BoardId) =
        let board = this.Get boardId
        this.Write (mapper board)
```

Which makes our code even simpler, and moves all the side effects into one place, leaving just this one expression in our API.


```fsharp
let repository = InMemoryBoardRepository()

let setNameEndpoint (payload : SetNamePayload) = 
  let partialFunction =withBoardName payload.NewName
  repository.Update partialFunction payload.BoardId
```
(Partial application became relevant after all! woo!)

![Gif of women saying that it's so beautiful](https://media1.giphy.com/media/p8GJOXwSNzQPu/giphy.gif?cid=ecf05e47su6mo54jl1bb5xfrj5nlzckwtp9o261b2dhnh2a7&rid=giphy.gif&ct=g align="center")

## Conclusion
Yes, you will need side effects. But by keeping your domain pure you improve testability, readability, and a wide range of other -ilities. By centralizing your side effects you are able to do more heavy testing here, while keeping the side effects as simple as possible, making the code much easier to reason about, and much less error-prone - And as our outer layer (application layer, data access, integrations) should we quite simple and very thin it should be very localized.

I hope the F#-parts didn't pose too big of a readability challenges. After a few weeks, I promise it'll read like beautiful prose. 

The next step would be to define our API simply by writing different `Board -> Board` functions. Let's discuss that in a future post.
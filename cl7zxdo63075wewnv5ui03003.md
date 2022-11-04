# Strongly typed Id in f#

## Motivation

Way too often I see code where someone defines an id as a simple `int` or `Guid` or something along those lines. This is a classic case of the [Primitive Obsession](https://blog.ploeh.dk/2011/05/25/DesignSmellPrimitiveObsession/) smell and has a variety of potential problems, as well as may [pollute your code with Guard clauses](https://www.youtube.com/watch?v=9cr7grNWn6c). 

I am in the process of trying to create a Kanban board to explore [Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html) and [Domain-Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design) and learn F#. Here I need an Id on my `Board` type, to put on Events and pass around. 

### In-depth motivation

You might be able to skip this part. These are just my personal key points from the articles linked above. Reasons to have a type-strong id:

- You don't want to be able to pass a `ColumnId` in a context where a `BoardId` is required.
- Whether a `BoardId` is a `Guid`, `int`, `string`, or something else, is an implementation detail. Any consumer of the domain doesn't need/want to know. 
- It improves readability. It's quite obvious what kind of Id a method takes if it is strongly typed. 

- If it's an int, you probably don't want to allow a negative integer. Moving this validation into a `BoardId` type is nice.

## The Solution

### Initial (WRONG) solution
I initially expected to be able to do this. 


```f#
type BoardId = Guid
```

It seemed like it would work initially, however you can then cast them back and forth implicitly. This is simply a [type alias](https://fsharpforfunandprofit.com/posts/type-abbreviations/), which doesn't solve any problems regarding actual encapsulation, misuse or validation.

## The (Minimal) Solution

After searching for some time I found [this StackOverflow post](https://stackoverflow.com/questions/56235474/strongly-typed-ids-in-f)
```fsharp
[<Struct>]
type ProductId = ProductId of Guid
```

### The (Better) Solution
Improving a bit on their result (which included helper methods), I've added additional helpers and made this a bit stronger, covering the use cases that I had. 

```fsharp
namespace Fanban.Domain

open System

[<Struct>]
type BoardId =
    private
    | BoardId of Guid

    static member New() = BoardId(Guid.NewGuid())
    static member Parse (value: string) = BoardId(Guid.Parse(value))
    static member TryParse (value: string) =
        let couldParse, result = Guid.TryParse(value)
        if couldParse then Some (BoardId result) else None
    member this.Value = let (BoardId i) = this in i
    override this.ToString() = this.Value.ToString()
```

This makes it possible to write code like:

```fsharp
let CreateBoardEvent name (columns: ColumnName list) =
    { Id = BoardId.New()
      Name = name
      ColumnNames = columns }

```

and types can define the id type very explicitly:

```fsharp
and SetBoardNameEvent =
    { BoardId: BoardId
      Name: string }
```
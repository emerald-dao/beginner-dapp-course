# Chapter 4 Day 3 - Passing in Arguments to a Script

Yesterday we learned how to execute a script with FCL. Today, we're going to learn how to pass in arguments to the script.

## Videos

If you watch this video, it will help you understand the content for today. Note that they are different, but the concept is the same: https://www.youtube.com/watch?v=w9UFsv5wqo4

## Important Note

1. None of today's material will involve changing our Emerald DApp. This is a standalone lesson to help you understand arguments.
2. All of what you learn today is **the exact same for transactions**. We will be using transactions tomorrow ;)

## Quick Overview of Yesterday

Yesterday, we made executed a script using FCL like this:

```javascript
async function executeScript() {
  const response = await fcl.query({
    cadence: `
    import HelloWorld from 0x90250c4359cebac7 // THIS WAS MY ADDRESS, USE YOURS

    pub fun main(): String {
        return HelloWorld.greeting
    }
    `,
    args: (arg, t) => [] // ARGUMENTS GO IN HERE
  })

  console.log("Response from our script: " + response);
}
```

But one missing piece was the `args` property. Why didn't we put anything there? Well, our Cadence code didn't take any arguments. If you look at the Cadence code...

```cadence
pub fun main(): String
```

...you'll see it doesn't take any arguments. But what if we had a Cadence script like this?

```cadence
pub fun main(a: Int, b: Int): Int {
  // Example:
  // a = 2
  // b = 3
  // result = 5
  return a + b
}
```

Or something like this?

```cadence
pub fun main(greeting: String, person: String): String {
  // Example:
  // greeting = "Hello"
  // person = "Jacob"
  // result = "Hello, Jacob!"
  return greeting.concat(", ").concat(person).concat("!")
}
```

Now we need to pass in arguments.

## Passing in Arguments Using FCL

Here is an example of passing in arguments, and then we will explain...

```javascript
async function executeScript() {
  const response = await fcl.query({
    cadence: `
    pub fun main(a: Int, b: Int): Int {
      // Example:
      // a = 2
      // b = 3
      // result = 5
      return a + b
    }
    `,
    args: (arg, t) => [
      arg("2", t.Int),
      arg("3", t.Int)
    ]
  })
}
```

Now, there's a few things we should talk about to help you understand:
1. All the arguments go inside the `[]`. 
2. You create a new argument with the `arg` keyword
3. You put the value of the argument first (ex. `"2"`)
4. You put the Cadence type of the value 2nd using `t` (ex. `t.Int`)

You may be wondering, why are our Integers represented as strings? The answer is, I have no idea. The FCL team just wanted it like that.

### Different Types

Let's look at another example using tons of different types:

```javascript
async function executeScript() {
  const response = await fcl.query({
    cadence: `
    pub fun main(
      a: Int, 
      b: String, 
      c: UFix64, 
      d: Address, 
      e: Bool,
      f: String?,
      g: [Int],
      h: {String: Address}
    ) {
      // Example:
      // a = 2
      // b = "Jacob is so cool"
      // c = 5.0
      // d = 0x6c0d53c676256e8c
      // e = true
      // f = nil
      // g = [1, 2, 3]
      // h = {"FLOAT": 0x2d4c3caffbeab845, "EmeraldID": 0x39e42c67cc851cfb}

      // something happens here... but it doesn't matter
    }
    `,
    args: (arg, t) => [
      arg("2", t.Int),
      arg("Jacob is so cool", t.String),
      arg("5.0", t.UFix64),
      arg("0x6c0d53c676256e8c", t.Address),
      arg(true, t.Bool),
      arg(null, t.Optional(t.String)),
      arg([1, 2, 3], t.Array(t.Int)),
      arg(
        [
          { key: "FLOAT", value: "0x2d4c3caffbeab845" },
          { key: "EmeraldID", value: "0x39e42c67cc851cfb" }
        ], 
        t.Dictionary({ key: t.String, value: t.Address })
      )
    ]
  })
}
```

Hopefully this helps you understand the different types of arguments you can pass in :)

## Conclusion

That's it! Not so bad, right?

## Quests

I only have one quest for you today, and it's almost exactly what we reviewed already :)

1. Write a function that executes a script with all the Cadence types that we reviewed today. Call the script when the page refreshes. Return something random from the Cadence script, and console log it to prove to me your script actually worked.

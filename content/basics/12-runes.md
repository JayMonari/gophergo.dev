---
Lesson: 12
Title: "Runes"
Draft: false
---

Maybe it's the nerd in me ð¤ but when I hear `rune` my mind goes to the
dragon-born ð² **FUS-RO-DAH!!** ð¬ï¸ Just me? ð anyways, time to level up â¬ï¸  on
our `rune` game.

It **cannot** be overstated. A `rune` is an `int32`. Plain and simple. The
`type` is there to help guide us into understanding it is an `int32`
specifically for strings, but it's an `int32`, yeah? Well, ð¤ Maybe we can step
back and find out what it means to be `int32` and then understand how runes fit
into that. ð¡ Before that lets take a look at a `byte` _or_ a `uint8`.

## One Byte At A Time ð¹

A `uint8` or unsigned integer 8 is called that because it has 8 slots and
_all_ of them are for values. If it was signed that means that one of the slots
would be `0` for positive or `1` for negative. Lets look at `uint8` through the
eyes ðï¸ðï¸ of a computer

`0000 0000`

Note that I separated it into groups of 4 because it's easier to read, but a
computer ð¥ï¸ crams them all together.

We have 8 slots and only 2 possible numbers that go in those 8 slots,
either `0` or `1`. This is known as binary (base 2). We ð¤PUNYð¤ humans use
decimal (base 10).

With those 8 places, ð¤ how many numbers can we represent in decimal? Well
let's do something easier, in decimal if I gave you `0` how many different
numbers could you fit in that one slot? 0,1,2,3,4,5,6,7,8,9 -- Ten! -- You
could fit ten in decimal. OK, now `00` 0,1,2,3,4...97,98,99 -- One Hundred! --
Alright one more time with `000`, how many different numbers could you fit?
0,1,2,3,4,5...997,998,999 -- One Thousand!

ð¤ Hmm, I think I'm seeing a pattern, don't you? We went from 10 to 100 to
1000. It looks like were adding another zero every time, but how do we express
that with math? Well it was `10` then it was `10 * 10` then it was `10 * 10 *
10` and instead of repeating that we can raise 10 to a power. {{< topower 10 1
>}} {{< topower 10 2 >}} {{< topower 10 3 >}} we can raise 10 to a power of
_how many digits we have_.

Back to our original question, there are **2** values and **8** slots, how many
numbers can we fit in a `byte`? {{< topower 2 8 >}} ð and what's _that_
number? ð¤¨ Well funny you ask because the [Extended Ascii
Table](https://www.asciitable.com/) has **the exact amount** that a `byte` can
represent {{< topower 2 8 >}} or 256 values. ð¤¯ So that means **everything**
not in those 256 values (0 is a value, that's why it only goes to 255)
**cannot** be represented with a `byte` or a `uint8`, not even emojis ð¢

## Setup

Let's make our directory `runes` and the files we want inside of that directory
`example_test.go` `runes.go`

```sh
mkdir runes
touch runes/example_test.go runes/runes.go
```

Now let's open up `runes.go` and for the very first line we'll add
```go
package runes
```
Next for `example_test.go` for the very first line we'll add
```go
package runes_test
```

## Deepen Understanding of Runes

So again a `rune` is just `int32` meaning it looks like 4 times the size of a
`byte` or a `uint8`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1ï¸â£&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2ï¸â£&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3ï¸â£&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4ï¸â£

`0000 0000 0000 0000 0000 0000 0000 0000`

ð That's a lot of zeroes! Yes, 32 in fact ð So now we can see that a `rune`
is an `int32` which is also the same as four `uint8` _or_ four `byte`

OK... So it's just a number, but what number makes up a letter? Like `a` maybe?
Good question! ð Let's represent it in both `byte` and `rune` format.

It's not a big deal to understand all this stuff at once and if what you see
confuses you, come back at a later time or look at other resources who might do
a better job explaining, it's always good to have other inputs to fill in the
gaps.

The letter `a` according to the [Extended Ascii
Table](https://www.asciitable.com/) is `97` in decimal. So the `byte` would
look like

`0110 0001`

`7654 3210` Slots to insert `0` or `1`

{{< topower 2 6 >}}(64) + {{< topower 2 5 >}}(32) + {{< topower 2 0 >}}(1) = 97

I got that from slot 6 + slot 5 + slot 0

And there you go, your very first look at what a computer sees.... Aren't you
glad we have `byte` and `rune` so we can just write `'a'`? I know I am ð
Speaking of `rune` it would look the same, just with a lot more wasted space.

`0000 0000 0000 0000 0000 0000 0110 0001`

and finally it should now make sense why we're allowed to compare "letters" to
"letters", because they aren't letters, it's all `int`. (Wait, it's all ints?ð²
Always has been ð«ð§âð )

### Coding Time!

![https://twitter.com/egonelbre](/egon-elbre/gopher-heart-eyes.png "gopher-heart-eyes by Egon Elbre")

#### `runes.go`

```go
// ByteAndRuneAreInt shows that we can compare characters to their integer
// counterpart in any common base we want, e.g. binary, octal, decimal,
// hexadecimal, or even Unicode code point. And that `byte` fits in `rune`
// making it a smaller version of a `rune`.
func ByteAndRuneAreInt() {
	var myByte byte = 'a'
	if myByte == 'a' {
		fmt.Println("It's a")
	}
	if myByte == 97 {
		fmt.Printf("dec: %d is %c\n", myByte, myByte)
	}
	if myByte == 0b01100001 {
		fmt.Printf("bin: %b is %c\n", myByte, myByte)
	}
	if myByte == 0o141 {
		fmt.Printf("oct: %o is %c\n", myByte, myByte)
	}
	if myByte == 0x61 {
		fmt.Printf("hex: %x is %c\n", myByte, myByte)
	}
	if myByte == []byte("\u0061")[0] {
		fmt.Printf("Unicode: %#U is %c\n", myByte, myByte)
	}
	myRune := rune(myByte)
	if myRune == 'a' {
		fmt.Println("It's a")
	}
	if myRune == 97 {
		fmt.Printf("dec: %d is %c\n", myRune, myRune)
	}
	if myRune == 0b01100001 {
		fmt.Printf("bin: %b is %c\n", myRune, myRune)
	}
	if myRune == 0o141 {
		fmt.Printf("oct: %o is %c\n", myRune, myRune)
	}
	if myRune == 0x61 {
		fmt.Printf("hex: %x is %c\n", myRune, myRune)
	}
	if myRune == rune([]byte("\u0061")[0]) {
		fmt.Printf("Unicode: %#U is %c\n", myRune, myRune)
	}
}
```

#### `example_test.go`

```go
func ExampleByteAndRuneAreInt() {
	runes.ByteAndRuneAreInt()
	// Output:
	// It's a
	// dec: 97 is a
	// bin: 1100001 is a
	// oct: 141 is a
	// hex: 61 is a
	// Unicode: U+0061 'a' is a
	// It's a
	// dec: 97 is a
	// bin: 1100001 is a
	// oct: 141 is a
	// hex: 61 is a
	// Unicode: U+0061 'a' is a
}
```

## Byte Count

We just had our lesson on [range](/basics/11-range/#range-through-string) and
it shows that we will get `rune` types when we `range` through a `string`, so
what happens when we do a normal `for` loop? We get `byte` types instead, this
is usually bad for us ð¬ because we want to count the amount of characters,
_not_ the amount of space they take up.

### Coding Time!

![https://twitter.com/egonelbre](/egon-elbre/gopher-heart-eyes.png "gopher-heart-eyes by Egon Elbre")

#### `runes.go`

```go
// ByteCount shows how to get the amount of bytes in a string and using a for
// loop won't work if the bytes do not fall in the ASCII Table.
// For the complete table you can visit https://asciitable.com
func ByteCount() {
	fmt.Printf("Len %s: %d\n", helloWorldHindi, len(helloWorldHindi))
	fmt.Printf("Len %s: %d\n", helloWorldRussian, len(helloWorldRussian))
	fmt.Printf("Len %s: %d\n", helloWorldJapanese, len(helloWorldJapanese))
	fmt.Println()
	for i := 0; i < len(helloWorldJapanese); i++ {
		fmt.Printf("Byte in hexadecimal: %x and decimal: %d and as string: %s\n",
			helloWorldJapanese[i], helloWorldJapanese[i], string(helloWorldJapanese[i]))
	}
}
```

#### `example_test.go`

â ï¸ Attention â ï¸ Some lines look like they end in a blank space `string: ` but
it's a non-printing ASCII character. It would be best to copy and paste this
output into your test or you're very likely going to have problems. This is why
we represent things as `rune` to avoid such problems ð¤

```go
func ExampleByteCount() {
	runes.ByteCount()
	// Output:
	// Len à¤¨à¤®à¤¸à¥à¤¤à¥ à¤¦à¥à¤¨à¤¿à¤¯à¤¾: 37
	// Len ÐÑÐ¸Ð²ÐµÑ Ð¼Ð¸Ñ: 19
	// Len ããã«ã¡ã¯ä¸ç: 21
	//
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: 93 and decimal: 147 and as string: Â
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 82 and decimal: 130 and as string: Â
	// Byte in hexadecimal: 93 and decimal: 147 and as string: Â
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: ab and decimal: 171 and as string: Â«
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: a1 and decimal: 161 and as string: Â¡
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: af and decimal: 175 and as string: Â¯
	// Byte in hexadecimal: e4 and decimal: 228 and as string: Ã¤
	// Byte in hexadecimal: b8 and decimal: 184 and as string: Â¸
	// Byte in hexadecimal: 96 and decimal: 150 and as string: Â
	// Byte in hexadecimal: e7 and decimal: 231 and as string: Ã§
	// Byte in hexadecimal: 95 and decimal: 149 and as string: Â
	// Byte in hexadecimal: 8c and decimal: 140 and as string: Â
}
```

## Rune Count

Now we'll see how we can get what we're probably aiming ð¯ for, the amount of
characters (`rune`) in a `string`. It's important to note that we bring in
a package from the standard library, `unicode/utf8` that will get the amount of
runes in the string.

### Coding Time!

![https://twitter.com/egonelbre](/egon-elbre/gopher-heart-eyes.png "gopher-heart-eyes by Egon Elbre")

#### `runes.go`

```go
import (
  "fmt"
  "unicode/utf8"
)

const helloWorldHindi = "à¤¨à¤®à¤¸à¥à¤¤à¥ à¤¦à¥à¤¨à¤¿à¤¯à¤¾"
const helloWorldRussian = "ÐÑÐ¸Ð²ÐµÑ Ð¼Ð¸Ñ"
const helloWorldJapanese = "ããã«ã¡ã¯ä¸ç"

// RuneCount shows how to get the actual count of characters (runes) in a
// string and how to loop through a string and get the runes using the range
// keyword.
func RuneCount() {
	fmt.Printf("Rune count in %s: %d\n",
		helloWorldHindi, utf8.RuneCountInString(helloWorldHindi))
	fmt.Printf("Rune count in %s: %d\n",
		helloWorldRussian, utf8.RuneCountInString(helloWorldRussian))
	fmt.Printf("Rune count in %s: %d\n",
		helloWorldJapanese, utf8.RuneCountInString(helloWorldJapanese))
	fmt.Println()
	for i, r := range helloWorldHindi {
		fmt.Printf("Rune verbose unicode value: %#U Its index: %d As a string %q\n",
			r, i, string(r))
	}
}
```

#### `example_test.go`

```go
func ExampleRuneCount() {
	runes.RuneCount()
	// Output:
	// Rune count in à¤¨à¤®à¤¸à¥à¤¤à¥ à¤¦à¥à¤¨à¤¿à¤¯à¤¾: 13
	// Rune count in ÐÑÐ¸Ð²ÐµÑ Ð¼Ð¸Ñ: 10
	// Rune count in ããã«ã¡ã¯ä¸ç: 7
	//
	// Rune verbose unicode value: U+0928 'à¤¨' Its index: 0 As a string "à¤¨"
	// Rune verbose unicode value: U+092E 'à¤®' Its index: 3 As a string "à¤®"
	// Rune verbose unicode value: U+0938 'à¤¸' Its index: 6 As a string "à¤¸"
	// Rune verbose unicode value: U+094D 'à¥' Its index: 9 As a string "à¥"
	// Rune verbose unicode value: U+0924 'à¤¤' Its index: 12 As a string "à¤¤"
	// Rune verbose unicode value: U+0947 'à¥' Its index: 15 As a string "à¥"
	// Rune verbose unicode value: U+0020 ' ' Its index: 18 As a string " "
	// Rune verbose unicode value: U+0926 'à¤¦' Its index: 19 As a string "à¤¦"
	// Rune verbose unicode value: U+0941 'à¥' Its index: 22 As a string "à¥"
	// Rune verbose unicode value: U+0928 'à¤¨' Its index: 25 As a string "à¤¨"
	// Rune verbose unicode value: U+093F 'à¤¿' Its index: 28 As a string "à¤¿"
	// Rune verbose unicode value: U+092F 'à¤¯' Its index: 31 As a string "à¤¯"
	// Rune verbose unicode value: U+093E 'à¤¾' Its index: 34 As a string "à¤¾"
}
```

## What does Range do?

Let's peel back some of the magic ð of what `range` is doing for us in place
of a regular `for` loop. Remember we established that a `rune` is `int32` which
would be four bytes or four `int8` (8 + 8 + 8 + 8 = 32). So depending on how
big the `rune` is depends on how many bytes you need. Well the `unicode/utf8`
package has a function just for that!

### Coding Time!

![https://twitter.com/egonelbre](/egon-elbre/gopher-heart-eyes.png "gopher-heart-eyes by Egon Elbre")

#### `runes.go`

```go
// ByteToRuneForLoop shows what the range keyword does by counting the amount
// of bytes and moving forward by that width
func ByteToRuneForLoop() {
	for i, width := 0, 0; i < len(helloWorldRussian); i += width {
		r, w := utf8.DecodeRuneInString(helloWorldRussian[i:])
		fmt.Printf("Rune verbose unicode value: %#U Its index: %d As a string %q\n",
			r, i, string(r))
		width = w
	}
}
```

#### `example_test.go`

```go
func ExampleByteToRuneForLoop() {
	runes.ByteToRuneForLoop()
	// Output:
	// Rune verbose unicode value: U+041F 'Ð' Its index: 0 As a string "Ð"
	// Rune verbose unicode value: U+0440 'Ñ' Its index: 2 As a string "Ñ"
	// Rune verbose unicode value: U+0438 'Ð¸' Its index: 4 As a string "Ð¸"
	// Rune verbose unicode value: U+0432 'Ð²' Its index: 6 As a string "Ð²"
	// Rune verbose unicode value: U+0435 'Ðµ' Its index: 8 As a string "Ðµ"
	// Rune verbose unicode value: U+0442 'Ñ' Its index: 10 As a string "Ñ"
	// Rune verbose unicode value: U+0020 ' ' Its index: 12 As a string " "
	// Rune verbose unicode value: U+043C 'Ð¼' Its index: 13 As a string "Ð¼"
	// Rune verbose unicode value: U+0438 'Ð¸' Its index: 15 As a string "Ð¸"
	// Rune verbose unicode value: U+0440 'Ñ' Its index: 17 As a string "Ñ"
}
```

## The Whole File

![https://twitter.com/egonelbre](/egon-elbre/gopher-victorious.png "gopher-victorious by Egon Elbre")

```go
package runes

import (
	"fmt"
	"unicode/utf8"
)

const helloWorldHindi = "à¤¨à¤®à¤¸à¥à¤¤à¥ à¤¦à¥à¤¨à¤¿à¤¯à¤¾"
const helloWorldRussian = "ÐÑÐ¸Ð²ÐµÑ Ð¼Ð¸Ñ"
const helloWorldJapanese = "ããã«ã¡ã¯ä¸ç"

// ByteAndRuneAreInt shows that we can compare characters to their integer
// counterpart in any common base we want, e.g. binary, octal, decimal,
// hexadecimal, or even Unicode code point. And that `byte` fits in `rune`
// making it a smaller version of a `rune`.
func ByteAndRuneAreInt() {
	var myByte byte = 'a'
	if myByte == 'a' {
		fmt.Println("It's a")
	}
	if myByte == 97 {
		fmt.Printf("dec: %d is %c\n", myByte, myByte)
	}
	if myByte == 0b01100001 {
		fmt.Printf("bin: %b is %c\n", myByte, myByte)
	}
	if myByte == 0o141 {
		fmt.Printf("oct: %o is %c\n", myByte, myByte)
	}
	if myByte == 0x61 {
		fmt.Printf("hex: %x is %c\n", myByte, myByte)
	}
	if myByte == []byte("\u0061")[0] {
		fmt.Printf("Unicode: %#U is %c\n", myByte, myByte)
	}
	myRune := rune(myByte)
	if myRune == 'a' {
		fmt.Println("It's a")
	}
	if myRune == 97 {
		fmt.Printf("dec: %d is %c\n", myRune, myRune)
	}
	if myRune == 0b01100001 {
		fmt.Printf("bin: %b is %c\n", myRune, myRune)
	}
	if myRune == 0o141 {
		fmt.Printf("oct: %o is %c\n", myRune, myRune)
	}
	if myRune == 0x61 {
		fmt.Printf("hex: %x is %c\n", myRune, myRune)
	}
	if myRune == rune([]byte("\u0061")[0]) {
		fmt.Printf("Unicode: %#U is %c\n", myRune, myRune)
	}
}

// ByteCount shows how to get the amount of bytes in a string and using a for
// loop won't work if the bytes do not fall in the ASCII Table.
// For the complete table you can visit https://asciitable.com
func ByteCount() {
	fmt.Printf("Len %s: %d\n", helloWorldHindi, len(helloWorldHindi))
	fmt.Printf("Len %s: %d\n", helloWorldRussian, len(helloWorldRussian))
	fmt.Printf("Len %s: %d\n", helloWorldJapanese, len(helloWorldJapanese))
	fmt.Println()
	for i := 0; i < len(helloWorldJapanese); i++ {
		fmt.Printf("Byte in hexadecimal: %x and decimal: %d and as string: %s\n",
			helloWorldJapanese[i], helloWorldJapanese[i], string(helloWorldJapanese[i]))
	}
}

// RuneCount shows how to get the actual count of characters (runes) in a
// string and how to loop through a string and get the runes using the range
// keyword.
func RuneCount() {
	fmt.Printf("Rune count in %s: %d\n",
		helloWorldHindi, utf8.RuneCountInString(helloWorldHindi))
	fmt.Printf("Rune count in %s: %d\n",
		helloWorldRussian, utf8.RuneCountInString(helloWorldRussian))
	fmt.Printf("Rune count in %s: %d\n",
		helloWorldJapanese, utf8.RuneCountInString(helloWorldJapanese))
	fmt.Println()
	for i, r := range helloWorldHindi {
		fmt.Printf("Rune verbose unicode value: %#U Its index: %d As a string %q\n",
			r, i, string(r))
	}
}

// ByteToRuneForLoop shows what the range keyword does by counting the amount
// of bytes and moving forward by that width
func ByteToRuneForLoop() {
	for i, width := 0, 0; i < len(helloWorldRussian); i += width {
		r, w := utf8.DecodeRuneInString(helloWorldRussian[i:])
		fmt.Printf("Rune verbose unicode value: %#U Its index: %d As a string %q\n",
			r, i, string(r))
		width = w
	}
}
```

## All Of The Outputs To Our Examples

![https://twitter.com/egonelbre](/egon-elbre/typing-furiously.gif "typing-furiously by Egon Elbre")

```go
package runes_test

import "basics/runes"

func ExampleByteAndRuneAreInt() {
	runes.ByteAndRuneAreInt()
	// Output:
	// It's a
	// dec: 97 is a
	// bin: 1100001 is a
	// oct: 141 is a
	// hex: 61 is a
	// Unicode: U+0061 'a' is a
	// It's a
	// dec: 97 is a
	// bin: 1100001 is a
	// oct: 141 is a
	// hex: 61 is a
	// Unicode: U+0061 'a' is a
}

func ExampleByteCount() {
	runes.ByteCount()
	// Output:
	// Len à¤¨à¤®à¤¸à¥à¤¤à¥ à¤¦à¥à¤¨à¤¿à¤¯à¤¾: 37
	// Len ÐÑÐ¸Ð²ÐµÑ Ð¼Ð¸Ñ: 19
	// Len ããã«ã¡ã¯ä¸ç: 21
	//
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: 93 and decimal: 147 and as string: Â
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 82 and decimal: 130 and as string: Â
	// Byte in hexadecimal: 93 and decimal: 147 and as string: Â
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: ab and decimal: 171 and as string: Â«
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: a1 and decimal: 161 and as string: Â¡
	// Byte in hexadecimal: e3 and decimal: 227 and as string: Ã£
	// Byte in hexadecimal: 81 and decimal: 129 and as string: Â
	// Byte in hexadecimal: af and decimal: 175 and as string: Â¯
	// Byte in hexadecimal: e4 and decimal: 228 and as string: Ã¤
	// Byte in hexadecimal: b8 and decimal: 184 and as string: Â¸
	// Byte in hexadecimal: 96 and decimal: 150 and as string: Â
	// Byte in hexadecimal: e7 and decimal: 231 and as string: Ã§
	// Byte in hexadecimal: 95 and decimal: 149 and as string: Â
	// Byte in hexadecimal: 8c and decimal: 140 and as string: Â
}

func ExampleRuneCount() {
	runes.RuneCount()
	// Output:
	// Rune count in à¤¨à¤®à¤¸à¥à¤¤à¥ à¤¦à¥à¤¨à¤¿à¤¯à¤¾: 13
	// Rune count in ÐÑÐ¸Ð²ÐµÑ Ð¼Ð¸Ñ: 10
	// Rune count in ããã«ã¡ã¯ä¸ç: 7
	//
	// Rune verbose unicode value: U+0928 'à¤¨' Its index: 0 As a string "à¤¨"
	// Rune verbose unicode value: U+092E 'à¤®' Its index: 3 As a string "à¤®"
	// Rune verbose unicode value: U+0938 'à¤¸' Its index: 6 As a string "à¤¸"
	// Rune verbose unicode value: U+094D 'à¥' Its index: 9 As a string "à¥"
	// Rune verbose unicode value: U+0924 'à¤¤' Its index: 12 As a string "à¤¤"
	// Rune verbose unicode value: U+0947 'à¥' Its index: 15 As a string "à¥"
	// Rune verbose unicode value: U+0020 ' ' Its index: 18 As a string " "
	// Rune verbose unicode value: U+0926 'à¤¦' Its index: 19 As a string "à¤¦"
	// Rune verbose unicode value: U+0941 'à¥' Its index: 22 As a string "à¥"
	// Rune verbose unicode value: U+0928 'à¤¨' Its index: 25 As a string "à¤¨"
	// Rune verbose unicode value: U+093F 'à¤¿' Its index: 28 As a string "à¤¿"
	// Rune verbose unicode value: U+092F 'à¤¯' Its index: 31 As a string "à¤¯"
	// Rune verbose unicode value: U+093E 'à¤¾' Its index: 34 As a string "à¤¾"
}

func ExampleByteToRuneForLoop() {
	runes.ByteToRuneForLoop()
	// Output:
	// Rune verbose unicode value: U+041F 'Ð' Its index: 0 As a string "Ð"
	// Rune verbose unicode value: U+0440 'Ñ' Its index: 2 As a string "Ñ"
	// Rune verbose unicode value: U+0438 'Ð¸' Its index: 4 As a string "Ð¸"
	// Rune verbose unicode value: U+0432 'Ð²' Its index: 6 As a string "Ð²"
	// Rune verbose unicode value: U+0435 'Ðµ' Its index: 8 As a string "Ðµ"
	// Rune verbose unicode value: U+0442 'Ñ' Its index: 10 As a string "Ñ"
	// Rune verbose unicode value: U+0020 ' ' Its index: 12 As a string " "
	// Rune verbose unicode value: U+043C 'Ð¼' Its index: 13 As a string "Ð¼"
	// Rune verbose unicode value: U+0438 'Ð¸' Its index: 15 As a string "Ð¸"
	// Rune verbose unicode value: U+0440 'Ñ' Its index: 17 As a string "Ñ"
}
```

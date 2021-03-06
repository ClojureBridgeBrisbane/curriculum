Flow Control
============

"Flow control" is the programming term for deciding how to react to a given circumstance. We make decisions like this all the time. *If* it's a nice day out, *then* we should visit the park; *otherwise* we should stay inside and play board games. *If* your car's tank is empty, *then* you should visit a gas station; *otherwise* you should continue to your destination.

Software is also full of these decisions. *If* the user's input is valid, *then* we should save her data; *otherwise* we show an error message. The common pattern here is that you test some condition and react differently based on whether the condition is *true* or *false*.

## if

In Clojure, the most basic tool we have for this process is the `if` operator. Here's how you might code the data validation scenario:

```clojure
(if (valid? data)
  (save! data)
  (error "Your data was invalid"))
```

The general form of the `if` operator is:

```clojure
(if conditional-expression
  expression-to-evaluate-when-true
  expression-to-evaluate-when-false)
```

When testing the truth of an expression, Clojure considers the values `nil` and `false` to be false and everything else to be true. Here are some examples:

```clojure
(if (> 3 1)
  "3 is greater than 1"
  "3 is not greater than 1")
;=> "3 is greater than 1"

(if (> 1 3)
  "1 is greater than 3"
  "1 is not greater than 3")
;=> "1 is not greater than 3"

(if "anything other than nil or false is considered true"
  "A string is considered true"
  "A string is not considered true")
;=> "A string is considered true"

(if nil
  "nil is considered true"
  "nil is not considered true")
;=> "nil is not considered true"

(if (get {:a 1} :b)
  "expressions which evaluate to nil are considered true"
  "expressions which evaluate to nil are not considered true")
;=> "expressions which evaluate to nil are not considered true"
```

### EXERCISE: Even more name formatting

Write a function `format-name` that takes a map representing a user, with keys `:first`, `:last`, and possibly `:middle`. It should return their name as a string, like so:

```clj
(format-name {:first "Margaret" :last "Atwood"})
;=> "Margaret Atwood"

(format-name {:first "Ursula" :last "Le Guin" :middle "K."})
;=> "Ursula K. Le Guin"
```

### BONUS: Flexible name formatting

Change `format-name` to take a second argument, `order`. If `order` equals `:last`, then the format should be "Last, First Middle"; otherwise, it should be "First Middle Last."


## Boolean logic with and, or, and not

`if` statements are not limited to testing only one thing. You can test multiple conditions using boolean logic. _Boolean logic_ refers to combining and changing the results of predicates using `and`, `or`, and `not`.

If you've never seen this concept in programming before, remember that it follows the common sense way you look at things normally. Is this _and_ that true? Only if both are true. Is this _or_ that true? Yes, if either -- or both! -- are. Is this _not_ true? Yes, if it's false.

Look at this truth table:

| x     | y     | (and x y) | (or x y) | (not x) | (not y) |
| ----- | ----- | --------- | -------- | ------- | ------- |
| false | false | false | false | true  | true  |
| true  | false | false | true  | false | true  |
| true  | true  | true  | true  | false | false |
| false | true  | false | true  | true  | false |

`and`, `or`, and `not` work like other functions (they aren't exactly functions, but work like them), so they are in _prefix notation_, like we've seen with arithmetic.

`and`, `or`, and `not` can be combined. This can be hard to read. Here's an example:

```clj
(defn leap-year?
  "Every four years, except years divisible by 100, but yes for years divisible by 400."
  [year]
  (and (zero? (mod year 4))
       (or (zero? (mod year 400))
           (not (zero? (mod year 100))))))
```

## List comprehensions with `for`

In many programming languages, you do something to every member of a sequence by using a language construct called `for` (or some variant on that.) In those languages, `for` iterates over the sequence and has a body that does something with each value in the sequence. 

Clojure also has a `for` statement, but it works a little differently. It also iterates over a sequence, but it also returns a new sequence based on the body of the for statement.

With just one sequence given to the `for` statement, it works like `map`, which we saw previously.

```clojure
(for [x [1 2 3]]
  (* x x))
;;=> (1 4 9)

(map (fn [x] (* x x)) [1 2 3])
;;=> (1 4 9)
```

`for` can do even more, though! It takes any number of sequences, and iterates over all the combinations of the sequences it's given and returns a sequence:

```clojure
(for [x [1 2 3]
      y ["a" "b" "c"]]
  (str x y))
; => ("1a" "1b" "1c" "2a" "2b" "2c" "3a" "3b" "3c")
```

You can also specify what combinations are allowed with the `:when` keyword. This uses boolean logic to decide which combinations are valid.

```clojure
(for [x [1 2 3]
      y ["a" "b" "c"]
      :when (> x 2)]
  (str x y))
; => ("3a" "3b" "3c")

(for [x [1 2 3]
      y ["a" "b" "c"]
      :when (and (> x 2) (not= y "a"))]
  (str x y))
; => ("3b" "3c")
```

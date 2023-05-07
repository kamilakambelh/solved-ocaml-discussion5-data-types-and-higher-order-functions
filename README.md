Download Link: https://assignmentchef.com/product/solved-ocaml-discussion5-data-types-and-higher-order-functions
<br>
## Introduction

This exercise consists of a few short functions to help you familiarize yourself with OCaml. You can review the content of the discussion in [notes.rb](notes.rb), which includes the topics and examples from the discussion video.

### Testing &amp; Submitting

You will submit this project to [Gradescope](https://www.gradescope.com/courses/171498/assignments/718825). You may only submit the **disc5.ml** file. To test locally, run `dune runtest -f`.

## Part 1: Currying and Partial Applications

These function are best implemented using the concepts of currying and partial application. While you are not _required_ to implement them in this way, it is **_highly_** recommended.

#### `mul_n n lst`

– **Type**: `int -&gt; int list -&gt; int list`– **Description**: Given an int list, multiply every element in the list by `n`– **Examples**:“`ocamlmul_n 3 [1;2;3] = [3; 6; 9]mul_n 5 [1;2;3] = [5;10;15]mul_n 0 [1;2;3] = [0;0;0]mul_n 10 [] = []“`

#### `join strs sep`

– **Type**: `string list -&gt; string -&gt; string`– **Description**: Given a list of strings and a separator, return a new string that is each string concatenated with the separator inbetween them.– **Examples**:“`ocamljoin [“a”;”b”;”c”] “, ” = “a, b, c”;join [“coffee”; “pizza”; “water”] ” and ” = “coffee and pizza and water”“`

## Part 2: Option Functions

#### `list_of_option o`

– **Type**: `’a option -&gt; ‘a list`– **Description**: Take in an option and return `[]` of the option is `None` or `[a]` if the option is `Some a`.– **Examples**:“`ocamllist_of_option None = []list_of_option (Some 1) = [1]list_of_option (Some (Some 3)) = [Some 3]“`

#### `match_key k p`

– **Type**: `’k -&gt; ‘k*’v -&gt; ‘v option`– **Description**: If the pair `p`’s key matches `k` returns the value as an option. Otherwise return `None`– **Examples**:“`ocamlmatch_key 1 (1, “str”) = Some “str”match_key 2 (1, “str”) = Nonematch_key “key” (“key”, “pizza”) = Some “pizza”match_key “not key” (“key”, “pizza”) = None“`

## Part 3: LengthLists

For this part, you will be working with the `lengthlist` type. This type is meant to represent the idea of _run length encoding_ – essentially, compressing a list by grouping terms together if there are _multiple_ of the same term in a row. For example, the list `[1;1;1;1;2;2;2;3;3;4]` can be represented as, for example, using tuples of the form `(value, count)`, `[(1,4); (2, 3); (3, 2); (4, 1)]`.

The `’a lengthlist` type encodes this idea, while keeping the linkedlist structure of the default OCaml list. The type is defined as in below:

“`ocamltype ‘a lengthlist =Cons of (‘a * int * ‘a lengthlist)| Empty“`

Now, we would represent the above list as `Cons(1, 4, Cons(2, 3, Cons(3, 2, Cons(4, 1, Empty))))`. We ask you to implement the following functions for length lists.

#### `length_to_list llst`

– **Type**: `’a lengthlist -&gt; ‘a list`– **Description**: Converts a lengthlist into its equivalent Ocaml list.– **Examples**:“`ocamllengthlist Cons(1, 4, Cons(2, 3, Cons(3, 2, Cons(4, 1, Empty)))) = [1;1;1;1;2;2;2;3;3;4]lengthlist Empty = []lengthlist Cons(“hi”, 2, Cons(“hello”, 3, Empty)) = [“hi”; “hi”; “hello”; “hello”; “hello”]“`

#### `map fn llst`

– **Type**: `(‘a-&gt;’b) -&gt; ‘a lengthlist -&gt; ‘b lengthlist`– **Description**: Recreate the map function, but for lengthlists. Note that we only operate on the elements, and not the number of each element.– **Examples**:“`ocamlmap (+ 3) Cons(1, 4, Cons(2, 3, Cons(3, 2, Cons(4, 1, Empty)))) = Cons(4, 4, Cons(5, 3, Cons(6, 2, Cons(7, 1, Empty))))map string_of_int Cons(1, 4, Cons(2, 3, Cons(3, 2, Cons(4, 1, Empty)))) = Cons(“1:, 4, Cons(“2”, 3, Cons(“3”, 2, Cons(“4”, 1, Empty))))“`

#### `decrement_count llst`

– **Type**: `’a lengthlist -&gt; ‘a lengthlist`– **Description**: Remove one of each element in the lengthlist. If the number of an element decreases to 0, remove it from the list entirely– **Examples**:“`ocamldecrement_count Cons(1, 4, Cons(2, 3, Cons(3, 2, Cons(4, 1, Empty)))) = Cons(1, 3, Cons(2, 2, Cons(3, 1, Empty)))decrement_count Cons(1, 4, Cons(2, 3, Empty)) = Cons(1, 3, Cons(2, 2, Empty))decrement_count Cons(“str”, 1, Empty) = Emptydecrement_count Empty = Empty“`
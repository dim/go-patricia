# go-patricia #

**Documentation**: [GoDoc](http://godoc.org/github.com/tchap/go-patricia/patricia)<br />
**Build Status**: [![Build Status](https://travis-ci.org/tchap/go-patricia.png?branch=master)](https://travis-ci.org/tchap/go-patricia)<br >
**Test Coverage**: Comming as soon as Drone.io people update their Go.

## About ##

A generic patricia trie (also called radix tree) implemented in Go (Golang).

The patricia trie as implemented in this library enables fast visiting of items
in some particular ways:

1. visit all items saved in the tree,
2. visit all items matching particular prefix (visit subtree), or
3. given a string, visit all items matching some prefix of that string.

`[]byte` type is used for keys, `interface{}` for values.

`Trie` is not thread safe. Synchronize the access yourself.

### State of the Project ###

This project is very much still in development and the API can change any time.

More (unit) testing would be cool.

## Usage ##

Import the package from GitHub first.

```go
import "github.com/tchap/go-patricia/patricia"
```

Then you can start having fun.

```go
printItem := func(prefix patricia.Prefix, item patricia.Item) error {
	fmt.Printf("%q: %v\n", prefix, item)
	return nil
}

// Create a new tree.
trie := NewTrie()

// Insert some items.
trie.Insert(Prefix("Pepa Novak"), 1)
trie.Insert(Prefix("Pepa Sindelar"), 2)
trie.Insert(Prefix("Karel Macha"), 3)
trie.Insert(Prefix("Karel Hynek Macha"), 4)

// Just check if some things are present in the tree.
key := Prefix("Pepa Novak")
fmt.Printf("%q present? %v\n", key, trie.Match(key))
// "Pepa Novak" present? true
key = Prefix("Karel")
fmt.Printf("Anybody called %q here? %v\n", key, trie.MatchSubtree(key))
// Anybody called "Karel" here? true

// Walk the tree.
trie.Visit(printItem)
// "Pepa Novak": 1
// "Pepa Sindelar": 2
// "Karel Macha": 3
// "Karel Hynek Macha": 4

// Walk a subtree.
trie.VisitSubtree(Prefix("Pepa"), printItem)
// "Pepa Novak": 1
// "Pepa Sindelar": 2

// Modify an item, then fetch it from the tree.
trie.Set(Prefix("Karel Hynek Macha"), 10)
key = Prefix("Karel Hynek Macha")
fmt.Printf("%q: %v\n", key, trie.Get(key))
// "Karel Hynek Macha": 10

// Walk prefixes.
prefix := Prefix("Karel Hynek Macha je kouzelnik")
trie.VisitPrefixes(prefix, printItem)
// "Karel Hynek Macha": 10

// Delete some items.
trie.Delete(Prefix("Pepa Novak"))
trie.Delete(Prefix("Karel Macha"))

// Walk again.
trie.Visit(printItem)
// "Pepa Sindelar": 2
// "Karel Hynek Macha": 10

// Delete a subtree.
trie.DeleteSubtree(Prefix("Pepa"))

// Print what is left.
trie.Visit(printItem)
// "Karel Hynek Macha": 10
```

## License ##

MIT, check the `LICENSE` file.

[![Gittip
Badge](http://img.shields.io/gittip/alanhamlett.png)](https://www.gittip.com/tchap/
"Gittip Badge")

[![Bitdeli
Badge](https://d2weczhvl823v0.cloudfront.net/tchap/go-patricia/trend.png)](https://bitdeli.com/free
"Bitdeli Badge")

## !map `(fn list)`

Returns a new list consisting of the result of applying `fn` to the items in `list`.

```cl
(!map (lambda (num) (* num num)) (quote (1 2 3 4))) ;; => (quote (1 4 9 16))
(!map (quote square) (quote (1 2 3 4))) ;; => (quote (1 4 9 16))
(!!map (* it it) (quote (1 2 3 4))) ;; => (quote (1 4 9 16))
```

## !reduce-from `(fn initial-value list)`

Returns the result of applying `fn` to `initial-value` and the
first item in `list`, then applying `fn` to that result and the 2nd
item, etc. If `list` contains no items, returns `initial-value` and
`fn` is not called.

```cl
(!reduce-from (quote +) 7 (quote (1 2))) ;; => 10
(!reduce-from (lambda (memo item) (+ memo item)) 7 (quote (1 2))) ;; => 10
(!!reduce-from (+ acc it) 7 (quote (1 2 3))) ;; => 13
```

## !reduce `(fn list)`

Returns the result of applying `fn` to the first 2 items in `list`,
then applying `fn` to that result and the 3rd item, etc. If `list`
contains no items, `fn` must accept no arguments as well, and
reduce returns the result of calling `fn` with no arguments. If
`list` has only 1 item, it is returned and `fn` is not called.

```cl
(!reduce (quote +) (quote (1 2))) ;; => 3
(!reduce (lambda (memo item) (format %s-%s memo item)) (quote (1 2 3))) ;; => 1-2-3
(!!reduce (format %s-%s acc it) (quote (1 2 3))) ;; => 1-2-3
```

## !filter `(fn list)`

Returns a new list of the items in `list` for which `fn` returns a non-nil value.

```cl
(!filter (lambda (num) (= 0 (% num 2))) (quote (1 2 3 4))) ;; => (quote (2 4))
(!filter (quote even?) (quote (1 2 3 4))) ;; => (quote (2 4))
(!!filter (= 0 (% it 2)) (quote (1 2 3 4))) ;; => (quote (2 4))
```

## !remove `(fn list)`

Returns a new list of the items in `list` for which `fn` returns nil.

```cl
(!remove (lambda (num) (= 0 (% num 2))) (quote (1 2 3 4))) ;; => (quote (1 3))
(!remove (quote even?) (quote (1 2 3 4))) ;; => (quote (1 3))
(!!remove (= 0 (% it 2)) (quote (1 2 3 4))) ;; => (quote (1 3))
```

## !concat `(&rest lists)`

Returns a new list with the concatenation of the elements in
the supplied `lists`.

```cl
(!concat (quote (1))) ;; => (quote (1))
(!concat (quote (1)) (quote (2))) ;; => (quote (1 2))
(!concat (quote (1)) (quote (2 3)) (quote (4))) ;; => (quote (1 2 3 4))
```

## !mapcat `(fn list)`

Returns the result of applying concat to the result of applying map to `fn` and `list`.
Thus function `fn` should return a collection.

```cl
(!mapcat (quote list) (quote (1 2 3))) ;; => (quote (1 2 3))
(!mapcat (lambda (item) (list 0 item)) (quote (1 2 3))) ;; => (quote (0 1 0 2 0 3))
(!!mapcat (list 0 it) (quote (1 2 3))) ;; => (quote (0 1 0 2 0 3))
```

## !partial `(fn &rest args)`

Takes a function `fn` and fewer than the normal arguments to `fn`,
and returns a fn that takes a variable number of additional `args`.
When called, the returned function calls `fn` with args +
additional args.

```cl
(funcall (!partial (quote +) 5) 3) ;; => 8
(funcall (!partial (quote +) 5 2) 3) ;; => 10
```

## !difference `(list list2)`

Return a new list with only the members of `list` that are not in LIST2.
The test for equality is done with `equal`,
or with `!compare-fn` if that's non-nil.

```cl
(!difference (quote nil) (quote nil)) ;; => (quote nil)
(!difference (quote (1 2 3)) (quote (4 5 6))) ;; => (quote (1 2 3))
(!difference (quote (1 2 3 4)) (quote (3 4 5 6))) ;; => (quote (1 2))
```

## !intersection `(list list2)`

Return a new list containing only the elements that are members of both `list` and LIST2.
The test for equality is done with `equal`,
or with `!compare-fn` if that's non-nil.

```cl
(!intersection (quote nil) (quote nil)) ;; => (quote nil)
(!intersection (quote (1 2 3)) (quote (4 5 6))) ;; => (quote nil)
(!intersection (quote (1 2 3 4)) (quote (3 4 5 6))) ;; => (quote (3 4))
```

## !uniq `(list)`

Return a new list with all duplicates removed.
The test for equality is done with `equal`,
or with `!compare-fn` if that's non-nil.

```cl
(!uniq (quote nil)) ;; => (quote nil)
(!uniq (quote (1 2 2 4))) ;; => (quote (1 2 4))
```

## !contains? `(list element)`

Return whether `list` contains `element`.
The test for equality is done with `equal`,
or with `!compare-fn` if that's non-nil.

```cl
(!contains? (quote (1 2 3)) 1) ;; => t
(!contains? (quote (1 2 3)) 2) ;; => t
(!contains? (quote (1 2 3)) 4) ;; => nil
```
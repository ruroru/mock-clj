# mock-clj

Minimalist & non-invasive API for mocking in Clojure. 
It was accidently written when I was testing my code.
Basically, it is a syntax sugar around `with-redefs`. 

## Usage

```clojure
(with-mock specs & body)
```

`specs => var-symbol value-expr/functions`

Temporarily redefines `var` while executing `body`. 
If the right-hand side of a spec is a value, then it equals to create a function constantly return the value (stub).
If the right-hand side of a spec is a function, then the var-symbol will temporaily be replaced by the function directly.

### Example 

```clojure
(defn foo [a] (str "foo" a))

(defn bar [a] (str (foo a) "bar"))

(deftest test-bar
  (with-mock [foo "oof"] ; equals to [foo (constantly "oof")] 
    (is (= (bar "test") "oofbar"))
    ; Calls history
    (is (= (calls foo) ["oof"]))
    ; Last-all
    (is (= (last-call foo) "oof"))
    ; call-count
    (is (= 1 (call-count foo)))
    ; Called? 
    (is (called? foo))
    ; Reset calls history
    (reset-calls! foo)))
```

## License

Copyright © 2017 Ming

Distributed under the Eclipse Public License.

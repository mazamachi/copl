# 式の名前無し表現
## 54
```js
x, y |- if x then y + 1 else y - 1 ==> if #2 then #1 + 1 else #1 - 1 by Tr-If {
  x, y |- x ==> #2 by Tr-Var2 {
    x |- x ==> #1 by Tr-Var1 {};
  };
  x, y |- y + 1 ==> #1 + 1 by Tr-Plus {
    x, y |- y ==> #1 by Tr-Var1 {};
    x, y |- 1 ==> 1 by Tr-Int {};
  };
  x, y |- y - 1 ==> #1 - 1 by Tr-Minus {
    x, y |- y ==> #1 by Tr-Var1 {};
    x, y |- 1 ==> 1 by Tr-Int {};
  };
}
```

## 55
```js
true, 4 |- if #2 then #1 + 1 else #1 - 1 evalto 5 by E-IfT {
  true, 4 |- #2 evalto true by E-Var {};
  true, 4 |- #1 + 1 evalto 5 by E-Plus {
    true, 4 |- #1 evalto 4 by E-Var {};
    true, 4 |- 1 evalto 1 by E-Int {};
    4 plus 1 is 5 by B-Plus {};
  };
}
```

## 56
```js
|- let x = 3 * 3 in let y = 4 * x in x + y
==> let . = 3 * 3 in let . = 4 * #1 in #2 + #1
by Tr-Let {
  |- 3 * 3 ==> 3 * 3 by Tr-Times {
    |- 3 ==> 3 by Tr-Int {};
    |- 3 ==> 3 by Tr-Int {};
  };
  x |- let y = 4 * x in x + y ==> let . = 4 * #1 in #2 + #1 by Tr-Let {
    x |- 4 * x ==> 4 * #1 by Tr-Times {
      x |- 4 ==> 4 by Tr-Int {};
      x |- x ==> #1 by Tr-Var1 {};
    };
    x, y |- x + y ==> #2 + #1 by Tr-Plus {
      x, y |- x ==> #2 by Tr-Var2 {
        x |- x ==> #1 by Tr-Var1 {};
      };
      x, y |- y ==> #1 by Tr-Var1 {};
    }
  }
}
```

## 57
```js
|- let . = 3 * 3 in let . = 4 * #1 in #2 + #1 evalto 45 by E-Let {
  |- 3 * 3 evalto 9 by E-Times {
    |- 3 evalto 3 by E-Int {};
    |- 3 evalto 3 by E-Int {};
    3 times 3 is 9 by B-Times {};
  };
  9 |- let . = 4 * #1 in #2 + #1 evalto 45 by E-Let {
    9 |- 4 * #1 evalto 36 by E-Times {
      9 |- 4 evalto 4 by E-Int {};
      9 |- #1 evalto 9 by E-Var {};
      4 times 9 is 36 by B-Times {};
    };
    9, 36 |- #2 + #1 evalto 45 by E-Plus {
      9, 36 |- #2 evalto 9 by E-Var {};
      9, 36 |- #1 evalto 36 by E-Var {};
      9 plus 36 is 45 by B-Plus {};
    };
  }
}
```

## 60
```js
|- let x = let y = 3 - 2 in y * y in let y = 4 in x + y
==> let . = let . = 3 - 2 in #1 * #1 in let . = 4 in #2 + #1
by Tr-Let {
  |- let y = 3 - 2 in y * y ==> let . = 3 - 2 in #1 * #1 by Tr-Let {
    |-  3 - 2 ==> 3 - 2 by Tr-Minus {
      |- 3 ==> 3 by Tr-Int {};
      |- 2 ==> 2 by Tr-Int {};
    };
    y |- y * y ==> #1 * #1 by Tr-Times {
      y |- y ==> #1 by Tr-Var1 {};
      y |- y ==> #1 by Tr-Var1 {};
    };
  };
  x |- let y = 4 in x + y ==> let . = 4 in #2 + #1 by Tr-Let {
    x |- 4 ==> 4 by Tr-Int {};
    x, y |- x + y ==> #2 + #1 by Tr-Plus {
      x, y |- x ==> #2 by Tr-Var2 {
        x |- x ==> #1 by Tr-Var1 {};
      };
      x, y |- y ==> #1 by Tr-Var1 {};
    };
  };
}
```

## 61
```js
|- let . = let . = 3 - 2 in #1 * #1 in let . = 4 in #2 + #1 evalto 5 by E-Let {
  |- let . = 3 - 2 in #1 * #1 evalto 1 by E-Let {
    |- 3 - 2 evalto 1 by E-Minus {
      |- 3 evalto 3 by E-Int {};
      |- 2 evalto 2 by E-Int {};
      3 minus 2 is 1 by B-Minus {};
    };
    1 |- #1 * #1 evalto 1 by E-Times {
      1 |- #1 evalto 1 by E-Var {};
      1 |- #1 evalto 1 by E-Var {};
      1 times 1 is 1 by B-Times {};
    };
  };
  1 |- let . = 4 in #2 + #1 evalto 5 by E-Let {
    1 |- 4 evalto 4 by E-Int {};
    1, 4 |- #2 + #1 evalto 5 by E-Plus {
      1, 4 |- #2 evalto 1 by E-Var {};
      1, 4 |- #1 evalto 4 by E-Var {};
      1 plus 4 is 5 by B-Plus {};
    };
  };
}
```

## 62
```js
|- let y = 2 in fun x -> x + y ==> let . = 2 in fun . -> #1 + #2 by Tr-Let {
  |-  2 ==> 2 by Tr-Int {};
  y |- fun x -> x + y ==> fun . -> #1 + #2 by Tr-Fun {
    y, x |- x + y ==> #1 + #2 by Tr-Plus {
      y, x |- x ==> #1 by Tr-Var1 {};
      y, x |- y ==> #2 by Tr-Var2 {
        y |- y ==> #1 by Tr-Var1 {};
      };
    };
  };
}
```

## 63
```js
|- let . = 2 in fun . -> #1 + #2 evalto (2)[fun . -> #1 + #2] by E-Let {
  |- 2 evalto 2 by E-Int {};
  2 |- fun . -> #1 + #2 evalto (2)[fun . -> #1 + #2] by E-Fun {}
}
```

## 65
```js
|- let . = fun . -> #1 3 + #1 4 in #1 (fun . -> #1 * #1) evalto 25 by E-Let {
  |- fun . -> #1 3 + #1 4 evalto ()[fun . -> #1 3 + #1 4] by E-Fun {};
  ()[fun . -> #1 3 + #1 4] |- #1 (fun . -> #1 * #1) evalto 25 by E-App {
    ()[fun . -> #1 3 + #1 4] |- #1 evalto ()[fun . -> #1 3 + #1 4] by E-Var {};
    ()[fun . -> #1 3 + #1 4] |- fun . -> #1 * #1 evalto (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] by E-Fun {};
    (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] |- #1 3 + #1 4 evalto 25 by E-Plus {
      (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] |- #1 3 evalto 9 by E-App {
        (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] |- #1 evalto (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] by E-Var {};
        (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] |- 3 evalto 3 by E-Int {};
        ()[fun . -> #1 3 + #1 4], 3 |- #1 * #1 evalto 9 by E-Times {
          ()[fun . -> #1 3 + #1 4], 3 |- #1 evalto 3 by E-Var {};
          ()[fun . -> #1 3 + #1 4], 3 |- #1 evalto 3 by E-Var {};
          3 times 3 is 9 by B-Times {};
        }
      };
      (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] |- #1 4 evalto 16 by E-App {
        (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] |- #1 evalto (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] by E-Var {};
        (()[fun . -> #1 3 + #1 4])[fun . -> #1 * #1] |- 4 evalto 4 by E-Int {};
        ()[fun . -> #1 3 + #1 4], 4 |- #1 * #1 evalto 16 by E-Times {
          ()[fun . -> #1 3 + #1 4], 4 |- #1 evalto 4 by E-Var {};
          ()[fun . -> #1 3 + #1 4], 4 |- #1 evalto 4 by E-Var {};
          4 times 4 is 16 by B-Times {};
        }
      };
      9 plus 16 is 25 by B-Plus {};
    };
  };
}
```

## 68
再帰
```js
|- let rec fact = fun n ->
  if n < 2 then 1 else n * fact (n - 1) in
  fact 3
==>
  let rec . = fun . ->
  if #1 < 2 then 1 else #1 * #2 (#1 - 1) in #1 3
by Tr-LetRec {
  fact, n |- if n < 2 then 1 else n * fact (n - 1) ==> if #1 < 2 then 1 else #1 * #2 (#1 - 1) by Tr-If {
    fact, n |- n < 2 ==> #1 < 2 by Tr-Lt {
      fact, n |- n ==> #1 by Tr-Var1 {};
      fact, n |- 2 ==> 2 by Tr-Int {};
    };
    fact, n |- 1 ==> 1 by Tr-Int {};
    fact, n |- n * fact (n - 1) ==> #1 * #2 (#1 - 1) by Tr-Times {
      fact, n |- n ==> #1 by Tr-Var1 {};
      fact, n |- fact (n - 1) ==> #2 (#1 - 1) by Tr-App {
        fact, n |- fact ==> #2 by Tr-Var2 {
          fact |- fact ==> #1 by Tr-Var1 {};
        };
        fact, n |- n - 1 ==> #1 - 1 by Tr-Minus {
          fact, n |- n ==> #1 by Tr-Var1 {};
          fact, n |- 1 ==> 1 by Tr-Int {};
        };
      };
    };
  };
  fact |- fact 3 ==> #1 3 by Tr-App {
    fact |- fact ==> #1 by Tr-Var1 {};
    fact |- 3 ==> 3 by Tr-Int {};
  };
};
```

## 69
再帰関数の評価
```js
|- let rec . = fun . ->
     if #1 < 2 then 1 else #1 * #2 (#1 - 1) in #1 3
   evalto 6
by E-LetRec {
  ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)] |- #1 3 evalto 6 by E-AppRec {
    ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)] |- #1 evalto ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)] by E-Var {};
    ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)] |- 3 evalto 3 by E-Int {};
    ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- if #1 < 2 then 1 else #1 * #2 (#1 - 1) evalto 6 by E-IfF {
      ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #1 < 2 evalto false by E-Lt {
        ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #1 evalto 3 by E-Var {};
        ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- 2 evalto 2 by E-Int {};
        3 less than 2 is false by B-Lt {};
      };
      ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #1 * #2 (#1 - 1) evalto 6 by E-Times {
        ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #1 evalto 3 by E-Var {};
        ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #2 (#1 - 1) evalto 2 by E-AppRec {
          ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #2 evalto ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)] by E-Var {};
          ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #1 - 1 evalto 2 by E-Minus {
            ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- #1 evalto 3 by E-Var {};
            ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 3 |- 1 evalto 1 by E-Int {};
            3 minus 1 is 2 by B-Minus {};
          };
          ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- if #1 < 2 then 1 else #1 * #2 (#1 - 1) evalto 2 by E-IfF {
            ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #1 < 2 evalto false by E-Lt {
              ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #1 evalto 2 by E-Var {};
              ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- 2 evalto 2 by E-Int {};
              2 less than 2 is false by B-Lt {};
            };
            ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #1 * #2 (#1 - 1) evalto 2 by E-Times {
              ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #1 evalto 2 by E-Var {};
              ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #2 (#1 - 1) evalto 1 by E-AppRec {
                ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #2 evalto ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)] by E-Var {};
                ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #1 - 1 evalto 1 by E-Minus {
                  ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- #1 evalto 2 by E-Var {};
                  ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 2 |- 1 evalto 1 by E-Int {};
                  2 minus 1 is 1 by B-Minus {};
                };
                ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 1 |- if #1 < 2 then 1 else #1 * #2 (#1 - 1) evalto 1 by E-IfT {
                  ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 1 |- #1 < 2 evalto true by E-Lt {
                    ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 1 |- #1 evalto 1 by E-Var {};
                    ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 1 |- 2 evalto 2 by E-Int {};
                    1 less than 2 is true by B-Lt {};
                  };
                  ()[rec . = fun . -> if #1 < 2 then 1 else #1 * #2 (#1 - 1)], 1 |- 1 evalto 1 by E-Int {};
                };
              };
              2 times 1 is 2 by B-Times {};
            };
          };
        };
        3 times 2 is 6 by B-Times {};
      };
    };
  };
};
```

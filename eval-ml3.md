# (再帰)関数抽象・適用
## 40
```ruby
|- fun x -> x + 1 evalto ()[fun x -> x + 1] by E-Fun {};
```

## 41
```ruby
|- let y = 2 in fun x -> x + y evalto (y=2)[fun x -> x + y] by E-Let {
  |- 2 evalto 2 by E-Int {};
  y = 2 |- fun x -> x + y evalto (y=2)[fun x -> x + y]  by E-Fun {};
}
```

## 42
```ruby
|- let sq = fun x -> x * x in sq 3 + sq 4 evalto 25 by E-Let {
  |- fun x -> x * x evalto ()[fun x -> x * x] by E-Fun {};
  sq = ()[fun x -> x * x] |- sq 3 + sq 4 evalto 25 by E-Plus {
    sq = ()[fun x -> x * x] |- sq 3 evalto 9 by E-App {
      sq = ()[fun x -> x * x] |- sq evalto ()[fun x -> x * x] by E-Var1 {};
      sq = ()[fun x -> x * x] |- 3 evalto 3 by E-Int {};
      x = 3 |- x * x evalto 9 by E-Times {
        x = 3 |- x evalto 3 by E-Var1 {};
        x = 3 |- x evalto 3 by E-Var1 {};
        3 times 3 is 9 by B-Times {};
      }
    };
    sq = ()[fun x -> x * x] |- sq 4 evalto 16 by E-App {
      sq = ()[fun x -> x * x] |- sq evalto ()[fun x -> x * x] by E-Var1 {};
      sq = ()[fun x -> x * x] |- 4 evalto 4 by E-Int {};
      x = 4 |- x * x evalto 16 by E-Times {
        x = 4 |- x evalto 4 by E-Var1 {};
        x = 4 |- x evalto 4 by E-Var1 {};
        4 times 4 is 16 by B-Times {};
      }
    };
    9 plus 16 is 25 by B-Plus {};
  }
}
```

## 43
高階関数
```ruby
|- let sm = fun f -> f 3 + f 4 in sm (fun x -> x * x) evalto 25 by E-Let {
  |- fun f -> f 3 + f 4 evalto ()[fun f -> f 3 + f 4] by E-Fun {};
  sm = ()[fun f -> f 3 + f 4] |- sm (fun x -> x * x) evalto 25 by E-App {
    sm = ()[fun f -> f 3 + f 4] |- sm evalto ()[fun f -> f 3 + f 4] by E-Var1 {};
    sm = ()[fun f -> f 3 + f 4] |- fun x -> x * x evalto (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] by E-Fun {};
    f = (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] |- f 3 + f 4 evalto 25 by E-Plus {
      f = (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] |- f 3 evalto 9 by E-App {
        f = (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] |- f evalto (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] by E-Var1 {};
        f = (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] |- 3 evalto 3 by E-Int {};
        sm = ()[fun f -> f 3 + f 4], x = 3 |- x * x evalto 9 by E-Times {
          sm = ()[fun f -> f 3 + f 4], x = 3 |- x evalto 3 by E-Var1 {};
          sm = ()[fun f -> f 3 + f 4], x = 3 |- x evalto 3 by E-Var1 {};
          3 times 3 is 9 by B-Times {};
        }
      };
      f = (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] |- f 4 evalto 16 by E-App {
        f = (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] |- f evalto (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] by E-Var1 {};
        f = (sm = ()[fun f -> f 3 + f 4])[fun x -> x * x] |- 4 evalto 4 by E-Int {};
        sm = ()[fun f -> f 3 + f 4], x = 4 |- x * x evalto 16 by E-Times {
          sm = ()[fun f -> f 3 + f 4], x = 4 |- x evalto 4 by E-Var1 {};
          sm = ()[fun f -> f 3 + f 4], x = 4 |- x evalto 4 by E-Var1 {};
          4 times 4 is 16 by B-Times {};
        }
      };
      9 plus 16 is 25 by B-Plus {};
    }
  }
}
```

## 44
部分適用っぽい

```ruby
|- let max = fun x -> fun y -> if x < y then y else x in max 3 5 evalto 5 by E-Let {
  |- fun x -> fun y -> if x < y then y else x evalto ()[fun x -> fun y -> if x < y then y else x] by E-Fun {};
  max = ()[fun x -> fun y -> if x < y then y else x] |- max 3 5 evalto 5 by E-App {
    max = ()[fun x -> fun y -> if x < y then y else x] |- max 3 evalto (x = 3)[fun y -> if x < y then y else x] by E-App {
      max = ()[fun x -> fun y -> if x < y then y else x] |- max evalto ()[fun x -> fun y -> if x < y then y else x] by E-Var1 {};
      max = ()[fun x -> fun y -> if x < y then y else x] |- 3 evalto 3 by E-Int {};
      x = 3 |- fun y -> if x < y then y else x evalto (x = 3)[fun y -> if x < y then y else x] by E-Fun {};
    };
    max = ()[fun x -> fun y -> if x < y then y else x] |- 5 evalto 5 by E-Int {};
    x = 3, y = 5 |- if x < y then y else x evalto 5 by E-IfT {
      x = 3, y = 5 |- x < y evalto true by E-Lt {
        x = 3, y = 5 |- x evalto 3 by E-Var2 {
          x = 3 |- x evalto 3 by E-Var1 {};
        };
        x = 3, y = 5 |- y evalto 5 by E-Var1 {};
        3 less than 5 is true by B-Lt {};
      };
      x = 3, y = 5 |- y evalto 5 by E-Var1 {};
    }
  }
}
```

## 45
```ruby
|- let a = 3 in let f = fun y -> y * a in let a = 5 in f 4 evalto 12 by E-Let {
  |- 3 evalto 3 by E-Int {};
  a = 3 |- let f = fun y -> y * a in let a = 5 in f 4 evalto 12 by E-Let {
    a = 3 |- fun y -> y * a evalto (a = 3)[fun y -> y * a] by E-Fun {};
    a = 3, f = (a = 3)[fun y -> y * a] |- let a = 5 in f 4 evalto 12 by E-Let {
      a = 3, f = (a = 3)[fun y -> y * a] |- 5 evalto 5 by E-Int {};
      a = 3, f = (a = 3)[fun y -> y * a], a = 5 |- f 4 evalto 12 by E-App {
        a = 3, f = (a = 3)[fun y -> y * a], a = 5 |- f evalto (a = 3)[fun y -> y * a] by E-Var2 {
          a = 3, f = (a = 3)[fun y -> y * a] |- f evalto (a = 3)[fun y -> y * a] by E-Var1 {};
        };
        a = 3, f = (a = 3)[fun y -> y * a], a = 5 |- 4 evalto 4 by E-Int {};
        a = 3, y = 4 |- y * a evalto 12 by E-Times {
          a = 3, y = 4 |- y evalto 4 by E-Var1 {};
          a = 3, y = 4 |- a evalto 3 by E-Var2 {
            a = 3 |- a evalto 3 by E-Var1 {};
          };
          4 times 3 is 12 by B-Times {};
        };
      };
    };
  };
};
```

## 50
```ruby
|- let rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1) in fact 3 evalto 6 by E-LetRec {
  fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] |- fact 3 evalto 6 by E-AppRec {
    fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] |- fact evalto ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] by E-Var1 {};
    fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] |- 3 evalto 3 by E-Int {};
    fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- if n < 2 then 1 else n * fact (n - 1) evalto 6 by E-IfF {
      fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- n < 2 evalto false by E-Lt {
        fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- n evalto 3 by E-Var1 {};
        fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- 2 evalto 2 by E-Int {};
        3 less than 2 is false by B-Lt {};
      };
      fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- n * fact (n - 1) evalto 6 by E-Times {
        fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- n evalto 3 by E-Var1 {};
        fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- fact (n - 1) evalto 2 by E-AppRec {
          fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- fact evalto ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] by E-Var2 {
            fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] |- fact evalto ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] by E-Var1 {};
          };
          fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- n - 1 evalto 2 by E-Minus {
            fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- n evalto 3 by E-Var1 {};
            fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 3 |- 1 evalto 1 by E-Int {};
            3 minus 1 is 2 by B-Minus {};
          };
          fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- if n < 2 then 1 else n * fact (n - 1) evalto 2 by E-IfF {
            fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- n < 2 evalto false by E-Lt {
              fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- n evalto 2 by E-Var1 {};
              fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- 2 evalto 2 by E-Int {};
              2 less than 2 is false by B-Lt {};
            };
            fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- n * fact (n - 1) evalto 2 by E-Times {
              fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- n evalto 2 by E-Var1 {};
              fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- fact (n - 1) evalto 1 by E-AppRec {
                fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- fact evalto ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] by E-Var2 {
                  fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] |- fact evalto ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)] by E-Var1 {};
                };
                fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- n - 1 evalto 1 by E-Minus {
                  fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- n evalto 2 by E-Var1 {};
                  fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 2 |- 1 evalto 1 by E-Int {};
                  2 minus 1 is 1 by B-Minus {};
                };
                fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 1 |- if n < 2 then 1 else n * fact (n - 1) evalto 1 by E-IfT {
                  fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 1 |- n < 2 evalto true by E-Lt{
                    fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 1 |- n evalto 1 by E-Var1 {};
                    fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 1 |- 2 evalto 2 by E-Int {};
                    1 less than 2 is true by B-Lt {};
                  };
                  fact = ()[rec fact = fun n -> if n < 2 then 1 else n * fact (n - 1)], n = 1 |- 1 evalto 1 by E-Int {};
                }
              };
              2 times 1 is 2 by B-Times {};
            };
          };
        };
        3 times 2 is 6 by B-Times {};
      }
    }
  }
}
```

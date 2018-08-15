# リスト
## 70
```ruby
|- (1 + 2) :: (3 + 4) :: [] evalto 3 :: 7 :: [] by E-Cons {
  |- 1 + 2 evalto 3 by E-Plus {
    |- 1 evalto 1 by E-Int {};
    |- 2 evalto 2 by E-Int {};
    1 plus 2 is 3 by B-Plus {};
  };
  |- (3 + 4) :: [] evalto 7 :: [] by E-Cons {
    |- 3 + 4 evalto 7 by E-Plus {
      |- 3 evalto 3 by E-Int {};
      |- 4 evalto 4 by E-Int {};
      3 plus 4 is 7 by B-Plus {};
    };
    |- [] evalto [] by E-Nil {};
  };
};
```

## 71
match
```ruby
|- let f = fun x -> match x with [] -> 0 | a :: b -> a in
  f (4::[]) + f [] + f (1 :: 2 :: 3 :: [])
  evalto 5
by E-Let {
  |- fun x -> match x with [] -> 0 | a :: b -> a evalto ()[fun x -> match x with [] -> 0 | a :: b -> a ] by E-Fun {};
  f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f (4 :: []) + f [] + f (1 :: 2 :: 3 :: []) evalto 5 by E-Plus {
    f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f (4 :: []) + f [] evalto 4 by E-Plus {
      f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f (4 :: []) evalto 4 by E-App {
        f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f evalto ()[fun x -> match x with [] -> 0 | a :: b -> a ] by E-Var {};
        f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 4 :: [] evalto 4 :: [] by E-Cons {
          f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 4 evalto 4 by E-Int {};
          f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- [] evalto [] by E-Nil {};
        };
        x = 4 :: [] |- match x with [] -> 0 | a :: b -> a evalto 4 by E-MatchCons {
          x = 4 :: [] |- x evalto 4 :: [] by E-Var {};
          x = 4 :: [], a = 4, b = [] |- a evalto 4 by E-Var{};
        };
      };
      f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f [] evalto 0 by E-App {
        f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f evalto ()[fun x -> match x with [] -> 0 | a :: b -> a ] by E-Var {};
        f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- [] evalto [] by E-Nil {};
        x = [] |- match x with [] -> 0 | a :: b -> a evalto 0 by E-MatchNil {
          x = [] |- x evalto [] by E-Var {};
          x = [] |- 0 evalto 0 by E-Int {};
        };
      };
      4 plus 0 is 4 by B-Plus {};
    };
    f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f (1 :: 2 :: 3 :: []) evalto 1 by E-App {
      f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- f evalto ()[fun x -> match x with [] -> 0 | a :: b -> a ] by E-Var {};
      f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 1 :: 2 :: 3 :: [] evalto 1 :: 2 :: 3 :: [] by E-Cons {
        f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 1 evalto 1 by E-Int {};
        f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 2 :: 3 :: [] evalto 2 :: 3 :: [] by E-Cons {
          f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 2 evalto 2 by E-Int {};
          f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 3 :: [] evalto 3 :: [] by E-Cons {
            f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- 3 evalto 3 by E-Int {};
            f = ()[fun x -> match x with [] -> 0 | a :: b -> a ] |- [] evalto [] by E-Nil {};
          };
        };
      };
      x = 1 :: 2 :: 3 :: [] |- match x with [] -> 0 | a :: b -> a evalto 1 by E-MatchCons {
        x = 1 :: 2 :: 3 :: [] |- x evalto 1 :: 2 :: 3 :: [] by E-Var {};
        x = 1 :: 2 :: 3 :: [], a = 1, b = 2 :: 3 :: [] |- a evalto 1 by E-Var {};
      };
    };
    4 plus 1 is 5 by B-Plus {};
  };
};
```

## 72
```ruby
|- let rec f = fun x -> if x < 1 then [] else x :: f (x - 1) in
  f 3 evalto 3 :: 2 :: 1 :: []
by E-LetRec {
  f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)] |- f 3 evalto 3 :: 2 :: 1 :: [] by E-AppRec {
    f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)] |- f evalto ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)] by E-Var {};
    f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)] |- 3 evalto 3 by E-Int {};
    f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- if x < 1 then [] else x :: f (x - 1) evalto 3 :: 2 :: 1 :: [] by E-IfF {
      f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- x < 1 evalto false by E-Lt {
        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- x evalto 3 by E-Var {};
        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- 1 evalto 1 by E-Int {};
        3 less than 1 is false by B-Lt {};
      };
      f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- x :: f (x - 1) evalto 3 :: 2 :: 1 :: [] by E-Cons {
        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- x evalto 3 by E-Var {};
        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- f (x - 1) evalto 2 :: 1 :: [] by E-AppRec {
          f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- f evalto ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)] by E-Var {};
          f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- x - 1 evalto 2 by E-Minus {
            f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- x evalto 3 by E-Var {};
            f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 3 |- 1 evalto 1 by E-Int {};
            3 minus 1 is 2 by B-Minus {};
          };
          f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- if x < 1 then [] else x :: f (x - 1) evalto 2 :: 1 :: [] by E-IfF {
            f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- x < 1 evalto false by E-Lt {
              f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- x evalto 2 by E-Var {};
              f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- 1 evalto 1 by E-Int {};
              2 less than 1 is false by B-Lt {};
            };
            f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- x :: f (x - 1) evalto 2 :: 1 :: [] by E-Cons {
              f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- x evalto 2 by E-Var {};
              f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- f (x - 1) evalto 1 :: [] by E-AppRec {
                f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- f evalto ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)] by E-Var {};
                f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- x - 1 evalto 1 by E-Minus {
                  f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- x evalto 2 by E-Var {};
                  f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 2 |- 1 evalto 1 by E-Int {};
                  2 minus 1 is 1 by B-Minus {};
                };
                f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- if x < 1 then [] else x :: f (x - 1) evalto 1 :: [] by E-IfF {
                  f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- x < 1 evalto false by E-Lt {
                    f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- x evalto 1 by E-Var {};
                    f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- 1 evalto 1 by E-Int {};
                    1 less than 1 is false by B-Lt {};
                  };
                  f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- x :: f (x - 1) evalto 1 :: [] by E-Cons {
                    f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- x evalto 1 by E-Var {};
                    f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- f (x - 1) evalto [] by E-AppRec {
                      f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- f evalto ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)] by E-Var {};
                      f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- x - 1 evalto 0 by E-Minus {
                        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- x evalto 1 by E-Var {};
                        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 1 |- 1 evalto 1 by E-Int {};
                        1 minus 1 is 0 by B-Minus {};
                      };
                      f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 0 |- if x < 1 then [] else x :: f (x - 1) evalto [] by E-IfT {
                        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 0 |- x < 1 evalto true by E-Lt {
                          f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 0 |- x evalto 0 by E-Var {};
                          f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 0 |- 1 evalto 1 by E-Int {};
                          0 less than 1 is true by B-Lt {};
                        };
                        f = ()[rec f = fun x -> if x < 1 then [] else x :: f (x - 1)], x = 0 |- [] evalto [] by E-Nil {};
                      };
                    };
                  };
                };
              };
            };
          };
        };
      };
    };
  };
};
```

## 76
関数のリスト
()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)]
ApplyEnv = apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)]
fun x -> x * x = fun x -> x * x
fun y -> y + 3 = fun y -> y + 3
```ruby
|- let rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x) in
  apply ((fun x -> x * x) :: (fun y -> y + 3) :: []) 4
  evalto 49
by E-LetRec {
  apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- apply ((fun x -> x * x) :: (fun y -> y + 3) :: []) 4 evalto 49 by E-App {
    apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- apply ((fun x -> x * x) :: (fun y -> y + 3) :: []) evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-AppRec {
      apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- apply evalto ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Var {};
      apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- (fun x -> x * x) :: (fun y -> y + 3) :: [] evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] by E-Cons {
        apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- fun x -> x * x evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] by E-Fun {};
        apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- (fun y -> y + 3) :: [] evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] by E-Cons {
          apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- fun y -> y + 3 evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] by E-Fun {};
          apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- [] evalto [] by E-Nil {};
        };
      };
      apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- fun x -> match l with [] -> x | f :: l -> f (apply l x) evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Fun {};
    };
    apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] |- 4 evalto 4 by E-Int {};
    apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4 |- match l with [] -> x | f :: l -> f (apply l x) evalto 49 by E-MatchCons {
      apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4 |- l evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] by E-Var {};
      apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- f (apply l x) evalto 49 by E-App {
        apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- f evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] by E-Var {};
        apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- apply l x evalto 7 by E-App {
          apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- apply l evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-AppRec {
            apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- apply evalto ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Var {};
            apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- l evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] by E-Var {};
            apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- fun x -> match l with [] -> x | f :: l -> f (apply l x) evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Fun {};
          };
          apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x] :: (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun x -> x * x], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] |- x evalto 4 by E-Var {};
          apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4 |- match l with [] -> x | f :: l -> f (apply l x) evalto 7 by E-MatchCons {
            apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4 |- l evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [] by E-Var {};
            apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3], l = [] |- f (apply l x) evalto 7 by E-App {
              apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3], l = [] |- f evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] by E-Var {};
              apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3], l = [] |- apply l x evalto 4 by E-App {
                apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3], l = [] |- apply l evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-AppRec {
                  apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3], l = [] |- apply evalto ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Var {};
                  apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3], l = [] |- l evalto [] by E-Var {};
                  apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = [] |- fun x -> match l with [] -> x | f :: l -> f (apply l x) evalto (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Fun {};
                };
                apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3] :: [], x = 4, f = (apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)])[fun y -> y + 3], l = [] |- x evalto 4 by E-Var {};
                apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = [], x = 4 |- match l with [] -> x | f :: l -> f (apply l x) evalto 4 by E-MatchNil {
                  apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = [], x = 4 |- l evalto [] by E-Var {};
                  apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], l = [], x = 4 |- x evalto 4 by E-Var {};
                }
              };
              apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], y = 4 |- y + 3 evalto 7 by E-Plus {
                apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], y = 4 |- y evalto 4 by E-Var {};
                apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], y = 4 |- 3 evalto 3 by E-Int {};
                4 plus 3 is 7 by B-Plus {};
              };
            };
          };
        };
        apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], x = 7 |- x * x evalto 49 by E-Times {
          apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], x = 7 |- x evalto 7 by E-Var {};
          apply = ()[rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x)], x = 7 |- x evalto 7 by E-Var {};
          7 times 7 is 49 by B-Times {};
        };
      };
    };
  };
};
```

見やすい版
```ruby
|- let rec apply = fun l -> fun x -> match l with [] -> x | f :: l -> f (apply l x) in
  apply ((fun x -> x * x) :: (fun y -> y + 3) :: []) 4
  evalto 49
by E-LetRec {
  ApplyEnv |- apply ((F1) :: (F2) :: []) 4 evalto 49 by E-App {
    ApplyEnv |- apply ((F1) :: (F2) :: []) evalto (ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-AppRec {
      ApplyEnv |- apply evalto ApplyFn by E-Var {};
      ApplyEnv |- (F1) :: (F2) :: [] evalto (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [] by E-Cons {
        ApplyEnv |- F1 evalto (ApplyEnv)[F1] by E-Fun {};
        ApplyEnv |- (F2) :: [] evalto (ApplyEnv)[F2] :: [] by E-Cons {
          ApplyEnv |- F2 evalto (ApplyEnv)[F2] by E-Fun {};
          ApplyEnv |- [] evalto [] by E-Nil {};
        };
      };
      ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [] |- fun x -> match l with [] -> x | f :: l -> f (apply l x) evalto (ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Fun {};
    };
    ApplyEnv |- 4 evalto 4 by E-Int {};
    ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4 |- match l with [] -> x | f :: l -> f (apply l x) evalto 49 by E-MatchCons {
      ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4 |- l evalto (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [] by E-Var {};
      ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F1], l = (ApplyEnv)[F2] :: [] |- f (apply l x) evalto 49 by E-App {
        ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F1], l = (ApplyEnv)[F2] :: [] |- f evalto (ApplyEnv)[F1] by E-Var {};
        ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F1], l = (ApplyEnv)[F2] :: [] |- apply l x evalto 7 by E-App {
          ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F1], l = (ApplyEnv)[F2] :: [] |- apply l evalto (ApplyEnv, l = (ApplyEnv)[F2] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-AppRec {
            ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F1], l = (ApplyEnv)[F2] :: [] |- apply evalto ApplyFn by E-Var {};
            ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F1], l = (ApplyEnv)[F2] :: [] |- l evalto (ApplyEnv)[F2] :: [] by E-Var {};
            ApplyEnv, l = (ApplyEnv)[F2] :: [] |- fun x -> match l with [] -> x | f :: l -> f (apply l x) evalto (ApplyEnv, l = (ApplyEnv)[F2] :: [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Fun {};
          };
          ApplyEnv, l = (ApplyEnv)[F1] :: (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F1], l = (ApplyEnv)[F2] :: [] |- x evalto 4 by E-Var {};
          ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4 |- match l with [] -> x | f :: l -> f (apply l x) evalto 7 by E-MatchCons {
            ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4 |- l evalto (ApplyEnv)[F2] :: [] by E-Var {};
            ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F2], l = [] |- f (apply l x) evalto 7 by E-App {
              ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F2], l = [] |- f evalto (ApplyEnv)[F2] by E-Var {};
              ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F2], l = [] |- apply l x evalto 4 by E-App {
                ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F2], l = [] |- apply l evalto (ApplyEnv, l = [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-AppRec {
                  ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F2], l = [] |- apply evalto ApplyFn by E-Var {};
                  ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F2], l = [] |- l evalto [] by E-Var {};
                  ApplyEnv, l = [] |- fun x -> match l with [] -> x | f :: l -> f (apply l x) evalto (ApplyEnv, l = [])[fun x -> match l with [] -> x | f :: l -> f (apply l x)] by E-Fun {};
                };
                ApplyEnv, l = (ApplyEnv)[F2] :: [], x = 4, f = (ApplyEnv)[F2], l = [] |- x evalto 4 by E-Var {};
                ApplyEnv, l = [], x = 4 |- match l with [] -> x | f :: l -> f (apply l x) evalto 4 by E-MatchNil {
                  ApplyEnv, l = [], x = 4 |- l evalto [] by E-Var {};
                  ApplyEnv, l = [], x = 4 |- x evalto 4 by E-Var {};
                }
              };
              ApplyEnv, y = 4 |- y + 3 evalto 7 by E-Plus {
                ApplyEnv, y = 4 |- y evalto 4 by E-Var {};
                ApplyEnv, y = 4 |- 3 evalto 3 by E-Int {};
                4 + 3 is 7 by B-Plus {};
              };
            };
          };
        };
        ApplyEnv, x = 7 |- x * x evalto 49 by E-Times {
          ApplyEnv, x = 7 |- x evalto 7 by E-Var {};
          ApplyEnv, x = 7 |- x evalto 7 by E-Var {};
          7 times 7 is 49 by B-Times {};
        };
      };
    };
  };
};
```

# 単純型システム
## 80
```ruby
|- 3 + 5 : int by T-Plus {
  |- 3 : int by T-Int {};
  |- 5 : int by T-Int {};
}
```

## 81
```ruby
|- if 4 < 5 then 2 + 3 else 8 * 8 : int by T-If {
  |- 4 < 5 : bool by T-Lt {
    |- 4 : int by T-Int {};
    |- 5 : int by T-Int {};
  };
  |- 2 + 3 : int by T-Plus {
    |- 2 : int by T-Int {};
    |- 3: int by T-Int {};
  };
  |- 8 * 8 : int by T-Times {
    |- 8 : int by T-Int {};
    |- 8 : int by T-Int {};
  };
}
```

## 82
型変数
```ruby
x : bool, y : int |- if x then y + 1 else y - 1 : int by T-If {
  x : bool, y : int |- x : bool by T-Var {};
  x : bool, y : int |- y + 1 : int by T-Plus {
    x : bool, y : int |- y : int by T-Var {};
    x : bool, y : int |- 1 : int by T-Int {};
  };
  x : bool, y : int |- y - 1 : int by T-Minus {
    x : bool, y : int |- y : int by T-Var {};
    x : bool, y : int |- 1 : int by T-Int {};
  };
};
```

## 83
let による型環境
```ruby
|- let x = 3 < 2 in let y = 5 in if x then y else 2 : int by T-Let {
  |- 3 < 2 : bool by T-Lt {
    |- 3 : int by T-Int {};
    |- 2 : int by T-Int {};
  };
  x : bool |- let y = 5 in if x then y else 2 : int by T-Let {
    x : bool |- 5 : int by T-Int {};
    x : bool, y : int |- if x then y else 2 : int by T-If {
      x : bool, y : int |- x : bool by T-Var {};
      x : bool, y : int |- y : int by T-Var {};
      x : bool, y : int |- 2 : int by T-Int {};
    };
  };
};
```

## 84
関数の型
```ruby
|- fun x -> x + 1 : int -> int by T-Fun {
  x : int |- x + 1 : int by T-Plus {
    x : int |- x : int by T-Var {};
    x : int |- 1 : int by T-Int {};
  };
};
```

## 85
関数適用
```ruby
|- let f = fun x -> x + 1 in f 4 : int by T-Let {
  |- fun x -> x + 1 : int -> int by T-Fun {
    x : int |- x + 1 : int by T-Plus {
      x : int |- x : int by T-Var {};
      x : int |- 1 : int by T-Int {};
    };
  };
  f : int -> int |- f 4 : int by T-App {
    f : int -> int |- f : int -> int by T-Var {};
    f : int -> int |- 4 : int by T-Int {};
  };
};
```

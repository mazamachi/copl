# 局所定義と環境
## 34
```js
x = 3, y = 2 |- x evalto 3 by E-Var2 {
  x = 3 |- x evalto 3 by E-Var1 {}
}
```

## 35
```js
x = true, y = 4 |- if x then y + 1 else y - 1 evalto 5 by E-IfT {
  x = true, y = 4 |- x evalto true by E-Var2 {
    x = true |- x evalto true by E-Var1 {}
  };
  x = true, y = 4 |- y + 1 evalto 5 by E-Plus {
    x = true, y = 4 |- y evalto 4 by E-Var1 {};
    x = true, y = 4 |- 1 evalto 1 by E-Int {};
    4 plus 1 is 5 by B-Plus {};
  }
}
```

## 36
```js
|- let x = 1 + 2 in x * 4 evalto 12 by E-Let {
  |- 1 + 2 evalto 3 by E-Plus {
    |- 1 evalto 1 by E-Int {};
    |- 2 evalto 2 by E-Int {};
    1 plus 2 is 3 by B-Plus {};
  };
  x = 3 |- x * 4 evalto 12 by E-Times {
    x = 3 |- x evalto 3 by E-Var1 {};
    x = 3 |- 4 evalto 4 by E-Int {};
    3 times 4 is 12 by B-Times {};
  }
}
```

### 39
```ruby
|- let x = let y = 3 - 2 in y * y in let y = 4 in x + y evalto 5 by E-Let {
  |- let y = 3 - 2 in y * y evalto 1 by E-Let {
    |- 3 - 2 evalto 1 by E-Minus {
      |- 3 evalto 3 by E-Int {};
      |- 2 evalto 2 by E-Int {};
      3 minus 2 is 1 by B-Minus {};
    };
    y = 1 |- y * y evalto 1 by E-Times {
      y = 1 |- y evalto 1 by E-Var1 {};
      y = 1 |- y evalto 1 by E-Var1 {};
      1 times 1 is 1 by B-Times {};
    }
  };
  x = 1 |- let y = 4 in x + y evalto 5 by E-Let {
    x = 1 |- 4 evalto 4 by E-Int {};
    x = 1, y = 4 |- x + y evalto 5 by E-Plus {
      x = 1, y = 4 |- x evalto 1 by E-Var2 {
        x = 1 |- x evalto 1 by E-Var1 {};
      };
      x = 1, y = 4 |- y evalto 4 by E-Var1 {};
      1 plus 4 is 5 by B-Plus {};
    }
  };
}
```

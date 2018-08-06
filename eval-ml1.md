# 単純な式の評価
## 25
```js
3 + 5 evalto 8 by E-Plus {
  3 evalto 3 by E-Int {};
  5 evalto 5 by E-Int {};
  3 plus 5 is 8 by B-Plus {};
}
```

## 26
```js
8 - 2 - 3 evalto 3 by E-Minus {
  8 - 2 evalto 6 by E-Minus {
    8 evalto 8 by E-Int {};
    2 evalto 2 by E-Int {};
    8 minus 2 is 6 by B-Minus {};
  };
  3 evalto 3 by E-Int {};
  6 minus 3 is 3 by B-Minus {};
}
```

## 28
```js
if 4 < 5 then 2 + 3 else 8 * 8 evalto 5 by E-IfT {
  4 < 5 evalto true by E-Lt {
    4 evalto 4 by E-Int {};
    5 evalto 5 by E-Int {};
    4 less than 5 is true by B-Lt {};
  };
  2 + 3 evalto 5 by E-Plus {
    2 evalto 2 by E-Int {};
    3 evalto 3 by E-Int {};
    2 plus 3 is 5 by B-Plus {};
  }
}
```

## 31
```js
1 + true + 2 evalto error by E-PlusErrorL {
  1 + true evalto error by E-PlusBoolR {
    true evalto true by E-Bool {}
  }
}
```

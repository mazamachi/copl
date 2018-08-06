# 算術式の簡約
## 21
```js
Z + S(S(Z)) -*-> S(S(Z)) by MR-One {
  Z + S(S(Z)) ---> S(S(Z)) by R-Plus {
    Z plus S(S(Z)) is S(S(Z)) by P-Zero {}
  }
}
```

## 22
```js
S(Z) * S(Z) + S(Z) * S(Z) -d-> S(Z) + S(Z) * S(Z) by DR-PlusL {
  S(Z) * S(Z) -d-> S(Z) by DR-Times {
    S(Z) times S(Z) is S(Z) by T-Succ {
      Z times S(Z) is Z by T-Zero {};
      S(Z) plus Z is S(Z) by P-Succ {
        Z plus Z is Z by P-Zero {}
      }
    }
  }
}
```


## 23
```js
S(Z) * S(Z) + S(Z) * S(Z) ---> S(Z) * S(Z) + S(Z) by R-PlusR {
  S(Z) * S(Z) ---> S(Z) by R-Times {
    S(Z) times S(Z) is S(Z) by T-Succ {
      Z times S(Z) is Z by T-Zero {};
      S(Z) plus Z is S(Z) by P-Succ {
        Z plus Z is Z by P-Zero {}
      }
    }
  }
}
```

# 算術式の評価
## 15
```js
Z + S(S(Z)) evalto S(S(Z)) by E-Plus {
  Z evalto Z by E-Const {};
  S(S(Z)) evalto S(S(Z)) by E-Const {};
  Z plus S(S(Z)) is S(S(Z)) by P-Zero {};
}
```

## 16

```js
S(S(Z)) + Z evalto S(S(Z)) by E-Plus {
  S(S(Z)) evalto S(S(Z)) by E-Const {};
  Z evalto Z by E-Const {};
  S(S(Z)) plus Z is S(S(Z)) by P-Succ {
    S(Z) plus Z is S(Z) by P-Succ {
      Z plus Z is Z by P-Zero {}
    }
  }
}
```

## 17
```js
S(Z) + S(Z) + S(Z) evalto S(S(S(Z))) by E-Plus {
  S(Z) + S(Z) evalto S(S(Z)) by E-Plus {
    S(Z) evalto S(Z) by E-Const {};
    S(Z) evalto S(Z) by E-Const {};
    S(Z) plus S(Z) is S(S(Z)) by P-Succ {
      Z plus S(Z) is S(Z) by P-Zero {};
    };
  };
  S(Z) evalto S(Z) by E-Const {};
  S(S(Z)) plus S(Z) is S(S(S(Z))) by P-Succ {
    S(Z) plus S(Z) is S(S(Z)) by P-Succ {
      Z plus S(Z) is S(Z) by P-Zero {};
    };
  }
}
```

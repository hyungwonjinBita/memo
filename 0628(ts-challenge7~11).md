# 7. 🌟 Awaited

https://driip.me/b812974b-3974-46e3-829e-1476b9b30c94

```typescript
// type MyAwaited<T extends Promise<any>> = T extends Promise<infer P> ? (P extends Promise<any> ? MyAwaited<P> : P) : never;
type MyAwaited<T> = T extends Promise<infer P> ? MyAwaited<P> : T;

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type X = Promise<string>;
type Y = Promise<{ field: number }>;
type Z = Promise<Promise<string | number>>;
type Z1 = Promise<Promise<Promise<string | boolean>>>;

type cases = [
  Expect<Equal<MyAwaited<X>, string>>,
  Expect<Equal<MyAwaited<Y>, { field: number }>>,
  Expect<Equal<MyAwaited<Z>, string | number>>,
  Expect<Equal<MyAwaited<Z1>, string | boolean>>
];

// @ts-expect-error
type error = MyAwaited<number>;
```

（infer は前の T の条件と合わせて後ろにまた出るという意味で今まで理解、、なぜかというと infer を消すと TS は infer が理解できなくなる。新たな気づきがあったら修正予定）
Conditional tpyes で extends の使い方はやっと理解できたと思う。extends の後ろにある条件の中に extends の前の部分が入っているのかをチェックする

解釈してみると MyAwaited という type alias のジェネリック T は `Promise<infer P>`の中に入る集合である。
そして、P は Conditional types の条件部分以降にも出るという意味で infer をつける
今の条件でみると

type X の場合、P は string  
type Y の場合、P は{field: number}  
type Z の場合、P は Promise<string|number>  
type Z1 の場合、P は Promise<Primise<string|boolean>>になる

で、全部`Promise<infer P>`という条件を満たしていることがわかった。なので、true になって`MyAwaited<P>`の中に各々の要素が入って T が出るまで回る

例えば、type X の場合、true になって MyAwaited <’string’>が再帰的に行う、しかし次の段階で T である string は`Promise<infer P>`を満たさないので三項演算子の結果は false になり、T になる

他の項目も同じ感じで解ける

# 8. If

```ts
type If<C extends boolean, T, F> = C extends true ? T : F;

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [
  Expect<Equal<If<true, "a", "b">, "a">>,
  Expect<Equal<If<false, "a", 2>, 2>>
];

// @ts-expect-error
type error = If<null, "a", "b">;
```

最初は下記のように書いて間違えた

```ts
type If<C, T, F> = C extends boolean ? T : F;
```

しかし、考え直してみると

```ts
C extends boolean
```

という条件は最初は true か false なのでいつも「真」なのでテストケースの上の方だけ正解になった、、

# 9. concat

```ts
type Concat<T extends any[], U extends any[]> = [...T, ...U];

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [
  Expect<Equal<Concat<[], []>, []>>,
  Expect<Equal<Concat<[], [1]>, [1]>>,
  Expect<Equal<Concat<[1, 2], [3, 4]>, [1, 2, 3, 4]>>,
  Expect<
    Equal<
      Concat<["1", 2, "3"], [false, boolean, "4"]>,
      ["1", 2, "3", false, boolean, "4"]
    >
  >
];
```

これは簡単だった！

# 10. 🌟 include

```ts
// Equalを自分で作るとしたら
type Equal<A, B> = (<T>() => T extends A ? 1 : 2) extends <T>() => T extends B
  ? 1
  : 2
  ? true
  : false;

type Includes<T extends readonly any[], U> = T extends [infer A, ...infer R]
  ? Equal<A, U> extends true
    ? true
    : Includes<R, U>
  : false;

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [
  Expect<
    Equal<Includes<["Kars", "Esidisi", "Wamuu", "Santana"], "Kars">, true>
  >,
  Expect<
    Equal<Includes<["Kars", "Esidisi", "Wamuu", "Santana"], "Dio">, false>
  >,
  Expect<Equal<Includes<[1, 2, 3, 5, 6, 7], 7>, true>>,
  Expect<Equal<Includes<[1, 2, 3, 5, 6, 7], 4>, false>>,
  Expect<Equal<Includes<[1, 2, 3], 2>, true>>,
  Expect<Equal<Includes<[1, 2, 3], 1>, true>>,
  Expect<Equal<Includes<[{}], { a: "A" }>, false>>,
  Expect<Equal<Includes<[boolean, 2, 3, 5, 6, 7], false>, false>>,
  Expect<Equal<Includes<[true, 2, 3, 5, 6, 7], boolean>, false>>,
  Expect<Equal<Includes<[false, 2, 3, 5, 6, 7], false>, true>>,
  Expect<Equal<Includes<[{ a: "A" }], { readonly a: "A" }>, false>>,
  Expect<Equal<Includes<[{ readonly a: "A" }], { a: "A" }>, false>>,
  Expect<Equal<Includes<[1], 1 | 2>, false>>,
  Expect<Equal<Includes<[1 | 2], 1>, false>>,
  Expect<Equal<Includes<[null], undefined>, false>>,
  Expect<Equal<Includes<[undefined], null>, false>>
];
```

最初は下記のように書いた

```ts
type Includes<T extends readonly any[], U> = U extends T[number] ? true : false;
```

しかし、primitive value だけが正解になった。
なので、再帰的な処理が必要だった。

```ts
type Includes<T extends readonly any[], U> = T extends [infer A, ...infer R]
  ? Equal<A, U> extends true
    ? true
    : Includes<R, U>
  : false;
```

A と U を比べて違かったら A をとって残りの R を渡してその R の中でまた A と R で分けて処理する仕組み

# 11. push

```ts
type Push<T extends any[], U> = [...T, U];

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [
  Expect<Equal<Push<[], 1>, [1]>>,
  Expect<Equal<Push<[1, 2], "3">, [1, 2, "3"]>>,
  Expect<Equal<Push<["1", 2, "3"], boolean>, ["1", 2, "3", boolean]>>
];
```

9 の concat と同じ問題だった。

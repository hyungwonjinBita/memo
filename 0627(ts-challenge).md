# 1. Pick

https://typescriptbook.jp/reference/type-reuse/utility-types/pick

```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P];
};

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [
  Expect<Equal<Expected1, MyPick<Todo, "title">>>,
  Expect<Equal<Expected2, MyPick<Todo, "title" | "completed">>>,
  // @ts-expect-error
  MyPick<Todo, "title" | "completed" | "invalid">
];

interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

interface Expected1 {
  title: string;
}

interface Expected2 {
  title: string;
  completed: boolean;
}
```

Pick は object の型と keys を入れる
なので、keys の位置には K が T の key たちであることを明示的に書く
P は K の中にある key、value は object[key]の形を持つ

# 2. ReadOnly

```ts
type MyReadonly<T> = {
  readonly [P in keyof T]: T[P];
};

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [Expect<Equal<MyReadonly<Todo1>, Readonly<Todo1>>>];

interface Todo1 {
  title: string;
  description: string;
  completed: boolean;
  meta: {
    author: string;
  };
}
```

Pick と同じ原理なので説明省略

# 3. Tuple to Object

```ts
type TupleToObject<T extends readonly any[]> = {
  [key in T[number]]: key;
};

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

const tuple = ["tesla", "model 3", "model X", "model Y"] as const;
const tupleNumber = [1, 2, 3, 4] as const;
const tupleMix = [1, "2", 3, "4"] as const;

type cases = [
  Expect<
    Equal<
      TupleToObject<typeof tuple>,
      {
        tesla: "tesla";
        "model 3": "model 3";
        "model X": "model X";
        "model Y": "model Y";
      }
    >
  >,
  Expect<Equal<TupleToObject<typeof tupleNumber>, { 1: 1; 2: 2; 3: 3; 4: 4 }>>,
  Expect<
    Equal<TupleToObject<typeof tupleMix>, { 1: 1; "2": "2"; 3: 3; "4": "4" }>
  >
];
```

T[number]の中にある要素を key と命名、number で書いた理由は配列なので、配列の要素を読み取るためには array[number]のような仕組みが必要

## as const（const assertion）

https://typescriptbook.jp/reference/values-types-variables/const-assertion
オブジェクトリテラルの末尾に as const を記述すればプロパティが readonly でリテラルタイプで指定した物と同等の扱いになります。

# 4. First of Array

https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#handbook-content

```ts
type First<T extends any[]> = T extends [] ? never : T[0];
type First<T extends any[]> = T["length"] extends 0 ? never : T[0];

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [
  Expect<Equal<First<[3, 2, 1]>, 3>>,
  Expect<Equal<First<[() => 123, { a: string }]>, () => 123>>,
  Expect<Equal<First<[]>, never>>,
  Expect<Equal<First<[undefined]>, undefined>>
];

type errors = [
  // @ts-expect-error
  First<"notArray">,
  // @ts-expect-error
  First<{ 0: "arrayLike" }>
];
```

1 番目は T が[]だったら never をそうじゃなければ T[0]で返す
2 番目は T['length']が 0 だったら never をそうじゃなければ T[0]で返す
ここで never を書いた理由はテストケースの３番目のテストが never であるため

# 5.　 Length of Tuple

https://nevertheless1record.tistory.com/m/4
https://stackoverflow.com/questions/30089879/typescript-and-dot-notation-access-to-objects

```ts
type Length<T extends readonly any[]> = T["length"];

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

const tesla = ["tesla", "model 3", "model X", "model Y"] as const;
const spaceX = [
  "FALCON 9",
  "FALCON HEAVY",
  "DRAGON",
  "STARSHIP",
  "HUMAN SPACEFLIGHT",
] as const;

type cases = [
  Expect<Equal<Length<typeof tesla>, 4>>,
  Expect<Equal<Length<typeof spaceX>, 5>>,
  // @ts-expect-error
  Length<5>,
  // @ts-expect-error
  Length<"hello world">
];
```

注目するところは JS とは違く T.length ではなく T['length']という点

# 6. Exclude(まだ完璧に理解できていない)

https://typescriptbook.jp/reference/type-reuse/utility-types/exclude

```ts
type MyExclude<T, U> = T extends U ? never : T;

/* _____________ テストケース _____________ */
import type { Equal, Expect } from "@type-challenges/utils";

type cases = [
  Expect<Equal<MyExclude<"a" | "b" | "c", "a">, Exclude<"a" | "b" | "c", "a">>>,
  Expect<
    Equal<
      MyExclude<"a" | "b" | "c", "a" | "b">,
      Exclude<"a" | "b" | "c", "a" | "b">
    >
  >,
  Expect<
    Equal<
      MyExclude<string | number | (() => void), Function>,
      Exclude<string | number | (() => void), Function>
    >
  >
];
```

T が U から extends できると never、できないと T ということは a,b,c を全て巡回して判断するってこと？
（テストしてみた結果、never だった場合何も返さない）

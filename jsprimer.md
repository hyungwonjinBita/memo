- const & let

  const とは違くて let は初期値を指定しなくても使える（この場合は undefined がデフォルトとして指定されている）

- var & let

  let は一回しか定義できないが var は何回も同じ変数名で定義できるので var を使うのは避けた方がいい

- データ型

  1. プリミティブ型：基本的な値の型  
     JS の文字列も一度作成したら変更できないイミュータブルの特性を持っている
  2. オブジェクトは一度作成した後もその値自体を変更できるためミュータブルの特性を持っている。
  3. 配列も object 　 type を持っている

- リテラル  
  数値や文字列など、データ型の値を直接記述できるように構文として定義だれたもの
- null は歴史的経緯で’object の type を持つようになった
- undefined はリテラルではない=> ただ undefined というグローバル変数の値を持っているだけ

```js
// {} は新しいオブジェクトを作成している
const objA = {};
const objB = {};
// 生成されたオブジェクトは異なる参照となる
console.log(objA === objB); // => false
// 同じ参照を比較している場合
console.log(objA === objA); // => true
```

オブジェクトリテラルは、新しいオブジェクトを作成します。 そのため、異なるオブジェクトを参照する変数を===で比較すると false を返します。

- 演算子

  - AND 演算子（＆＆）
    左辺の値の評価結果が true ならば、左辺の結果を返します。

  ```js
  // 左辺はtrueであるため、右辺の評価結果を返す
  console.log(true && "右辺の値"); // => "右辺の値"
  // 左辺がfalseであるなら、その時点でfalseを返す
  // 右辺は評価されない
  console.log(false && "右辺の値"); // => false

  true && console.log("hello");
  // hello
  false && console.log("hello");
  // false
  ```

  - OR 演算子（｜｜）
    OR 演算子は左辺の評価結果が true ならば、そのまま左辺の値を返します。

  - falsy な値の 7 つの種類

    1. false
    2. undefined
    3. null
    4. 0
    5. 0n
    6. NaN
    7. ""

  - NOT 演算子(!)

    - NOT 演算子は、オペランドの評価結果は true ならば、false を返します。
      つまり、オペランドの評価結果を反転する。
    - NOT 演算子を 2 回使うことで真偽値で変換することができる

    ```js
    const str = "";
    // 空文字列はfalsyであるため、true -> falseへと変換される
    console.log(!!str); // => false
    ```

  - Nullish coalescing 演算子（？？）
    Nillish coalescing 演算子は左辺の値が nullish(null or undefined)であるならば、右辺の評価結果を返します。

  ```js
  // 左辺がnullishであるため、右辺の値の評価結果を返す
  console.log(null ?? "右辺の値"); // => "右辺の値"
  console.log(undefined ?? "右辺の値"); // => "右辺の値"
  // 左辺がnullishではないため、左辺の値の評価結果を返す
  console.log(true ?? "右辺の値"); // => true
  console.log(false ?? "右辺の値"); // => false
  console.log(0 ?? "右辺の値"); // => 0
  console.log("文字列" ?? "右辺の値"); // => "文字列"
  ```

  なぜ必要なのか？

  - ||で解決できない問題があった

  ```js
  const inputValue = 0;
  const value = inputValue || 42;
  console.log(value); // 42
  ```

  上のケースで inputValue が falsy の場合と 0 の場合 inputValue が 42 になる
  しかし、下記の場合は 0 になる

  ```js
  const inputValue = 0;
  const value = inputValue ?? 42;
  console.log(value); // 0
  ```

  Nullish coalescing 演算子は、左辺が nullish の場合のみ、value に右辺で指定した 42 が入ります。そのため、inputValue が 0 という値が入った場合は、value にはそのまま inputVlaue の値である 0 が入る。

- 関数

  - Array-like に関して

  - Destructuring assignment

  ```js
  const user = {
    id: 42,
    name: { firstName: "Robert", middleName: "Andrew", lastName: "Garfield" },
    age: 100,
  };
  const {
    name: { firstName, middleName, lastName },
  } = user;
  console.log(firstName, middleName, lastName);
  // Robert Andrew Garfield
  ```

  - 関数はオブジェクト

  ```js
  function fn() {
    console.log("関数を呼び出します。");
  }
  const myFunc = fn;
  myFunc();
  ```

  関数が値として扱えることを、First Class Function と呼ぶ

  - メソッド

  ```js
  const obj = {
    helloFunc() {
      return "Hello!";
    },
    byeFunc() {
      return "Bye!";
    },
  };
  obj.helloFunc(); // "Hello!"
  obj.byeFunc(); // "Bye!"
  ```

- オブジェクト
  一般的にオブジェクトリテラルのプロティ名として使えるのはクォートっを省略できる

  ```js
  const obj = {
    key: "value",
  };
  ```

  一般的に使えない名はクォートで囲む

  ```js
  const obj = {
    "my-prop": "value",
  };
  ```

  この場合は呼び出すときに

  ```js
  obj["my-prop"];
  ```

  ブラケットで呼ぶ

  - プロパティの存在を確認する
    JavaScript では、存在しないプロパティに対してアクセスした場合は必ず undefined を返す
    つまり、例外が発生しない -> 間違いが発生しても気づきにくい

  - プロパティの存在確認（in)

  ```js
  const obj = { key: undefined };

  if ("key" in obj) {
    console.log("`key`propertyが存在する");
  }
  ```

  - プロパティの存在確認(Object.hasOwn)

  ```js
  const obj = { key: undefined };

  Object.hasOwn(obj, "key"); // true or false
  if (Object.hasOwn(obj, "key")) {
    console.log("`obj`は`key`プロパティを持っている");
  }
  ```

  - Optional Chaining 演算子（?.)
    Optional Chaining は左辺のオペランドが nullish の場合は、それ以上評価せずに undefined を返します。一方で、プロパティが存在する場合は、そのプロパティの評価結果を返します。
    プロパティが存在しない場合は undefined という値を返します。

  ```js
  const obj = {
    a: {
      b: "objのaプロパティのbプロパティ",
    },
  };
  console.log(obj?.a?.b); // "objのaプロパティのbプロパティ”
  console.log(obj?.k); // undefined
  ```

  ```js
  function printWidgetTitle(widget) {
    // 例外を避けるために`widget`のプロパティの存在を順番に確認してから、値を表示している
    if (widget.window !== undefined && widget.window.title !== undefined) {
      console.log(`ウィジェットのタイトルは${widget.window.title}です`);
    } else {
      console.log("ウィジェットのタイトルは未定義です");
    }
  }

  // Optional Chainingを使って、下のように簡単に表記可能

  function printWidgetTitle(widget) {
    const title = widget?.window?.title ?? "未定義";
    console.log(`ウィジェットのタイトルは${title}です`);
  }
  ```

  - オブジェクトのマージと複製

  ```js
  const obj = Object.assign(target, ...sources);

  // 新しいオブジェクトを作る場合
  const objectA = { a: "a" };
  const objectB = { b: "b" };
  const merged = Object.assign({}, objectA, objectB);
  console.log(merged); // => { a: "a", b: "b" }

  // 元々あるオブジェクトにマージ
  const objectA = { a: "a" };
  const objectB = { b: "b" };
  const merged = Object.assign(objectA, objectB);
  console.log(merged); // => { a: "a", b: "b" }
  ```

  しかし、assign より spread の方が使いやすい

  ```js
  const objectA = { a: "a" };
  const objectB = { b: "b" };
  const merged = {
    ...objectA,
    ...objectB,
  };
  console.log(merged); // => { a: "a", b: "b" }
  ```

- 配列

  - forEach 単純反復
  - map 配列の各要素を加工したい場合に利用する

  ```js
  const newArray = array.map((crt) => {
    return crt * 10;
  });
  ```

  - 配列から不要な要素を取り除いた配列を作成したい場合利用

  ```js
  const newArray = array.filter((crt) => {
    return crt % 2 === 1;
  });
  ```

- 関数と this

  - オブジェクトのプロパティが関数である場合にそれをメソッドと呼びます。

  - Arrow Function 以外の関数における this は、実行時に決まる値となります。
  - Arrow Function 以外の関数では、関数の定義だけを見て this の値が何かということは決定できない点に注意が必要です。

  - strict mode を使って意図してない動作を防止することができる
  - メソッドは何かしたのオブジェクトに所属している。JS ではオブジェクトのプロパティとして指定されている関数のことをメソッドと呼ぶから
  - this は現在の位置より一段階上にあるところを示す

  ```js
  const person = {
    fullName: "Brendan Eich",
    sayName: function () {
      return this.fullName;
    },
  };
  console.log(person.sayName());
  ```

  この場合は sayName プロパティの this は一段階上のオブジェクトである person を示す、そして person の中の fullName プロパティを参照することができる

  - 疑問点

  ```js
  const obj1 = {
    obj2: {
      method() {
        return "obj2のメソッド";
      },
      method2() {
        return this;
      },
      obj3: {
        method() {
          return this;
        },
      },
    },
  };
  ```

  obj3 から obj2 のメソッドを呼び出すことができるか？
  あったらどう呼び出す？

- this 問題となるパターン

  - this がどの値を参照するかは関数の呼び出し時に決まる（定義した時ではない）という性質に由来する。

  ```js
  "use strict";
  const person = {
    fullName: "Brendan Eich",
    sayName: function () {
      // `this`は呼び出し元によって異なる
      return this.fullName;
    },
  };
  person.sayName(); // 'Brendan Eich"

  メソッドを関数として呼ぶと問題発生;
  const say = person.sayName;
  say(); // undefined

  // 関数ではなくメソッドとして呼ぶと問題なし
  const sayMethod = psersoin.sayName();
  say; // 'Brandan Eich'
  ```

  person.sayName()は呼び出した時に person オブジェクトの sayName 関数を参照し this の値を確認する
  しかし、say 関数はどこのオブジェクトにも所属してないため、undefined になる

  - call, apply, bind メソッド

    - call （関数.call(this の値, ...関数の引数);）  
      選びたいオブジェクトを指定した状態で関数を呼び出すことができる。
      call メソッドの第二引数以降で指定した値を入れられる
      say.call(オブジェクト選択, value)

    ```js
    function say(message, age){
        return `${message} ${age} ${this.fullName}!`;
    }
    const person = {
        fullName: 'brandan Eich';
    }

    say.call(person, 'hello', 100)
    // 'hello 100 brandan Eich!'
    ```

    - apply（関数.apply(this の値, [関数の引数 1, 関数の引数 2]);）  
      apply メソッドは第一引数にオブジェクトを選択し、第二引数に関数の引数を**配列**として渡します。

    ```js
    function say(message, age) {
      return `${message} ${age} ${this.fullName}!`;
    }
    say.apply(person, ["hello", 100]);
    // 'hello 100 brandan Eich!'
    ```

    下記のような使い方も可能

    - push の場合

      ```js
      const array = ["a", "b"];
      const elements = [0, 1, 2];
      array.push.apply(array, elements);
      console.info(array); // ["a", "b", 0, 1, 2]
      ```

    - 最大値、最初値探し

      ```js
      const numbers = [5, 6, 2, 3, 7];

      const max = Math.max.apply(null, numbers);

      console.log(max);
      // expected output: 7

      const min = Math.min.apply(null, numbers);

      console.log(min);
      // expected output: 2
      ```

    - bind
      this の値を束縛した新しい関数を作成します。

    ```js
    function say(message, age) {
      return `${message} ${age} ${this.fullName}!`;
    }
    const sayPerson = say.bind(person, "hello", 100);
    sayPerson(); // 'hello 100 brandan Eich'
    ```

    bind を使わなかったら下記のようになる

    ```js
    // bind 使用 X
    const sayPerson = () => {
      return say.call(person, "hello", 100);
    };
    // bind 使用　O
    const sayPerson = say.bind(person, "hello", 100);
    ```

    基本的には call, apply, bind を使わずメソッドとして定義されている関数はメソッドとして呼ぶことが良い

  - コールバック関数と this

  ```js
  "use strict";
  const Prefixer = {
    prefix: "pre",
    prefixArray(strings) {
      return strings.map(function (str) {
        return this.prefix + "-" + str;
      });
    },
  };
  Prefixer.prefixArray(["a", "b", "c"]);
  ```

  この場合、map 関数の中 this のベースオブジェクトが undefied で type 　 error が発生する
  解決方法としては他の変数に保存する方法と map 関数の引数として this を入れる方法がある
  しかし、こういう方法はコードを書く時 this が変わることを意識して書かなければいけない問題がある
  それで Arrow function を使うと簡単に解決できる

  - Arrow Function

    - arrow function は通常の関数と違く暗黙的な this の値を受け取らない、スコープチェーンの仕組みと同様に外側の関数を探索する

    - this がどの値を参照するのかは関数の定義時に決まる

  - Arrow Function で定義した関数は call, apply, bind を使った this の指定は無視される => Arrow Function は this を持ってないため

- クラス

  - コンストラクタは？
    constructor という名前のメソッドとして定義
    クラスからインスタンスを作成する際にインスタンスに関する状態の初期化を行うメソッドである

  - クラス宣言文

  ```js
  class MyClass {
    constructor() {}
  }
  ```

  - クラス式

  ```js
  const MyClass = class MyClass {
    constructor() {}
  };

  const AnonymousClass = class {
    constructor() {}
  };
  ```

  - プロトタイプメソッド
    クラスの中のメソッドはインスタンスの動作を定義する。
    作成したメソッドは各インスタンスから共有される
    ＝＞これをプロトタイプメソッドと呼ぶ、またプロトタイプメソッドはインスタンスから呼び出せるため、インスタンスメソッドとも呼ぶ

  - メソッド定義方法
  - プロトタイプメソッドで定義

  ```js
  class Counter {
    constructor() {
      this.count = 0;
    }
    increment() {
      this.count++;
    }
  }
  ```

  - インスタンスオブジェクトで定義(constructor 関数で定義)

  ```js
  class Counter {
    counstructor() {
      this.count = 0;
      this.increment = () => {
        this.count++;
      };
    }
  }
  ```

  - インスタンスオブジェクトのメソッドとプロトタイプメソッドの違い

    - プロトタイプメソッドは各インスタンスから共有されているため、各インスタンスからのメソッドの参照先が同じ
    - インスタンスオブジェクトのメソッドは、コンストラクタで毎回同じ挙動の関数を新しく定義している　=> 各インスタンスからの参照先が異なる

    - インスタンスオブジェクトのメソッドは Arrow Fucntion か利用できる
      Arrow Function を使うと this が静的に決まるという性質を利用し、メソッドにおける this の参照先をインスタンスに固定できる

  - アクセッサプロバティ

    - getter（プロパティの参照）
      仮引数はいらない
      値を返す必要がある

    - setter（プロパティの代入）
      仮引数はプロパティへ代入する値が必要
      値を返す必要はない

  ```js
  class UserEnter {
    constructor(userName, age) {
      this.userName = userName;
      this.age = age;
    }
    get data() {
      console.log("getter");
      return [this.userName, Number(this.age)];
    }
    set data(value) {
      // setterの仮引数には一つの引数しか入らない
      console.log("setter");
      const [newUserName, newAge] = value.split(" ");
      this.userName = newUserName;
      this.age = newAge;
    }
  }

  const userTest = new UserEnter("JIN", 100);
  // getter
  userTest.data;
  // setter
  userTest.data = "Jin 30";
  ```

  value.split()を使ったのだが、もっといい方法があるのか？

  - 静的メソッド
    クラスをインスタンス化せずに利用する形

  - 2 種類のインスタンスメソッドの定義
    インスタンスオブジェクトのメソッドとプロトタイプのメソッドが同じ名前で定義されているとインスタンスオブジェクトのメソッドを呼び出すようになる
    そこで delete で削除するとプロトタイプのメソッドを使うようになる

  これでわかることは

  1. プロトタイプメソッドとインスタンスオブジェクトのメソッドは上書きされずにどちらも定義されている
  2. インスタンスオブジェクトのメソッドがプロトタイプオブジェクトのメソッドより優先して呼ばれる

  - super
    子クラスから親クラスを参照する時使う

- 例外処理

  - throw 文
    throw 文を書いて自分で例外を発生させることができる

  - Error オブジェクト
    new を Error の前に書いて作成
    第一引数にはエラーメッセージを文字列で渡す

- DOM(Document Object Model)

- 非同期処理

- Promise
  成功した場合 => resolved state => then が処理
  失敗した場合 => rejected state => catch が処理

- tagged Template

tagged tmplate を簡単に使うと文字列と値を分けることができる

```js
function teggedFunction(string, ...values) {
  console.log("String: ", strings);
  // Strings:  (3) ['This is ', ' literal. ', '', raw: Array(3)]
  console.log("Values: ", values);
  // Values :  (2) ['template', 0]
}

taggedFunction`This is ${"template"} literal. ${0}`;
```

- export
  名前付きとデフォルトの２種類がある
  名前付きの場合、エクスポートした時の名前をそのままインポートしなければならない
  デフォルトの場合、エクスポートした時の任意の名前を使用できる

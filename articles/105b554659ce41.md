---
title: "TypeScriptの型定義をざっくり理解する"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["TypeScript"]
published: true
publication_name: 'irsc'
---

## はじめに
TypeScript にはさまざまな型定義があります。
私は業務で TypeScript を使う中で、コンパイルエラーに度々苦労しています。
苦手意識を払拭するため、名著の[ブルーベリー本](https://www.amazon.co.jp/%E3%83%97%E3%83%AD%E3%82%92%E7%9B%AE%E6%8C%87%E3%81%99%E4%BA%BA%E3%81%AE%E3%81%9F%E3%82%81%E3%81%AETypeScript%E5%85%A5%E9%96%80-%E5%AE%89%E5%85%A8%E3%81%AA%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AE%E6%9B%B8%E3%81%8D%E6%96%B9%E3%81%8B%E3%82%89%E9%AB%98%E5%BA%A6%E3%81%AA%E5%9E%8B%E3%81%AE%E4%BD%BF%E3%81%84%E6%96%B9%E3%81%BE%E3%81%A7-Software-Design-plus/dp/4297127474
)で型定義について学習しました。
非常にわかりやすい本でおすすめです。
今回は TypeScript の型定義がざっくり理解できるように紹介していきます。
※サンプルコードは自作しています

## 変数の型
変数の型定義は、特定の値の種類を表します。この例では、Name 型が文字列型 (string) を示しています。
#### 構文
```TypeScript
type 型名 = 型;
```
#### 例
```TypeScript
type Name = string;
```

## オブジェクトの型
オブジェクトの型定義は、オブジェクトの構造を示します。それぞれのプロパティの型が指定され、それぞれのプロパティには名前があります。この例では、Student 型は名前 (Name)、年齢 (Age)、学生であるかどうか (IsStudent) を持つオブジェクトを表します。
#### 構文
```TypeScript
type 型名 = {
  プロパティ: 型;
};
```
#### 例
```TypeScript
type Student = {
  name: string;
  age: number;
  is_student: boolean;
};
```
## 部分型
部分型は、ある型が他の型のすべてのプロパティを持っている場合を示します。つまり、詳細な型 (Student) が一般的な型 (Person) のすべてのプロパティを含む場合、詳細な型は一般的な型の部分型です。
```TypeScript
type Person = {
  name: string;
  age: number;
};

// Person型を拡張してStudent型を作成
type Student = Person & {
  is_student: boolean;
};

const studentTaro: Student = {
  name: '山田太郎',
  age: 16,
  is_student: true,
};

// Student型はPerson型を含むため、以下の代入はエラーが起きない
const personTaro: Person = studentTaro;
```

## 型引数を持つ型
型引数を持つ型は、汎用的な型を定義します。具体的な型が代入されるときに型引数が置き換えられます。この例では、Student 型が名前 (Name) の型を型引数として取ります。
#### 構文
```TypeScript
type 型名<T> = {
  プロパティ: T;
};
```
#### 例
```TypeScript
type Student<T> = {
  name: T;
  age: number;
  is_student: boolean;
};

const firstStudent: Student<string> = {
  name: '山田太郎';
  age: 16;
  is_student: true;
};
```

## 配列の型
配列の型定義は、配列がどのような要素の型で構成されているかを示します。この例では、StudentNumbers 型が number 型の要素を持つ配列を表します。
#### 構文
```TypeScript
type StudentNumbers = number[];
```
#### 例
```TypeScript
const studentList: StudentNumbers = [1, 2, 3]
```

## 関数の型
関数の型定義は、関数が受け取る引数と返り値の型を示します。この例では、StringToUpper 型が文字列を受け取り、それを大文字に変換した文字列を返す関数の型を表します。
#### 構文
```TypeScript
type 関数名 = (引数名: 型, ...) => 返り値の型;
```
#### 例
```TypeScript
type StringToUpper = (input: string) => string;

const convertToUpper: StringToUpper = (input: string) => {
  return input.toUpperCase();
};

const result = convertToUpper('hello');
```
## ジェネリクス
ジェネリクスは、型や関数を汎用的に定義する方法です。型引数を使って、特定の型ではなく任意の型を扱うことができます。この例では、toArray 関数が任意の型の引数を受け取り、その引数を要素とする配列を返すことを示しています。
#### 構文
```TypeScript
const 関数名 = <T>(引数名: T): T => {
    // 関数の本体
};
```
#### 例
```TypeScript
const toArray<T>(arg: T): T[] => {
  return [arg];
}

// 型推論で型引数に srting型 が入る
const strArray = toArray("Hello");
// 型推論で型引数に number型 が入る
const numArray = toArray(123);
```
## ユニオン型
複数の型のいずれかを持つことができる型を定義します。
#### 構文
```TypeScript
type 型名 = 型1 | 型2;
```
#### 例
```TypeScript
type StringOrNumber = string | number;

const value1: StringOrNumber = 'Hello';
const value2: StringOrNumber = 42;
```
## インターセクション型
複数の型を組み合わせて一つの型を作ります。
#### 構文
```TypeScript
type 型名 = 型1 & 型2;
```
#### 例
```TypeScript
type Name = {
  name: string;
};

type Age = {
  age: number;
};

type Person = Name & Age;

const person: Person = {
  name: '田中太郎',
  age: 16
};
```
## リテラル型
リテラル型は、特定の値そのものを型として使用します。
#### 構文
```TypeScript
type 型名 = '値1' | '値2';
```
#### 例
```TypeScript
type YesNo = 'yes' | 'no';

const answer: YesNo = 'yes';
```


## まとめ
TypeScriptの型定義について紹介しました。
コンパイルエラーは TypeScript の大きな武器であり、型定義を使いこなせばプロダクトの品質に大きく寄与できます。
積極的に型を書いて恩恵を受けようと思います。

#### 参考
#### 「プロを目指す人のためのTypeScript入門 安全なコードの書き方から高度な型の使い方まで」
@[card](https://www.amazon.co.jp/%E3%83%97%E3%83%AD%E3%82%92%E7%9B%AE%E6%8C%87%E3%81%99%E4%BA%BA%E3%81%AE%E3%81%9F%E3%82%81%E3%81%AETypeScript%E5%85%A5%E9%96%80-%E5%AE%89%E5%85%A8%E3%81%AA%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AE%E6%9B%B8%E3%81%8D%E6%96%B9%E3%81%8B%E3%82%89%E9%AB%98%E5%BA%A6%E3%81%AA%E5%9E%8B%E3%81%AE%E4%BD%BF%E3%81%84%E6%96%B9%E3%81%BE%E3%81%A7-Software-Design-plus/dp/4297127474)

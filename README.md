```
T：Type
K：Key
U：Unknown
E：Element
------------------------------------------------------------------------------------->
■Intersection型
type IntersectionA = {
  id: number
  first: string
}

type IntersectionB = {
  id: number
  second: string
}

type IntersectionResult = IntersectionA & IntersectionB
------------------------------------------------------------------------------------->
■Union型
type Union: number | undefined;

type Union =
  | 400
  | 404;

type Union = (string | number)[];
------------------------------------------------------------------------------------->
■Generics型

class Generics<T> {
  private data = [];
  push(item: T) { this.data.push(item); }
}
const result = new Generics<number>();
result.push(0);


class Generics<T extends string> {
  name: T
  constructor (name: T) {
    this.name = name;
  };
};
const result = new Generics('文字');


interface GenericsProps {
  name: string
  age: number
  gender: 'male' | 'female' | 'other'
};
class Generics<T extends GenericsProps> {
  name: T['name']
  age: T['age']
  gender: T['gender']
  constructor (props: T) {
    this.name = props.name
    this.name = props.age
    this.name = props.gender
  }
}
const result = new Generics({
  name: 'name'
  age: 20
  gender: 'male'
})


const generics = <T extends number | string>(value: T): T => value;
const result = generics<string>("文字");


function generics<T>(arg: T): T {
  return arg;
}
generics<string>("文字");
generics<number>(0);


interface Generics<T> {
  value: T;
}
const result: Generics<string> = { value: "文字" };


interface Generics<T = string> {
  value: T;
}
const result: Generics<string> = { value: "文字" };


interface Generics<T extends string | number> {
  value: T;
}
const result: Generics<string> = { value: "文字" };
const result: Generics<number> = { value: 0 };


type GenericsBase<T> = T extends string ? number : boolean;
type GenericsA = GenericsBase<string>;
type GenericsB = GenericsBase<object>;
------------------------------------------------------------------------------------->
■Partial型（全てオプショナル Partial<T>）
type PartialBase = {
  name: string;
  age: string;
  address: string;
};
const result: Partial<PartialBase> = { name: "名前" };
const result: Partial<PartialBase> = { name: "名前", age: '30' };
------------------------------------------------------------------------------------->
■Required型（全て必須 Required<T>）
type PartialBase = {
  name: string;
  age: string;
  address: string;
};
const result: Required<PartialBase> = {
  name: "名前";
  age: "30";
  address: "hoge@gmail.com";
};
------------------------------------------------------------------------------------->
■Readonly型（全て変更不可 Readonly<T>）
type ReadonlyBase = Readonly<{
  name: string;
  age: string;
  address: string;
}>;
const result: ReadonlyBase = {
  name: "名前";
  age: "30";
  address: "hoge@gmail.com";
};
result.name = "update";  // エラー


interface ReadonlyBase {
  name: string;
  age: string;
  address: string;
}
function readonly(args: Readonly<ReadonlyBase>) {
  args.name = "update";  // エラー
}
readonly({ name: "名前", age: "30", address: "hoge@gmail.com" })


class Readonly {
  readonly name: string;
  constructor(name: string) {
    this.name = name;
  }
}
const result = new Readonly("名前);
result.name = "update";  // エラー
------------------------------------------------------------------------------------->
■Pick型（指定したプロパティから生成 Pick<T, K>）
interface PickBase {
  name: string;
  age: string;
  address: string;
}
type PickResult = Pick<PickBase, "name" | "age">;
------------------------------------------------------------------------------------->
■Omit型（指定したプロパティを削除 Omit<T, K>）
interface OmitBase {
  name: string;
  age: string;
  address: string;
}
type OmitResult = Omit<OmitBase, "name" | "age">;
------------------------------------------------------------------------------------->
■Extract型（Extract<T, U>）
type ExtractBase = {
    type: 'a',
    data: number;
} | {
    type: 'b',
    data: string;
};
type ExtractA = Extract<ExtractBase, {type: 'a'}>;
const result: ExtractA = { type: "a", data: 100 }    // OK
const result: ExtractA = { type: "a", data: "100" }  // エラー
------------------------------------------------------------------------------------->
<HTMLButtonElement>
<HTMLInputElement>
<HTMLDivElement>
<HTMLAnchorElement>
<HTMLParagraphElement>

const onClick = (e: MouseEvent<HTMLInputElement>) => {};
const onChange = (e: ChangeEvent<HTMLInputElement>) => {};
const onkeypress = (e: KeyboardEvent<HTMLInputElement>) => {};
const onBlur = (e: FocusEvent<HTMLInputElement>) => {};
const onFocus = (e: FocusEvent<HTMLInputElement>) => {};
const onSubmit = (e: FormEvent<HTMLFormElement>) => {};
------------------------------------------------------------------------------------->
```
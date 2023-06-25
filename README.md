# serial

A simple serialize library for js.

## Install

```bash
npm install @sky0014/serial
```

## Usage

### Register class or funtions that need to be serialized (support nested)

```ts
import serial from "@sky0014/serial";

class A {
  _a = 1;

  get a() {
    return this._a;
  }

  set a(val: number) {
    this._a = val;
  }
}

class B {
  b = new A();
}

class C {
  c = {
    a: new A(),
    b: new B(),
    c: 2,
  };
}

serial.register({
  A,
  B,
  C,
});
```

### Serialize obj to string

```ts
import serial from "@sky0014/serial";

const str1 = serial.stringify(new A());
//'{"__@@serial_cls":{"__@@serial_type":"A","__@@serial_data":{"_a":1}}}'

const str2 = serial.stringify(new C());
// '{"__@@serial_cls":{"__@@serial_type":"C","__@@serial_data":{"c":{"a":{"__@@serial_cls":{"__@@serial_type":"A","__@@serial_data":{"_a":1}}},"b":{"__@@serial_cls":{"__@@serial_type":"B","__@@serial_data":{"b":{"__@@serial_cls":{"__@@serial_type":"A","__@@serial_data":{"_a":1}}}}}},"c":2}}}}'
```

### Parsed from string

```ts
const a = serial.parse(str1); // a instance of A
console.log(a._a); // 1
console.log(a.a); // 1

const c = serial.parse(str2);
// c instance of C
// c.c.a instance of A
// c.c.b instance of B
console.log(c.c.c); // 2
```

## Publish

If your first time publish a package, login first:

```bash
npm login --registry=https://registry.npmjs.org
```

Then you can publish:

```bash
npm run pub
```

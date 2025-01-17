# clean-code-typescript [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Clean%20Code%20Typescript&url=https://github.com/labs42io/clean-code-typescript)

แนวคิด Clean Code สำหรับปรับใช้กับ TypeScript.  
ได้แรงบันดาลใจจาก [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript).

## สารบัญ

1. [บทนำ](#บทนำ)
2. [ตัวแปร](#ตัวแปร)
3. [ฟังก์ชัน](#ฟังก์ชัน)
4. [อ็อบเจกต์และโครงสร้างข้อมูล](#อ็อบเจกต์และโครงสร้างข้อมูล)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translations](#translations)

## บทนำ

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

หลักการทางวิศวกรรมซอฟต์แวร์จากหนังสือ [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) ของ Robert C.Martin, ปรับใช้สำหรับ TypeScript นี่ไม่ใช่คู่มือแนะนำแนวทางการเขียนโค้ด แต่เป็นแนวทางในการสร้างสรรค์ซอฟต์แวร์ให้[สามารถอ่านโค้ดเข้าใจได้ง่าย สามารถนำโค้ดมาใช้ซ้ำได้ และสามารถปรับปรุงโค้ดได้](https://github.com/ryanmcdermott/3rs-of-software-architecture) ในรูปแบบของภาษา TypeScript

แต่ไม่ใช่ว่าจะต้องทำตามหลักการในนี้ทั้งหมดทุกข้อนะ และหลักการที่ยอมรับกันทั่วไปก็มีไม่กี่ข้อเท่านั้น นี่จึงเป็นเพียงแค่แนวทางเท่านั้นไม่มีอะไรมากกว่านี้แล้ว แต่ทั้งหมดนี้เป็นโค้ดที่ตกผลึกมาจากประสบการณ์หลายปีของผู้เขียนหนังสือ _Clean Code_ นั่นเอง

งานด้านวิศวกรรมซอฟต์แวร์ของเราพึ่งจะมีขึ้นมาแค่ 50 กว่าปีเท่านั้น และเรายังคงต้องเรียนรู้กันอีกมาก เมื่อสถาปัตยกรรมซอฟต์แวร์เก่าพอ ๆ กับสถาปัตยกรรมเอง บางทีกฎของเราบางข้ออาจจะยากเกินไปที่จะทำตามนะ เอาหล่ะตอนนี้เรามาใช้แนวทางเหล่านี้เป็นหลักในการประเมินคุณภาพของโค้ด TypeScript ที่คุณและทีมของคุณสร้างขึ้นมากันดีกว่า

อีกอย่างนึง: การรู้สิ่งเหล่านี้ไม่ได้ทำให้คุณเป็นนักพัฒนาที่เก่งขึ้นมาทันที และต่อให้ใช้มันไปอีกหลายปีก็ไม่ได้หมายความว่าคุณจะไม่มีข้อผิดพลาดเลย โค้ดทุกส่วนในนี้ก็เริ่มมาจากการร่างในตอนแรกเหมือนการปั้นดินน้ำมันที่ให้เป็นรูปทรงขึ้นมาก่อน จนสุดท้ายเราก็ค่อย ๆ แกะสลักส่วนที่ยังไม่สมบูรณ์ออกตอนที่เราช่วยกันตรวจสอบในทีม อย่าโทษตัวเองเมื่อคุณต้องแก้โค้ดของคุณตั้งแต่ตอนแรก แต่จงทำให้โค้ดมันดีขึ้นเรื่อย ๆ แทนจะดีกว่า!

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## ตัวแปร

### ตั้งชื่อตัวแปรให้มีความหมาย

แต่ละชื่อควรตั้งให้คนที่อ่านรู้ได้ทันทีว่ามีอะไรอยู่บ้าง

**ไม่ดี:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}
```

**ดี:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ใช้ชื่อตัวแปรที่อ่านออกเสียงได้

ถ้าคุณไม่สามารถอ่านชื่อตัวแปรเป็นคำพูดได้ ก็เหมือนคุณเป็นคนบ้าที่พูดไม่รู้เรื่อง

**ไม่ดี:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
};
```

**ดี:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
};
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ใช้ชื่อเป็นคำศัพท์เดียวกันสำหรับตัวแปรที่เป็นประเภทเดียวกันไปเลย

**ไม่ดี:**

```ts
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**ดี:**

```ts
function getUser(): User;
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ตั้งชื่อให้สามารถพิมพ์ค้นหาได้

เราใช้เวลาอ่านโค้ดมากกว่าเขียนโค้ด ดังนั้นสิ่งสำคัญคือโค้ดที่เราเขียนจะต้องอ่านได้และค้นหาได้ โดยการที่เรา _ไม่_ ตั้งชื่อตัวแปรให้ท้ายที่สุดแล้วมีความหมายสำหรับการทำความเข้าใจโปรแกรมของเรา จะทำให้คนที่มาอ่านโค้ดลำบากมาก วิธีการทำให้ชื่อค่าคงที่ของคุณสามารถค้นหาได้ เครื่องมือเช่น [TSLint](https://palantir.github.io/tslint/rules/no-magic-numbers/) สามารถช่วยหาค่าคงที่ที่ไม่มีชื่อได้

**ไม่ดี:**

```ts
// What the heck is 86400000 for?
setTimeout(restart, 86400000);
```

**ดี:**

```ts
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ใช้ชื่อตัวแปรอธิบายการทำงานของโค้ด

**ไม่ดี:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
  // iterate through users map
}
```

**ดี:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // iterate through users map
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### หลีกเลี่ยงการใช้ตัวแปรเป็นชื่อย่อ

ชัดเจนดีกว่าโดยปริยาย.  
_ความชัดเจนคือราชา._

**ไม่ดี:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**ดี:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### อย่าเพิ่มบริบทโดยไม่จำเป็น

ถ้าชื่อ class/type/object บอกอะไรคุณบางอย่างได้อยู่แล้ว อย่าไปตั้งซ้ำในชื่อตัวแปรอีกที

**ไม่ดี:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
};

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**ดี:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
};

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### กำหนดค่าอาร์กิวเมนต์เริ่มต้นแทนการใช้ short circuiting หรือเขียนเงื่อนไข

กำหนดค่าอาร์กิวเมนต์เริ่มต้นไปเลยจะดูสะอาดกว่าการใช้ short circuiting.

**ไม่ดี:**

```ts
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**ดี:**

```ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ใช้ enum เพื่อจำกัดค่าต้องการ

Enums สามารถช่วยคุณจำกัดค่าต้องการได้ เช่น เวลาที่เรากังกลว่าค่าอาจจะไม่ตรงกับค่าที่ควรจะเป็นจริง ๆ

**ไม่ดี:**

```ts
const GENRE = {
  ROMANTIC: "romantic",
  DRAMA: "drama",
  COMEDY: "comedy",
  DOCUMENTARY: "documentary",
};

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
      // some logic to be executed
    }
  }
}
```

**ดี:**

```ts
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
      // some logic to be executed
    }
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## ฟังก์ชัน

### อาร์กิวเมนต์ของฟังก์ชัน (ถ้าให้ดีควรมีแค่ 2 หรือน้อยกว่า)

การจำกัดจำนวนของพารามิเตอร์เป็นสิ่งที่สำคัญอย่างยิ่ง เพราะว่ามันจะทำให้การทดสอบฟังก์ชันของคุณทำได้ง่ายมาก การที่มีมากกว่า 3 ตัวขึ้นไป จะทำให้เราต้องเขียนเทสเคสที่แตกต่างกันของแต่ละอาร์กิวเมนต์ ซึ่งมันจะจำนวนเทสเคสที่เยอะมาก

การมี 1 หรือ 2 อาร์กิวเมนต์จึงเป็นสิ่งที่ดีที่สุด และเป็นไปได้ไม่ควรถึง 3 อาร์กิวเมนต์ แต่ถ้าจำเป็นต้องมีมากกว่านั้นจริง ๆ ก็ควรหาทางรวมกันให้ได้ ซึ่งโดยปกติถ้าคุณมีมากกว่า 2 อาร์กิวเมนต์ แปลว่าฟังก์ชันที่คุณพยามทำนั้นมันใหญ่เกินไปแล้ว แต่ในกรณีที่ไม่ใช่แบบนั้น การใช้ higher-level object เป็นอาร์กิวเมนต์ก็จะเป็นทางเลือกที่ดีกว่า

ถ้าคุณพบว่าคุณเองต้องการใช้อาร์กิวเมนต์จำนวนมาก ให้ตัดสินใจใช้ object literals

เพื่อให้ชัดเจนว่าฟังก์ชันของคุณต้องการ property อะไรบ้าง คุณควรเลือกใช้ [destructuring](https://basarat.gitbook.io/typescript/future-javascript/destructuring) ซึ่งมีข้อดีดังนี้:

1. เมื่อมีคนดูการทำงานของฟังก์ชัน จะรู้ได้ทันทีเลยว่า property อะไรบ้างที่กำลังถูกใช้อยู่

2. สามารถใช้เพื่อจำลองเป็นชื่อพารามิเตอร์ได้

3. การ Destructuring จะส่งค่าเริ่มต้นของอาร์กิวเมนต์ไปในฟังก์ชันด้วย ทำให้ช่วยป้องกันไม่ให้เกิดปัญหา side effect ได้ ข้อควรจำ: ถ้าเป็น object และ array ที่ถูก destructure ไว้อยู่แล้วในอาร์กิวเมนต์ ค่าเหล่านั้นจะไม่ถูกส่งเข้าฟังก์ชันนั้นด้วย

4. TypeScript จะมีการเตือนหากมี property ไหนที่ไม่ได้ใช้ได้ด้วย ซึ่งจะเป็นไม่ได้หากเราไม่มีการใช้ destructuring

**ไม่ดี:**

```ts
function createMenu(
  title: string,
  body: string,
  buttonText: string,
  cancellable: boolean
) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);
```

**ดี:**

```ts
function createMenu(options: {
  title: string;
  body: string;
  buttonText: string;
  cancellable: boolean;
}) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

คุณสามารถปรับปรุงให้อ่านง่ายขึ้นได้อีกโดยใช้ [type aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases):

```ts
type MenuOptions = {
  title: string;
  body: string;
  buttonText: string;
  cancellable: boolean;
};

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ฟังก์ชันนึงควรทำแค่ 1 อย่าง

นี่เป็นกฎที่สำคัญที่สุดในวิศวกรรมซอฟต์แวร์ เมื่อฟังก์ชันนึงทำหลายอย่าง จะทำให้เขียน test case ยาก แต่เมื่อคุณแยกฟังก์ชันเป็นฟังกชั่นเล็ก ๆ ที่ทำงานแค่ 1 อย่าง จะทำให้สามารถปรับปรุงโค้ดได้ง่ายและดูสะอาดตาขึ้นมาก ถ้าคุณไม่ได้ทำอะไรผิดแปลกไปนอกเหนือจากคู่มือนี้ คุณก็จะลำ้หน้า developers คนอื่น ๆ อีกมาก

**ไม่ดี:**

```ts
function emailClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**ดี:**

```ts
function emailClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ชื่อฟังก์ชันควรบอกได้เลยว่าทำอะไร

**ไม่ดี:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**ดี:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ฟังก์ชันควรเป็นนามธรรมระดับหนึ่งเท่านั้น

เมื่อคุณมีนามธรรมมากกว่าหนึ่งระดับหน้าที่ ของคุณมักจะทำมากเกินไป การแยกฟังก์ชันนำไปสู่การใช้ซ้ำและการทดสอบที่ง่ายขึ้น

**ไม่ดี:**

```ts
function parseCode(code: string) {
  const REGEXES = [
    /* ... */
  ];
  const statements = code.split(" ");
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**ดี:**

```ts
const REGEXES = [
  /* ... */
];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(" ");
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ลบโค้ดที่ซ้ำกัน

พยายามหลีกเลี่ยงไม่ให้มีโค้ดซ้ำกัน การมีโค้ดซ้ำกันเป็นสิ่งที่ไม่ดีเลย เพราะว่าถ้าเราต้องแก้โค้ดนั้นมันก็หมายความว่าเราต้องตามแก้ให้ครบทุกจุดที่ซ้ำกัน

ลองนึกภาพว่าถ้าคุณเป็นเจ้าของร้านอาหารและคุณต้องการติดตามสินค้าคงคลังของคุณ: มะเขือเทศ, หัวหอม, กระเทียม, เครื่องเทศ และอื่น ๆ ทั้งหมดของคุณ
ถ้าคุณจดรายการเหล่านั้นไว้หลาย ๆ ที่ เมื่อคุณมีการเสิร์ฟอาหารที่มีมะเขือเทศออกไป คุณก็ต้องไปปรับจำนวนมะเขือเทศในในทุกรายการที่ที่จดไว้ แต่ถ้าคุณจดไว้ที่เดียวคุณก็แค่ปรับรายการแค่ที่เดียว!

บ่อยครั้งที่คุณมีโค้ดที่ซ้ำกันได้ เพราะคุณมีโค้ดที่แตกต่างกันเล็กน้อยมากกว่าสองที่ ส่วนใหญ่จะมีส่วนมีเหมือนกันอยู่เกือบทั้งหมด แต่ส่วนที่ต่างเล็กน้อยนั่นแหละบังคับให้คุณต้องแยกฟังก์ชันออกไปมากกว่าสองที่ ทั้ง ๆ ที่มันทำงานเหมือนกัน การลบโค้ดที่ซ้ำกันคือการสร้างสิ่งที่เป็นนามธรรมที่สามารถจัดสิ่งที่แตกต่างกันทั้งหมดนี้ด้วยการใช้แค่เพียง ฟังก์ชัน/โมดูล/คลาส เดียวเท่านั้น

การทำให้สิ่งที่เป็นนามธรรมถูกต้อง เป็นสิ่งสำคัญนั่นเป็นเหตุผลที่คุณควรปฏิบัติตามหลักการ [SOLID](#solid) นามธรรมที่ไม่ดีอาจแย่กว่าโค้ดที่ซ้ำกันจงระวังไว้ด้วย! บอกไว้เลยว่า ถ้าคุณสามารถสร้างนามธรรมที่ดีได้ก็จงทำ! อย่าทำอะไรซ้ำซากจำเจ มิฉะนั้นคุณจะพบว่าตัวเองต้องคอยตามไปแก้ไขโค้ดหลายที่ ทุกครั้งเวลาที่คุณต้องการแก้ไขโค้ดแค่จุดเดียว

**ไม่ดี:**

```ts
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink,
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio,
    };

    render(data);
  });
}
```

**ดี:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    };
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    };
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

คุณควรต้องใช้วิจารณญาณเกี่ยวกับเรื่องโค้ดซ้ำ เพราะมีบางครั้งที่ต้องเลือกระหว่างการมีโค้ดที่ซ้ำกันกับการมีที่โค้ดมีความซับซ้อนเพิ่มขิ้นโดยไม่จำเป็น เมื่อมีโค้ดที่เขียนคล้ายกันของโมดูลสองโมดูลที่อยู่กันคนละโดเมน การมีโค้ดซ้ำแบบนั้นก็ถือว่าเป็นเรื่องที่ยอมรับได้ และน่าสนใจกว่าการมีโค้ดพื้นฐานแยกกัน โค้ดพื้นฐานที่แยกออกมาในกรณีนี้แนะนำให้ใช้การพึ่งพาทางอ้อมระหว่างสองโมดูล

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### ตั้งค่าเริ่มต้นให้อ็อบเจกต์ด้วย Object.assign หรือ destructuring

**ไม่ดี:**

```ts
type MenuConfig = {
  title?: string;
  body?: string;
  buttonText?: string;
  cancellable?: boolean;
};

function createMenu(config: MenuConfig) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: "Bar" });
```

**ดี:**

```ts
type MenuConfig = {
  title?: string;
  body?: string;
  buttonText?: string;
  cancellable?: boolean;
};

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true,
    },
    config
  );

  // ...
}

createMenu({ body: "Bar" });
```

อีกทางนึง คุณสามารถใช้ destructuring ด้วยค่าเริ่มต้นแบบนี้:

```ts
type MenuConfig = {
  title?: string;
  body?: string;
  buttonText?: string;
  cancellable?: boolean;
};

function createMenu({
  title = "Foo",
  body = "Bar",
  buttonText = "Baz",
  cancellable = true,
}: MenuConfig) {
  // ...
}

createMenu({ body: "Bar" });
```

เพื่อหลีกเลี่ยงการเกิด side effects และ unexpected behavior โดยการผ่านค่าให้ชัดเจนไปเลย เช่น `undefined` หรือ `null` คุณสามารถบอก TypeScript compiler ให้ปิดมัน
โดยดูการตั้งค่า TypeScript ที่ [`--strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks)

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### อย่าใช้ flags เป็นพารามิเตอร์ของฟังก์ชัน

Flags จะไว้บอกผู้ใช้ของคุณว่าฟังก์ชันนี้ทำมากกว่าหนึ่งอย่าง ฟังก์ชันควรทำแค่อย่างเดียว แยกฟังก์ชันของคุณออกมา ถ้าพวกเขากำลังไล่หาโค้ดผิดที่จากโค้ดใช้ boolean ที่แตกต่างกัน

**ไม่ดี:**

```ts
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**ดี:**

```ts
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### หลีกเลี่ยง Side Effects (ตอนที่ 1)

ฟังก์ชันอาจจะทำให้เกิด side effect ได้ ถ้าหากมีการทำอย่างอื่นมากกว่ารับค่าส่งค่า ซึ่ง side effect จะเกิดขึ้นตอนที่มีการเขียนไฟล์เดียวกัน, เปลี่ยนค่าในตัวแปรที่เป็น global หรือการเผลอจ่ายเงินทั้งหมดไปให้คนแปลกหน้า

ตอนนี้คุณอาจจะมีส่วนที่ทำให้เกิด side effect อยู่บ้างในโปรแกรมของคุณ เช่นเดียวกับตัวอย่างก่อนหน้านี้ คุณอาจจะเขียนไฟล์เดียวกันอยู่ สิ่งที่คุณควรทำคือการหาตัวกลางมาทำแทน อย่าให้มีหลายฟังก์ชันหรือคลาสเขียนข้อมูลลงไฟล์เดียวกัน ให้มีแค่ service เดียวเท่านั้นที่บันเขียนไฟล์นั้นได้ ขอย้ำอีกครั้งว่า แค่ service เดียวเท่านั้น

อีกประเด็นสำคัญคือการพยายามหลีกเลี่ยง side effect ทั่วไป เช่นการใช้ state ร่ามกันระหว่าง object โดยไม่มีโครงสร้างใด ๆ ด้วยการใช้ชนิดข้อมูลที่สามารถเปลี่ยนแปลงได้ ซึ่งสามารถเขียนด้วยอะไรก็ได้ และจะไม่เป็นตัวกลางที่จะทำให้ side effect เกิดขึ้น ถ้าคุณสามารถทำได้เช่นนี้แล้ว คุณก็จะมีควารสุขขึ้นมากกว่าโปรแกรมเมอร์คนอื่น ๆ ส่วนใหญ่แน่นอน

**ไม่ดี:**

```ts
// Global variable referenced by following function.
let name = "Robert C. Martin";

function toBase64() {
  name = btoa(name);
}

toBase64();
// If we had another function that used this name, now it'd be a Base64 value

console.log(name); // expected to print 'Robert C. Martin' but instead 'Um9iZXJ0IEMuIE1hcnRpbg=='
```

**ดี:**

```ts
const name = "Robert C. Martin";

function toBase64(text: string): string {
  return btoa(text);
}

const encodedName = toBase64(name);
console.log(name);
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### หลีกเลี่ยง Side Effects (ตอนที่ 2)

ใน JavaScript ยุคแรก ๆ มีการผ่านค่า value และ objects/arrays ด้วยการอ้างอิง ในกรณีที่เป็น objects และ arrays ถ้าฟังก์ชันของคุณเป็นฟังก์ชันที่ทำการเปลี่ยนข้อมูล array ในตะกร้าสินค้า ตัวอย่างเช่น ฟังก์ชันการเพิ่มสินค้าที่จะซื้อ แต่มีฟังก์ชั่นอื่น ๆ ก็กำลังใช้อาเรย์ `cart` นั้นอยู่ด้วย จะทำให้มีผลกระทบกับการเพิ่มสินค้าได้ ซึ่งนั่นก็อาจจะดีนะ แต่มันก็แย่เหมือนกัน ลองคิดดูว่าถ้ามีเหตุการณ์แย่ ๆ ประมาณนี้:

ผู้ใช้กดปุ่ม "สั่งซื้อ" ซึ่งปุ่มนี้มันเรียกฟังก์ชัน `purchase` ที่จะสร้าง network request และส่งอาเรย์ `cart` ไปยังเซิร์ฟเวอร์ด้วย แต่เนื่องจากสัญญาณเครือข่ายไม่ดีทำให้ฟังก์ชัน purchase มีการลองส่งข้อมูลไปอีกครั้ง แล้วจะเกิดอะไรขึ้นถ้าระหว่างนี้ผู้ใช้เผลอกดปุ่ม "เพิ่มลงตะกร้า" กับสินค้าที่ไม่ได้ตั้งใจจะซื้อจริง ๆ ก่อนที่ network request จะเริ่มขึ้น? ถ้าหากเป็นแบบนี้จะทำให้ network request เริ่มส่งข้อมูลใหม่แล้วทำให้ฟังก์ชัน purchase จะส่งข้อมูลสินค้าที่เผลอกดไปด้วย เพราะว่ามันอ้างอิงข้อมูลจากอาเรย์ cart ที่ฟังก์ชัน `addItemToCart` แก้ไขข้อมูลสินค้าในตะกร้าโดยเพิ่มสินค้าที่ไม่ต้องการลงไปด้วย

ทางออกที่ดีคือให้ฟังก์ชัน `addItemToCart` ทำการโคลนอาเรย์ `cart` แก้ไข และคืนค่าที่โคลนไว้ออกไป วิธีนี้จะทำให้มั่นใจได้ว่าจะไม่มีผลกระทบการเปลี่ยนแปลงข้อมูลตะกร้าสินค้าของฟังก์ชันอื่นที่อ้างอิงข้อมูลตะกร้าสินค้าเดียวกันนี้

มีสองข้อแม้เกียวกับแนวทางนี้คือ:

1. อาจมีบางกรณีที่คุณต้องการแก้ไข input object จริง ๆ  แต่เมื่อคุณทำตามแนวทางการเขียนโปรแกรมนี้ คุณจะพบว่ามันจะเกิดขึ้นได้ยากมาก และส่วนใหญ่จะสามารถปรับให้มันไม่มี side effects เกิดขึ้นได้! (ดูเพิ่มเรื่อง [pure function](https://en.wikipedia.org/wiki/Pure_function))

2. การโคลน object ขนาดใหญ่อาจทำให้เสีย performance ไปมาก แต่โดยดีที่อาจจะไม่ใช่ปัญหาใหญ่ในทางปฏิบัติ เพราะเรามีสุดยอด library ที่ช่วยให้วิธีการเขียนโปรแกรมวิธีนี้ทำได้รวดเร็วและไม่ต้องใช้หน่วยความจำมากเท่ากับการโคลน objects และ arrays ด้วยตนเอง

**ไม่ดี:**

```ts
function addItemToCart(cart: CartItem[], item: Item): void {
  cart.push({ item, date: Date.now() });
}
```

**ดี:**

```ts
function addItemToCart(cart: CartItem[], item: Item): CartItem[] {
  return [...cart, { item, date: Date.now() }];
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Don't write to global functions

Polluting globals is a bad practice in JavaScript because you could clash with another library and the user of your API would be none-the-wiser until they get an exception in production. Let's think about an example: what if you wanted to extend JavaScript's native Array method to have a `diff` method that could show the difference between two arrays? You could write your new function to the `Array.prototype`, but it could clash with another library that tried to do the same thing. What if that other library was just using `diff` to find the difference between the first and last elements of an array? This is why it would be much better to just use classes and simply extend the `Array` global.

**ไม่ดี:**

```ts
declare global {
  interface Array<T> {
    diff(other: T[]): Array<T>;
  }
}

if (!Array.prototype.diff) {
  Array.prototype.diff = function <T>(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter((elem) => !hash.has(elem));
  };
}
```

**ดี:**

```ts
class MyArray<T> extends Array<T> {
  diff(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter((elem) => !hash.has(elem));
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Favor functional programming over imperative programming

Favor this style of programming when you can.

**ไม่ดี:**

```ts
const contributions = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500,
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500,
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150,
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000,
  },
];

let totalOutput = 0;

for (let i = 0; i < contributions.length; i++) {
  totalOutput += contributions[i].linesOfCode;
}
```

**ดี:**

```ts
const contributions = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500,
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500,
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150,
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000,
  },
];

const totalOutput = contributions.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Encapsulate conditionals

**ไม่ดี:**

```ts
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**ดี:**

```ts
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Avoid negative conditionals

**ไม่ดี:**

```ts
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**ดี:**

```ts
function isEmailUsed(email: string): boolean {
  // ...
}

if (!isEmailUsed(node)) {
  // ...
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Avoid conditionals

This seems like an impossible task. Upon first hearing this, most people say, "how am I supposed to do anything without an `if` statement?" The answer is that you can use polymorphism to achieve the same task in many cases. The second question is usually, "well that's great but why would I want to do that?" The answer is a previous clean code concept we learned: a function should only do one thing. When you have classes and functions that have `if` statements, you are telling your user that your function does more than one thing. Remember, just do one thing.

**ไม่ดี:**

```ts
class Airplane {
  private type: string;
  // ...

  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
      default:
        throw new Error("Unknown airplane type.");
    }
  }

  private getMaxAltitude(): number {
    // ...
  }
}
```

**ดี:**

```ts
abstract class Airplane {
  protected getMaxAltitude(): number {
    // shared logic with subclasses ...
  }

  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Avoid type checking

TypeScript is a strict syntactical superset of JavaScript and adds optional static type checking to the language.
Always prefer to specify types of variables, parameters and return values to leverage the full power of TypeScript features.
It makes refactoring more easier.

**ไม่ดี:**

```ts
function travelToTexas(vehicle: Bicycle | Car) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(currentLocation, new Location("texas"));
  }
}
```

**ดี:**

```ts
type Vehicle = Bicycle | Car;

function travelToTexas(vehicle: Vehicle) {
  vehicle.move(currentLocation, new Location("texas"));
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Don't over-optimize

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of times, if you are optimizing then you are just wasting your time. There are good [resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) for seeing where optimization is lacking. Target those in the meantime, until they are fixed if they can be.

**ไม่ดี:**

```ts
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**ดี:**

```ts
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Remove dead code

Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase.
If it's not being called, get rid of it! It will still be safe in your version history if you still need it.

**ไม่ดี:**

```ts
function oldRequestModule(url: string) {
  // ...
}

function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**ดี:**

```ts
function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Use iterators and generators

Use generators and iterables when working with collections of data used like a stream.  
There are some good reasons:

- decouples the callee from the generator implementation in a sense that callee decides how many
  items to access
- lazy execution, items are streamed on-demand
- built-in support for iterating items using the `for-of` syntax
- iterables allow implementing optimized iterator patterns

**ไม่ดี:**

```ts
function fibonacci(n: number): number[] {
  if (n === 1) return [0];
  if (n === 2) return [0, 1];

  const items: number[] = [0, 1];
  while (items.length < n) {
    items.push(items[items.length - 2] + items[items.length - 1]);
  }

  return items;
}

function print(n: number) {
  fibonacci(n).forEach((fib) => console.log(fib));
}

// Print first 10 Fibonacci numbers.
print(10);
```

**ดี:**

```ts
// Generates an infinite stream of Fibonacci numbers.
// The generator doesn't keep the array of all numbers.
function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function print(n: number) {
  let i = 0;
  for (const fib of fibonacci()) {
    if (i++ === n) break;
    console.log(fib);
  }
}

// Print first 10 Fibonacci numbers.
print(10);
```

There are libraries that allow working with iterables in a similar way as with native arrays, by
chaining methods like `map`, `slice`, `forEach` etc. See [itiriri](https://www.npmjs.com/package/itiriri) for
an example of advanced manipulation with iterables (or [itiriri-async](https://www.npmjs.com/package/itiriri-async) for manipulation of async iterables).

```ts
import itiriri from "itiriri";

function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

itiriri(fibonacci())
  .take(10)
  .forEach((fib) => console.log(fib));
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## อ็อบเจกต์และโครงสร้างข้อมูล

### ใช้ getters และ setters

TypeScript รองรับการใช้งาน getter/setter syntax.
การใช้ getters และ setters เพื่อเข้าถึงข้อมูลในอ็อบเจกต์ จะทำให้ได้ใช้ behavior ซึ่งจะมีกว่าการแค่มองหา property ใน object เพียงอย่างเดียว คุณอาจถามว่า "แล้วมันดียังไงหรอ" นี่คือเหตุผลครับ:

- เมื่อคุณต้องการทำมากกว่าแค่ดึงค่า property ใน object คุณไม่จำเป็นต้องคอยไล่หาและเปลี่ยนทุกตัวเข้าถึงใน codebase ของคุณ
- การเขียน validation เพิ่มเข้าไปได้ง่ายขึ้นด้วยการใช้ `set`
- มีการห่อหุ้มพวก representation ภายในด้วย
- การเก็บ log และจัดการ error ที่เกิดขึ้นตอน getting และ setting ทำได้ง่าย
- คุณสามารถทำ lazy load ให้กับ properties ใน object ได้ด้วย โดยสมมติว่ามีการ getting ข้อมูลมาจากเซิร์ฟเวอร์

**ไม่ดี:**

```ts
type BankAccount = {
  balance: number;
  // ...
};

const value = 100;
const account: BankAccount = {
  balance: 0,
  // ...
};

if (value < 0) {
  throw new Error("Cannot set negative balance.");
}

account.balance = value;
```

**ดี:**

```ts
class BankAccount {
  private accountBalance: number = 0;

  get balance(): number {
    return this.accountBalance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error("Cannot set negative balance.");
    }

    this.accountBalance = value;
  }

  // ...
}

// Now `BankAccount` encapsulates the validation logic.
// If one day the specifications change, and we need extra validation rule,
// we would have to alter only the `setter` implementation,
// leaving all dependent code unchanged.
const account = new BankAccount();
account.balance = 100;
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Make objects have private/protected members

TypeScript supports `public` _(default)_, `protected` and `private` accessors on class members.

**ไม่ดี:**

```ts
class Circle {
  radius: number;

  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**ดี:**

```ts
class Circle {
  constructor(private readonly radius: number) {}

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Prefer immutability

TypeScript's type system allows you to mark individual properties on an interface/class as _readonly_. This allows you to work in a functional way (an unexpected mutation is bad).  
For more advanced scenarios there is a built-in type `Readonly` that takes a type `T` and marks all of its properties as readonly using mapped types (see [mapped types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)).

**ไม่ดี:**

```ts
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**ดี:**

```ts
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```

Case of Array, you can create a read-only array by using `ReadonlyArray<T>`.
do not allow changes such as `push()` and `fill()`, but can use features such as `concat()` and `slice()` that do not change the value.

**ไม่ดี:**

```ts
const array: number[] = [1, 3, 5];
array = []; // error
array.push(100); // array will updated
```

**ดี:**

```ts
const array: ReadonlyArray<number> = [1, 3, 5];
array = []; // error
array.push(100); // error
```

Declaring read-only arguments in [TypeScript 3.4 is a bit easier](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#improvements-for-readonlyarray-and-readonly-tuples).

```ts
function hoge(args: readonly string[]) {
  args.push(1); // error
}
```

Prefer [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions) for literal values.

**ไม่ดี:**

```ts
const config = {
  hello: "world",
};
config.hello = "world"; // value is changed

const array = [1, 3, 5];
array[0] = 10; // value is changed

// writable objects is returned
function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // value is changed
```

**ดี:**

```ts
// read-only object
const config = {
  hello: "world",
} as const;
config.hello = "world"; // error

// read-only array
const array = [1, 3, 5] as const;
array[0] = 10; // error

// You can return read-only objects
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // error
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### type vs. interface

Use type when you might need a union or intersection. Use an interface when you want `extends` or `implements`. There is no strict rule, however, use the one that works for you.  
For a more detailed explanation refer to this [answer](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/54101543#54101543) about the differences between `type` and `interface` in TypeScript.

**ไม่ดี:**

```ts
interface EmailConfig {
  // ...
}

interface DbConfig {
  // ...
}

interface Config {
  // ...
}

//...

type Shape = {
  // ...
};
```

**ดี:**

```ts
type EmailConfig = {
  // ...
};

type DbConfig = {
  // ...
};

type Config = EmailConfig | DbConfig;

// ...

interface Shape {
  // ...
}

class Circle implements Shape {
  // ...
}

class Square implements Shape {
  // ...
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## Classes

### Classes should be small

The class' size is measured by its responsibility. Following the _Single Responsibility principle_ a class should be small.

**ไม่ดี:**

```ts
class Dashboard {
  getLanguage(): string {
    /* ... */
  }
  setLanguage(language: string): void {
    /* ... */
  }
  showProgress(): void {
    /* ... */
  }
  hideProgress(): void {
    /* ... */
  }
  isDirty(): boolean {
    /* ... */
  }
  disable(): void {
    /* ... */
  }
  enable(): void {
    /* ... */
  }
  addSubscription(subscription: Subscription): void {
    /* ... */
  }
  removeSubscription(subscription: Subscription): void {
    /* ... */
  }
  addUser(user: User): void {
    /* ... */
  }
  removeUser(user: User): void {
    /* ... */
  }
  goToHomePage(): void {
    /* ... */
  }
  updateProfile(details: UserDetails): void {
    /* ... */
  }
  getVersion(): string {
    /* ... */
  }
  // ...
}
```

**ดี:**

```ts
class Dashboard {
  disable(): void {
    /* ... */
  }
  enable(): void {
    /* ... */
  }
  getVersion(): string {
    /* ... */
  }
}

// split the responsibilities by moving the remaining methods to other classes
// ...
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### High cohesion and low coupling

Cohesion defines the degree to which class members are related to each other. Ideally, all fields within a class should be used by each method.
We then say that the class is _maximally cohesive_. In practice, this, however, is not always possible, nor even advisable. You should however prefer cohesion to be high.

Coupling refers to how related or dependent are two classes toward each other. Classes are said to be low coupled if changes in one of them don't affect the other one.

Good software design has **high cohesion** and **low coupling**.

**ไม่ดี:**

```ts
class UserManager {
  // Bad: each private variable is used by one or another group of methods.
  // It makes clear evidence that the class is holding more than a single responsibility.
  // If I need only to create the service to get the transactions for a user,
  // I'm still forced to pass and instance of `emailSender`.
  constructor(
    private readonly db: Database,
    private readonly emailSender: EmailSender
  ) {}

  async getUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await db.transactions.find({ userId });
  }

  async sendGreeting(): Promise<void> {
    await emailSender.send("Welcome!");
  }

  async sendNotification(text: string): Promise<void> {
    await emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**ดี:**

```ts
class UserService {
  constructor(private readonly db: Database) {}

  async getUser(id: number): Promise<User> {
    return await this.db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await this.db.transactions.find({ userId });
  }
}

class UserNotifier {
  constructor(private readonly emailSender: EmailSender) {}

  async sendGreeting(): Promise<void> {
    await this.emailSender.send("Welcome!");
  }

  async sendNotification(text: string): Promise<void> {
    await this.emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Prefer composition over inheritance

As stated famously in [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four, you should _prefer composition over inheritance_ where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition. The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.

You might be wondering then, "when should I use inheritance?" It depends on your problem at hand, but this is a decent list of when inheritance makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a" relationship (Human->Animal vs. User->UserDetails).

2. You can reuse code from the base classes (Humans can move like all animals).

3. You want to make global changes to derived classes by changing a base class. (Change the caloric expenditure of all animals when they move).

**ไม่ดี:**

```ts
class Employee {
  constructor(private readonly name: string, private readonly email: string) {}

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(
    name: string,
    email: string,
    private readonly ssn: string,
    private readonly salary: number
  ) {
    super(name, email);
  }

  // ...
}
```

**ดี:**

```ts
class Employee {
  private taxData: EmployeeTaxData;

  constructor(private readonly name: string, private readonly email: string) {}

  setTaxData(ssn: string, salary: number): Employee {
    this.taxData = new EmployeeTaxData(ssn, salary);
    return this;
  }

  // ...
}

class EmployeeTaxData {
  constructor(public readonly ssn: string, public readonly salary: number) {}

  // ...
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Use method chaining

This pattern is very useful and commonly used in many libraries. It allows your code to be expressive, and less verbose. For that reason, use method chaining and take a look at how clean your code will be.

**ไม่ดี:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): void {
    this.collection = collection;
  }

  page(number: number, itemsPerPage: number = 100): void {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
  }

  orderBy(...fields: string[]): void {
    this.orderByFields = fields;
  }

  build(): Query {
    // ...
  }
}

// ...

const queryBuilder = new QueryBuilder();
queryBuilder.from("users");
queryBuilder.page(1, 100);
queryBuilder.orderBy("firstName", "lastName");

const query = queryBuilder.build();
```

**ดี:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): this {
    this.collection = collection;
    return this;
  }

  page(number: number, itemsPerPage: number = 100): this {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
    return this;
  }

  orderBy(...fields: string[]): this {
    this.orderByFields = fields;
    return this;
  }

  build(): Query {
    // ...
  }
}

// ...

const query = new QueryBuilder()
  .from("users")
  .page(1, 100)
  .orderBy("firstName", "lastName")
  .build();
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## SOLID

### Single Responsibility Principle (SRP)

As stated in Clean Code, "There should never be more than one reason for a class to change". It's tempting to jam-pack a class with a lot of functionality, like when you can only take one suitcase on your flight. The issue with this is that your class won't be conceptually cohesive and it will give it many reasons to change. Minimizing the amount of time you need to change a class is important. It's important because if too much functionality is in one class and you modify a piece of it, it can be difficult to understand how that will affect other dependent modules in your codebase.

**ไม่ดี:**

```ts
class UserSettings {
  constructor(private readonly user: User) {}

  changeSettings(settings: UserSettings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**ดี:**

```ts
class UserAuth {
  constructor(private readonly user: User) {}

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  private readonly auth: UserAuth;

  constructor(private readonly user: User) {
    this.auth = new UserAuth(user);
  }

  changeSettings(settings: UserSettings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification." What does that mean though? This principle basically states that you should allow users to add new functionalities without changing existing code.

**ไม่ดี:**

```ts
class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {}

  async fetch<T>(url: string): Promise<T> {
    if (this.adapter instanceof AjaxAdapter) {
      const response = await makeAjaxCall<T>(url);
      // transform response and return
    } else if (this.adapter instanceof NodeAdapter) {
      const response = await makeHttpCall<T>(url);
      // transform response and return
    }
  }
}

function makeAjaxCall<T>(url: string): Promise<T> {
  // request and return promise
}

function makeHttpCall<T>(url: string): Promise<T> {
  // request and return promise
}
```

**ดี:**

```ts
abstract class Adapter {
  abstract async request<T>(url: string): Promise<T>;

  // code shared to subclasses ...
}

class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T> {
    // request and return promise
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T> {
    // request and return promise
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {}

  async fetch<T>(url: string): Promise<T> {
    const response = await this.adapter.request<T>(url);
    // transform response and return
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S is a subtype of T, then objects of type T may be replaced with objects of type S (i.e., objects of type S may substitute objects of type T) without altering any of the desirable properties of that program (correctness, task performed, etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class, then the parent class and child class can be used interchangeably without getting incorrect results. This might still be confusing, so let's take a look at the classic Square-Rectangle example. Mathematically, a square is a rectangle, but if you model it using the "is-a" relationship via inheritance, you quickly get into trouble.

**ไม่ดี:**

```ts
class Rectangle {
  constructor(protected width: number = 0, protected height: number = 0) {}

  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  setWidth(width: number): this {
    this.width = width;
    return this;
  }

  setHeight(height: number): this {
    this.height = height;
    return this;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): this {
    this.width = width;
    this.height = width;
    return this;
  }

  setHeight(height: number): this {
    this.width = height;
    this.height = height;
    return this;
  }
}

function renderLargeRectangles(rectangles: Rectangle[]) {
  rectangles.forEach((rectangle) => {
    const area = rectangle.setWidth(4).setHeight(5).getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**ดี:**

```ts
abstract class Shape {
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  abstract getArea(): number;
}

class Rectangle extends Shape {
  constructor(private readonly width = 0, private readonly height = 0) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private readonly length: number) {
    super();
  }

  getArea(): number {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes: Shape[]) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Interface Segregation Principle (ISP)

ISP states that "Clients should not be forced to depend upon interfaces that they do not use.". This principle is very much related to the Single Responsibility Principle.
What it really means is that you should always design your abstractions in a way that the clients that are using the exposed methods do not get the whole pie instead. That also include imposing the clients with the burden of implementing methods that they don’t actually need.

**ไม่ดี:**

```ts
interface SmartPrinter {
  print();
  fax();
  scan();
}

class AllInOnePrinter implements SmartPrinter {
  print() {
    // ...
  }

  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements SmartPrinter {
  print() {
    // ...
  }

  fax() {
    throw new Error("Fax not supported.");
  }

  scan() {
    throw new Error("Scan not supported.");
  }
}
```

**ดี:**

```ts
interface Printer {
  print();
}

interface Fax {
  fax();
}

interface Scanner {
  scan();
}

class AllInOnePrinter implements Printer, Fax, Scanner {
  print() {
    // ...
  }

  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements Printer {
  print() {
    // ...
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.

2. Abstractions should not depend upon details. Details should depend on abstractions.

This can be hard to understand at first, but if you've worked with Angular, you've seen an implementation of this principle in the form of Dependency Injection (DI). While they are not identical concepts, DIP keeps high-level modules from knowing the details of its low-level modules and setting them up. It can accomplish this through DI. A huge benefit of this is that it reduces the coupling between modules. Coupling is a very bad development pattern because it makes your code hard to refactor.

DIP is usually achieved by a using an inversion of control (IoC) container. An example of a powerful IoC container for TypeScript is [InversifyJs](https://www.npmjs.com/package/inversify)

**ไม่ดี:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

class XmlFormatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}

class ReportReader {

  // BAD: We have created a dependency on a specific request implementation.
  // We should just have ReportReader depend on a parse method: `parse`
  private readonly formatter = new XmlFormatter();

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader();
await report = await reader.read('report.xml');
```

**ดี:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

interface Formatter {
  parse<T>(content: string): T;
}

class XmlFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}


class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts a JSON string to an object T
  }
}

class ReportReader {
  constructor(private readonly formatter: Formatter) {
  }

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader(new XmlFormatter());
await report = await reader.read('report.xml');

// or if we had to read a json report
const reader = new ReportReader(new JsonFormatter());
await report = await reader.read('report.json');
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## Testing

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything.
Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches)
is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a good [coverage tool](https://github.com/gotwarlost/istanbul).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools) with typings support for TypeScript, so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.

### The three laws of TDD

1. You are not allowed to write any production code unless it is to make a failing unit test pass.

2. You are not allowed to write any more of a unit test than is sufficient to fail, and; compilation failures are failures.

3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### F.I.R.S.T. rules

Clean tests should follow the rules:

- **Fast** tests should be fast because we want to run them frequently.

- **Independent** tests should not depend on each other. They should provide same output whether run independently or all together in any order.

- **Repeatable** tests should be repeatable in any environment and there should be no excuse for why they fail.

- **Self-Validating** a test should answer with either _Passed_ or _Failed_. You don't need to compare log files to answer if a test passed.

- **Timely** unit tests should be written before the production code. If you write tests after the production code, you might find writing tests too hard.

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Single concept per test

Tests should also follow the _Single Responsibility Principle_. Make only one assert per unit test.

**ไม่ดี:**

```ts
import { assert } from "chai";

describe("AwesomeDate", () => {
  it("handles date boundaries", () => {
    let date: AwesomeDate;

    date = new AwesomeDate("1/1/2015");
    assert.equal("1/31/2015", date.addDays(30));

    date = new AwesomeDate("2/1/2016");
    assert.equal("2/29/2016", date.addDays(28));

    date = new AwesomeDate("2/1/2015");
    assert.equal("3/1/2015", date.addDays(28));
  });
});
```

**ดี:**

```ts
import { assert } from "chai";

describe("AwesomeDate", () => {
  it("handles 30-day months", () => {
    const date = new AwesomeDate("1/1/2015");
    assert.equal("1/31/2015", date.addDays(30));
  });

  it("handles leap year", () => {
    const date = new AwesomeDate("2/1/2016");
    assert.equal("2/29/2016", date.addDays(28));
  });

  it("handles non-leap year", () => {
    const date = new AwesomeDate("2/1/2015");
    assert.equal("3/1/2015", date.addDays(28));
  });
});
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### The name of the test should reveal its intention

When a test fails, its name is the first indication of what may have gone wrong.

**ไม่ดี:**

```ts
describe("Calendar", () => {
  it("2/29/2020", () => {
    // ...
  });

  it("throws", () => {
    // ...
  });
});
```

**ดี:**

```ts
describe("Calendar", () => {
  it("should handle leap year", () => {
    // ...
  });

  it("should throw when format is invalid", () => {
    // ...
  });
});
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## Concurrency

### Prefer promises vs callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting _(the callback hell)_.  
There are utilities that transform existing functions using the callback style to a version that returns promises
(for Node.js see [`util.promisify`](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original), for general purpose see [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify))

**ไม่ดี:**

```ts
import { get } from "request";
import { writeFile } from "fs";

function downloadPage(
  url: string,
  saveTo: string,
  callback: (error: Error, content?: string) => void
) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  "article.html",
  (error, content) => {
    if (error) {
      console.error(error);
    } else {
      console.log(content);
    }
  }
);
```

**ดี:**

```ts
import { get } from "request";
import { writeFile } from "fs";
import { promisify } from "util";

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then((response) => write(saveTo, response));
}

downloadPage(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  "article.html"
)
  .then((content) => console.log(content))
  .catch((error) => console.error(error));
```

Promises supports a few helper methods that help make code more concise:

| Pattern                  | Description                                                                                                                                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Promise.resolve(value)` | Convert a value into a resolved promise.                                                                                                                           |
| `Promise.reject(error)`  | Convert an error into a rejected promise.                                                                                                                          |
| `Promise.all(promises)`  | Returns a new promise which is fulfilled with an array of fulfillment values for the passed promises or rejects with the reason of the first promise that rejects. |
| `Promise.race(promises)` | Returns a new promise which is fulfilled/rejected with the result/error of the first settled promise from the array of passed promises.                            |

`Promise.all` is especially useful when there is a need to run tasks in parallel. `Promise.race` makes it easier to implement things like timeouts for promises.

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Async/Await are even cleaner than Promises

With `async`/`await` syntax you can write code that is far cleaner and more understandable than chained promises. Within a function prefixed with `async` keyword, you have a way to tell the JavaScript runtime to pause the execution of code on the `await` keyword (when used on a promise).

**ไม่ดี:**

```ts
import { get } from "request";
import { writeFile } from "fs";
import { promisify } from "util";

const write = util.promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then((response) => write(saveTo, response));
}

downloadPage(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  "article.html"
)
  .then((content) => console.log(content))
  .catch((error) => console.error(error));
```

**ดี:**

```ts
import { get } from "request";
import { writeFile } from "fs";
import { promisify } from "util";

const write = promisify(writeFile);

async function downloadPage(url: string, saveTo: string): Promise<string> {
  const response = await get(url);
  await write(saveTo, response);
  return response;
}

// somewhere in an async function
try {
  const content = await downloadPage(
    "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
    "article.html"
  );
  console.log(content);
} catch (error) {
  console.error(error);
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## Error Handling

Thrown errors are a good thing! They mean the runtime has successfully identified when something in your program has gone wrong and it's letting you know by stopping function
execution on the current stack, killing the process (in Node), and notifying you in the console with a stack trace.

### Always use Error for throwing or rejecting

JavaScript as well as TypeScript allow you to `throw` any object. A Promise can also be rejected with any reason object.  
It is advisable to use the `throw` syntax with an `Error` type. This is because your error might be caught in higher level code with a `catch` syntax.
It would be very confusing to catch a string message there and would make
[debugging more painful](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).  
For the same reason you should reject promises with `Error` types.

**ไม่ดี:**

```ts
function calculateTotal(items: Item[]): number {
  throw "Not implemented.";
}

function get(): Promise<Item[]> {
  return Promise.reject("Not implemented.");
}
```

**ดี:**

```ts
function calculateTotal(items: Item[]): number {
  throw new Error("Not implemented.");
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error("Not implemented."));
}

// or equivalent to:

async function get(): Promise<Item[]> {
  throw new Error("Not implemented.");
}
```

The benefit of using `Error` types is that it is supported by the syntax `try/catch/finally` and implicitly all errors have the `stack` property which
is very powerful for debugging.  
There are also other alternatives, not to use the `throw` syntax and instead always return custom error objects. TypeScript makes this even easier.
Consider the following example:

```ts
type Result<R> = { isError: false; value: R };
type Failure<E> = { isError: true; error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, "empty"> {
  if (items.length === 0) {
    return { isError: true, error: "empty" };
  }

  // ...
  return { isError: false, value: 42 };
}
```

For the detailed explanation of this idea refer to the [original post](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix or react to said error. Logging the error to the console (`console.log`) isn't much better as often it can get lost in a sea of things printed to the console. If you wrap any bit of code in a `try/catch` it means you think an error may occur there and therefore you should have a plan, or create a code path, for when it occurs.

**ไม่ดี:**

```ts
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}

// or even worse

try {
  functionThatMightThrow();
} catch (error) {
  // ignore error
}
```

**ดี:**

```ts
import { logger } from "./logging";

try {
  functionThatMightThrow();
} catch (error) {
  logger.log(error);
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Don't ignore rejected promises

For the same reason you shouldn't ignore caught errors from `try/catch`.

**ไม่ดี:**

```ts
getUser()
  .then((user: User) => {
    return sendEmail(user.email, "Welcome!");
  })
  .catch((error) => {
    console.log(error);
  });
```

**ดี:**

```ts
import { logger } from "./logging";

getUser()
  .then((user: User) => {
    return sendEmail(user.email, "Welcome!");
  })
  .catch((error) => {
    logger.log(error);
  });

// or using the async/await syntax:

try {
  const user = await getUser();
  await sendEmail(user.email, "Welcome!");
} catch (error) {
  logger.log(error);
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## Formatting

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is _DO NOT ARGUE_ over formatting. There are tons of tools to automate this. Use one! It's a waste of time and money for engineers to argue over formatting. The general rule to follow is _keep consistent formatting rules_.

For TypeScript there is a powerful tool called [TSLint](https://palantir.github.io/tslint/). It's a static analysis tool that can help you improve dramatically the readability and maintainability of your code. There are ready to use TSLint configurations that you can reference in your projects:

- [TSLint Config Standard](https://www.npmjs.com/package/tslint-config-standard) - standard style rules

- [TSLint Config Airbnb](https://www.npmjs.com/package/tslint-config-airbnb) - Airbnb style guide

- [TSLint Clean Code](https://www.npmjs.com/package/tslint-clean-code) - TSLint rules inspired by the [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.ca/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)

- [TSLint react](https://www.npmjs.com/package/tslint-react) - lint rules related to React & JSX

- [TSLint + Prettier](https://www.npmjs.com/package/tslint-config-prettier) - lint rules for [Prettier](https://github.com/prettier/prettier) code formatter

- [ESLint rules for TSLint](https://www.npmjs.com/package/tslint-eslint-rules) - ESLint rules for TypeScript

- [Immutable](https://www.npmjs.com/package/tslint-immutable) - rules to disable mutation in TypeScript

Refer also to this great [TypeScript StyleGuide and Coding Conventions](https://basarat.gitbook.io/typescript/styleguide) source.

### Use consistent capitalization

Capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just _be consistent_.

**ไม่ดี:**

```ts
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

type animal = {
  /* ... */
};
type Container = {
  /* ... */
};
```

**ดี:**

```ts
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = {
  /* ... */
};
type Container = {
  /* ... */
};
```

Prefer using `PascalCase` for class, interface, type and namespace names.  
Prefer using `camelCase` for variables, functions and class members.

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee.
We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

**ไม่ดี:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {}

  private lookupPeers() {
    return db.lookup(this.employee.id, "peers");
  }

  private lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**ดี:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {}

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, "peers");
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Organize imports

With clean and easy to read import statements you can quickly see the dependencies of current code. Make sure you apply following good practices for `import` statements:

- Import statements should be alphabetized and grouped.
- Unused imports should be removed.
- Named imports must be alphabetized (i.e. `import {A, B, C} from 'foo';`)
- Import sources must be alphabetized within groups, i.e.: `import * as foo from 'a'; import * as bar from 'b';`
- Groups of imports are delineated by blank lines.
- Groups must respect following order:
  - Polyfills (i.e. `import 'reflect-metadata';`)
  - Node builtin modules (i.e. `import fs from 'fs';`)
  - external modules (i.e. `import { query } from 'itiriri';`)
  - internal modules (i.e `import { UserService } from 'src/services/userService';`)
  - modules from a parent directory (i.e. `import foo from '../foo'; import qux from '../../foo/qux';`)
  - modules from the same or a sibling's directory (i.e. `import bar from './bar'; import baz from './bar/baz';`)

**ไม่ดี:**

```ts
import { TypeDefinition } from "../types/typeDefinition";
import { AttributeTypes } from "../model/attribute";
import { ApiCredentials, Adapters } from "./common/api/authorization";
import fs from "fs";
import { ConfigPlugin } from "./plugins/config/configPlugin";
import { BindingScopeEnum, Container } from "inversify";
import "reflect-metadata";
```

**ดี:**

```ts
import "reflect-metadata";

import fs from "fs";
import { BindingScopeEnum, Container } from "inversify";

import { AttributeTypes } from "../model/attribute";
import { TypeDefinition } from "../types/typeDefinition";

import { ApiCredentials, Adapters } from "./common/api/authorization";
import { ConfigPlugin } from "./plugins/config/configPlugin";
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Use typescript aliases

Create prettier imports by defining the paths and baseUrl properties in the compilerOptions section in the `tsconfig.json`

This will avoid long relative paths when doing imports.

**ไม่ดี:**

```ts
import { UserService } from "../../../services/UserService";
```

**ดี:**

```ts
import { UserService } from "@services/UserService";
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@services": ["services/*"]
    }
    ...
  }
...
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## Comments

The use of a comments is an indication of failure to express without them. Code should be the only source of truth.

> Don’t comment bad code—rewrite it.  
> — _Brian W. Kernighan and P. J. Plaugher_

### Prefer self explanatory code instead of comments

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**ไม่ดี:**

```ts
// Check if subscription is active.
if (subscription.endDate > Date.now) {
}
```

**ดี:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) {
  /* ... */
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**ไม่ดี:**

```ts
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
};
```

**ดี:**

```ts
type User = {
  name: string;
  email: string;
};
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use `git log` to get history!

**ไม่ดี:**

```ts
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**ดี:**

```ts
function combine(a: number, b: number): number {
  return a + b;
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the proper indentation and formatting give the visual structure to your code.  
Most IDE support code folding feature that allows you to collapse/expand blocks of code (see Visual Studio Code [folding regions](https://code.visualstudio.com/updates/v1_17#_folding-regions)).

**ไม่ดี:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Client class
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public methods
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private methods
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
}
```

**ดี:**

```ts
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

### TODO comments

When you find yourself that you need to leave notes in the code for some later improvements,
do that using `// TODO` comments. Most IDE have special support for those kinds of comments so that
you can quickly go over the entire list of todos.

Keep in mind however that a _TODO_ comment is not an excuse for bad code.

**ไม่ดี:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**ดี:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ กลับสู่ด้านบน](#สารบัญ)**

## Translations

This is also available in other languages:

- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [vitorfreitas/clean-code-typescript](https://github.com/vitorfreitas/clean-code-typescript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese**:
  - [beginor/clean-code-typescript](https://github.com/beginor/clean-code-typescript)
  - [pipiliang/clean-code-typescript](https://github.com/pipiliang/clean-code-typescript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [ralflorent/clean-code-typescript](https://github.com/ralflorent/clean-code-typescript)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [mheob/clean-code-typescript](https://github.com/mheob/clean-code-typescript)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [MSakamaki/clean-code-typescript](https://github.com/MSakamaki/clean-code-typescript)
- ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [738/clean-code-typescript](https://github.com/738/clean-code-typescript)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Real001/clean-code-typescript](https://github.com/Real001/clean-code-typescript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [JoseDeFreitas/clean-code-typescript](https://github.com/JoseDeFreitas/clean-code-typescript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [ozanhonamlioglu/clean-code-typescript](https://github.com/ozanhonamlioglu/clean-code-typescript)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hoangsetup/clean-code-typescript](https://github.com/hoangsetup/clean-code-typescript)

References will be added once translations are completed.  
Check this [discussion](https://github.com/labs42io/clean-code-typescript/issues/15) for more details and progress.
You can make an indispensable contribution to _Clean Code_ community by translating this to your language.

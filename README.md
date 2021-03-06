# clean-code-javascript

## সুচিপত্র

1. [পরিচিতি](#পরিচিতি)
2. [ভ্যারিয়েবল](#ভ্যারিয়েবল)
3. [ফাংশন](#ফাংশন)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

## পরিচিতি

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)


সফটওয়্যার ইঞ্জিনিয়ারিং প্রিন্সিপাল Robert C. Martin এর বই
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
 থেকে জাভাস্ক্রিপ্টের জন্য নেওয়া হয়েছে। এটি কোন স্টাইল গাইড নয়। এই গাইডটি জাভাস্ক্রিপ্ট দিয়ে
[readable, reusable, এবং refactorable](https://github.com/ryanmcdermott/3rs-of-software-architecture)
সফটওয়্যার বানাতে সহয়তা করবে।

<!-- Not every principle herein has to be strictly followed, and even fewer will be
universally agreed upon. These are guidelines and nothing more, but they are
ones codified over many years of collective experience by the authors of
_Clean Code_.

Our craft of software engineering is just a bit over 50 years old, and we are
still learning a lot. When software architecture is as old as architecture
itself, maybe then we will have harder rules to follow. For now, let these
guidelines serve as a touchstone by which to assess the quality of the
JavaScript code that you and your team produce.

One more thing: knowing these won't immediately make you a better software
developer, and working with them for many years doesn't mean you won't make
mistakes. Every piece of code starts as a first draft, like wet clay getting
shaped into its final form. Finally, we chisel away the imperfections when
we review it with our peers. Don't beat yourself up for first drafts that need
improvement. Beat up the code instead! -->

## **ভ্যারিয়েবল**

### অর্থপূর্ণ এবং উচ্চারণযোগ্য ভ্যারিয়েবল নাম ব্যবহার করা উচিৎ

**খারাপ:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**ভালো:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### ভ্যারিয়েবলের টাইপ অনুযায়ী একই ধরণের নাম ব্যবহার করা উচিৎ

**খারাপ:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**ভালো:**

```javascript
getUser();
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### সহজেই খুঁজে পাওয়া যায় এমন নাম ব্যবহার করা উচিৎ

আমরা জীবনে যতটা না কোড লিখব তার চেয়েও বেশি কোড পড়ব। এটি অত্যন্ত গুরুত্বপূর্ণ, যে কোডটি আমরা লিখব সেটা যেন
পড়ার উপযুক্ত এবং অনুসন্ধানযোগ্য হয়। আমাদের প্রোগ্রাম বোঝার জন্য অর্থপূর্ণ ভ্যারিয়েবল নাম ব্যবহার না করে, আমরা আমাদের পাঠকদের যন্ত্রণা এবং কষ্ট দেই। এজন্য ভ্যারিয়েবলের নামগুলি অনুসন্ধানযোগ্য করুন।
[buddy.js](https://github.com/danielstjules/buddy.js) এবং
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
এর মতো টুলসগুলো নাম ছাড়া ভেরিয়েবল সনাক্ত করতে সহায়তা করতে পারে।

**খারাপ:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**ভালো:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### অর্থপ্রকাশক বা ব্যাখ্যামূলক ভ্যারিয়েবল নাম ব্যবহার করা উচিৎ

**খারাপ:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**ভালো:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### আধ্যাত্মিক ম্যাপিং এড়িয়ে চলুন

ঊহ্য এর চেয়ে পরিষ্কারভাবে বর্ণিত বেশী ভালো।

**খারাপ:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**ভালো:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### অপ্রয়োজনীয় কনটেক্সট যুক্ত করবেন না

যদি আপনার ক্লাস/অবজেক্ট এর নামেই কোন কিছু প্রকাশ করে, তাহলে সেই একই জিনিস ভ্যারিয়েবলের নামে ব্যবহার করবেন না।

**খারাপ:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**ভালো:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {re often cleaner than short circuiting. Be aware that if you
use them
  car.color = "Red";
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### শর্ট সার্কুটিং বা কন্ডিশনালের পরিবর্তে ডিফল্ট আর্গুমেন্ট ব্যবহার করুন

ডিফল্ট আর্গুমেন্ট প্রায়শই শর্ট সার্কুটিং এর চেয়ে পরিষ্কার। সচেতন থাকবেন যদি আপনি শর্ট সার্কুটিং ব্যবহার করে থাকেন, আপনার ফাংশনটি কেবল `undefined` আর্গুমেন্টের জন্যই ডিফল্ট ভ্যালু প্রদান করবে। অন্যান্য “ফলসি” ভ্যালু যেমন `''`, `""`, `false`, `null`, `0`, and
`NaN` এর জন্য ডিফল্ট ভ্যালু দ্বারা প্রতিস্থাপিত হবে না।

**খারাপ:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**ভালো:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **ফাংশন**

### ফাংশন আর্গুমেন্ট (২ অথবা তার চেয়ে কম)

ফাংশন প্যারামিটারের সংখ্যা সীমাবদ্ধে রাখা অত্যন্ত গুরুত্বপূর্ণ, কারণ এতে ফাংশনটিকে পরীক্ষা করা অধিকর সহজ হয়ে যায়। তিনটিরও বেশি সংখ্যক প্যারামিটার একত্রিত হলে বিস্ফোরণের দিকে যাবে, যেখানে আপনাকে বিভিন্ন সংখ্যক কেস পরীক্ষা করতে হবে প্রতিটি আর্গুমেন্ট এর জন্য।

এক্ষেত্রে এক বা দুটি আর্গুমেন্ট হল আদর্শ এবং তিনটি যত সম্ভব এড়ানো উচিত, এর চেয়ে বেশি হলে একত্র করা উচিত। সাধারণত, আপনার যদি দুটিরও বেশি আর্গুমেন্ট থাকে তবে আপনার ফাংশনটি অনেক বেশি কাজ করার চেষ্টা করে। যেক্ষেত্রে এমন হয় না, সেক্ষেত্রে প্রায়শই হাই লেভেলের অবজেক্ট আর্গুমেন্ট হিসেবে কাজ করে।

যেহেতু অনেকগুলো ক্লাস বয়লারপ্লেট ছাড়াই জাভাস্ক্রিপ্ট আপনাকে এমনিতেই অবজেক্ট তৈরি করতে দেয়, যদি আপনার অনেক বেশি আর্গুমেন্ট এর দরকার হয়ে থাকে তাহলে আর্গুমেন্ট হিসেবে আপনি একটি অবজেক্ট ব্যবহার করতে পারেন।

ফাংশনটি কী কী বৈশিষ্ট্য প্রত্যাশা করে তা স্পষ্ট করে তুলতে, আপনি ES2015/ES6 ডেস্ট্রাকচারিং সিনট্যাক্স ব্যবহার করতে পারেন। এর কয়েকটি সুবিধা রয়েছে:

1. যখন কেউ ফাংশনটিকে দেখবে, এটি সাথেসাথে কি কি প্রোপার্টি ব্যবহার করা হয়েছে তা বলে দিবে।
2. ডেস্ট্রাকচারিং ফাংশনে প্রেরণ করা আর্গুমেন্টের নির্দিষ্ট প্রিমিটিভ ভ্যালু গুলিও ক্লোন করে। এটি পার্শ্ব প্রতিক্রিয়া রোধ করতে সহায়তা করতে পারে। বি.দ্রঃ অবজেক্ট এবং অ্যারে যেগুলো আর্গুমেন্ট অবজেক্ট হতে ডেস্ট্রাকচার্ড সেগুলো ক্লোন করা হয় না।
3. লিন্টারগুলো অব্যবহৃত প্রোপার্টি সম্পর্কে সতর্ক করতে পারে, যেটা ডেস্ট্রাকচারিং ছাড়া অসম্ভব হবে।

**খারাপ:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**ভালো:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### ফাংশনের একটি জিনিস করা উচিত

এটি এখন পর্যন্ত সফ্টওয়্যার ইঞ্জিনিয়ারিংয়ের সবচেয়ে গুরুত্বপূর্ণ নিয়ম। ফাংশনগুলি যখন একাধিক জিনিস করে তখন এদের কম্পোস এবং টেস্ট করা অনেক কঠিন হয়ে যায়। আপনি যখন কেবল একটি কাজে কোন ফাংশনকে আলাদা করতে পারেন, তখন এগুলি সহজেই রিফ্যাক্টর করা যায় এবং আপনার কোডটি আরও পরিষ্কার পরিচ্ছন্ন থাকবে। আপনি যদি এই ছাড়া অন্য কিছু এই গাইড থেকে না নিয়েও থাকেন, এরপরেও অনেক ডেভলপার হতে আপনি এগিয়ে থাকবেন।

**খারাপ:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**ভালো:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### ফাংশন যা কাজ করে সেটাই নাম হওয়া উচিৎ

**খারাপ:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**ভালো:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### ফাংশনগুলোতে শুধুমাত্র এক স্তরের অ্যাবস্ট্রাকশন থাকা উচিৎ

যখন আপনার একাধিক স্তরের অ্যাবস্ট্রাকশন থাকে তখন আপনার ফাংশনটি সাধারণত খুব বেশি কাজ করে। ফাংশনগুলি বিভক্ত করলে পুনরায় ব্যবহারযোগ্যতা এবং সহজেই টেস্ট করার দিকে ধাবিত করে।

**খারাপ:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**ভালো:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### ডুপ্লিকেট কোড রিমুভ করতে হবে

ডুপ্লিকেট কোড এড়াতে আপনার সর্বোচ্চ চেষ্টা করুন। ডুপ্লিকেট কোড অত্যন্ত খারাপ কারণ কোথাও কোন লজিক পরিবর্তন করতে হলে অনেক জায়গার লজিক/কোড পরিবর্তন করতে হবে।

কল্পনা করুন আপনি কোনও রেস্তোরাঁ চালাচ্ছেন এবং নিজেই বাজারের তালিকা ট্র্যাক করেনঃ আপনার সকল টমেটো, পেঁয়াজ, রসুন, মশলা ইত্যাদি। আপনার যদি এমন একাধিক তালিকা থাকে যা আপনি ট্রাক রাখেন, যখন আপনি টমেটো দিয়ে কোন খাবার পরিবেশন করবেন তখন আপনাকে সকল তালিকা আপডেট করতে হবে। যদি আপনার কেবল একটিই তালিকা থাকে তাহলে কেবলমাত্র একটি জায়গায় আপডেট করলেই হবে।
<!--
Oftentimes you have duplicate code because you have two or more slightly
different things, that share a lot in common, but their differences force you
to have two or more separate functions that do much of the same things. Removing
duplicate code means creating an abstraction that can handle this set of
different things with just one function/module/class.

Getting the abstraction right is critical, that's why you should follow the
SOLID principles laid out in the _Classes_ section. খারাপ abstractions can be
worse than duplicate code, so be careful! Having said this, if you can make
a ভালো abstraction, do it! Don't repeat yourself, otherwise you'll find yourself
updating multiple places anytime you want to change one thing. -->

**খারাপ:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**ভালো:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Set default objects with Object.assign

**খারাপ:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**ভালো:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  config = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );

  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### ফাংশন প্যারামিটার হিসেবে ফ্ল্যাগ ব্যবহার করা উচিৎ নয়

ফ্ল্যাগ ব্যবহারকারীকে নির্দেশনা দেয় যে এই ফাংশন একাধিক কাজ করে। ফাংশনের শুধুমাত্র একটি কাজ করা উচিৎ। আপনার ফাংশনগুলি আলাদা করুন যদি তারা কোন বুলিয়ানের উপর ভিত্তি করে ভিন্ন কোড পাথ অনুসরণ করে।

**খারাপ:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**ভালো:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### পার্শ্ব প্রতিক্রিয়া এড়িয়ে চলুন (প্রথম অংশ)

একটি ফাংশন তখনই কোন পার্শ্ব প্রতিক্রিয়া তৈরি করে যখন এটি একটি ভ্যালু গ্রহণ করে এবং অন্য ভ্যালু বা একের অধিক ভ্যালু রিটার্ন করে। পার্শ্ব প্রতিক্রিয়া যে কোন কিছু হতে পারে। যেমনঃ কোন ফাইলে লেখা, গ্লোবাল ভ্যারিয়েবলের পরিবর্তন অথবা দুর্ঘটনাক্রমে আপনার সমস্ত টাকা কোন অপরিচিত ব্যক্তিকে পাঠিয়ে দেওয়া ইত্যাদি।

এখন, আপনার বিশেষ উপলক্ষে আপনার প্রোগ্রামে পার্শ্ব প্রতিক্রিয়া থাকা দরকার। পূর্ববর্তী উদাহরণের মতো, আপনার হয়ত কোন ফাইল লিখতে হবে। আপনি এখন যা করতে চান তা হল, আপনি যেখানে এটি করছেন সেটা একত্রিত করা। একটি নির্দিষ্ট ফাইলে লেখার জন্য বেশ কয়েকটি ফাংশন এবং ক্লাস নেই। একটি ফাংশন আছে যেটা এই কাজ করে এবং কেবল মাত্র একটিই ফাংশন।

মূল বিষয় হচ্ছে কোনও কাঠামো ছাড়াই অবজেক্টের মধ্যে স্টেট ভাগ করে নেওয়ার মতো সাধারণ সমস্যাগুলি এড়াতে চলা, পরিবর্তনীয় ডেটা টাইপ ব্যবহার করা যা কোন কিছু দ্বারা লেখা যাবে এবং আপনি অন্যান্য সংখ্যাগরিষ্ঠ প্রোগ্রামারদের চেয়ে সুখী থাকবেন।

**খারাপ:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**ভালো:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### পার্শ্ব প্রতিক্রিয়া এড়িয়ে চলুন (দ্বিতীয় অংশ)

জাভাস্ক্রিপ্টে প্রিমিটিভগুলো ভ্যালু এবং অবজেক্ট/অ্যারে গুলো রেফারেন্স দিয়ে পাস হয়। অবজেক্ট এবং অ্যারের ক্ষেত্রে,যদি আপনার ফাংশন কোনও শপিং কার্ট অ্যারে পরিবর্তন করে উদাহরণ্বরূপঃ কেনার জন্য একটি আইটেম এড করলে  `cart` অ্যারে ব্যবহার করা অন্য কোন ফাংশনও এই সংযোজন দ্বারা প্রভাবিত হবে। এটি দুর্দান্ত হতে পারে, তবে এটি খারাপও হতে পারে।

<!-- Let's imagine a খারাপ situation:
The user clicks the "Purchase", button which calls a `purchase` function that
spawns a network request and sends the `cart` array to the server. Because
of a খারাপ network connection, the `purchase` function has to keep retrying the
request. Now, what if in the meantime the user accidentally clicks "Add to Cart"
button on an item they don't actually want before the network request begins?
If that happens and the network request begins, then that purchase function
will send the accidentally added item because it has a reference to a shopping
cart array that the `addItemToCart` function modified by adding an unwanted
item.

A great solution would be for the `addItemToCart` to always clone the `cart`,
edit it, and return the clone. This ensures that no other functions that are
holding onto a reference of the shopping cart will be affected by any changes.

Two caveats to mention to this approach:

1. There might be cases where you actually want to modify the input object,
   but when you adopt this programming practice you will find that those cases
   are pretty rare. Most things can be refactored to have no side effects!

2. Cloning big objects can be very expensive in terms of performance. Luckily,
   this isn't a big issue in practice because there are
   [great libraries](https://facebook.github.io/immutable-js/) that allow
   this kind of programming approach to be fast and not as memory intensive as
   it would be for you to manually clone objects and arrays. -->

**খারাপ:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**ভালো:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### গ্লোবাল ফাংশনে লিখবেন না

জাভাস্ক্রিপ্টে গ্লোবাল কোন কিছু ্ নষ্ট করা বা পরিবর্তন করা অত্যন্ত খারাপ প্রাকটিস কারণ আপনি অন্য একটি লাইব্রেরির সঙ্গে ক্লাস ঘটাতে পারেন এবং আপনার API ব্যবহারকারীরা ততক্ষণ পর্যন্ত টের পাবে না যতক্ষণ না তারা কোন ব্যতিক্রম কিছু লক্ষ্য করবে। একটি উদাহরণ সম্পর্কে চিন্তা করা যাক: ধরে নেন আপনি জাভাস্ক্রিপ্টের একটা নেটিভ অ্যারে মেথডকে এক্সটেন্ড করতে চান যেন সেখানে আরেকটি `diff` নামের মেথড থাকে যেটি দুটি অ্যারের মধ্যে পার্থক্য দেখাতে পারে। আপনি চাইলে `Array.prototype`-এ একটি নতুন ফাংশন লিখতে পারেন কিন্তু এতে অন্য কোন লাইব্রেরির সাথে সংঘর্ষ হতে পারে যারা একই কাজ করে। তখন কি হবে যদি আরেকটি লাইব্রেরি `diff` ব্যবহার করত শুধুমাত্র একটি অ্যারের প্রথম এবং শেষ ইলিমেন্টের মধ্যে পার্থক্য বের করার জন্য? এজন্যই ES2015/ES6 ক্লাস ব্যবহার করা আরও ভাল হবে এবং এটা সিম্পলি গ্লোবাল `Array` এক্সটেন্ড করবে।

**খারাপ:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**ভালো:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### ফাংশনাল প্রোগ্রামিংকে ইমপারেটিভ প্রোগ্রামিং থেকে বেশি গুরুত্ব দিন


JavaScript isn't a functional language in the way that Haskell is, but it has
a functional flavor to it. Functional languages can be cleaner and easier to test.
Favor this style of programming when you can.য়া 

**খারাপ:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**ভালো:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Encapsulate conditionals

**খারাপ:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**ভালো:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Avoid negative conditionals

**খারাপ:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**ভালো:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Avoid conditionals

This seems like an impossible task. Upon first hearing this, most people say,
"how am I supposed to do anything without an `if` statement?" The answer is that
you can use polymorphism to achieve the same task in many cases. The second
question is usually, "well that's great but why would I want to do that?" The
answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you
are telling your user that your function does more than one thing. Remember,
just do one thing.

**খারাপ:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**ভালো:**

```javascript
class Airplane {
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

**[⬆ উপরে যাও](#সুচিপত্র)**

### Avoid type-checking (part 1)

JavaScript is untyped, which means your functions can take any type of argument.
Sometimes you are bitten by this freedom and it becomes tempting to do
type-checking in your functions. There are many ways to avoid having to do this.
The first thing to consider is consistent APIs.

**খারাপ:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**ভালো:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Avoid type-checking (part 2)

If you are working with basic primitive values like strings and integers,
and you can't use polymorphism but you still feel the need to type-check,
you should consider using TypeScript. It is an excellent alternative to normal
JavaScript, as it provides you with static typing on top of standard JavaScript
syntax. The problem with manually type-checking normal JavaScript is that
doing it well requires so much extra verbiage that the faux "type-safety" you get
doesn't make up for the lost readability. Keep your JavaScript clean, write
ভালো tests, and have ভালো code reviews. Otherwise, do all of that but with
TypeScript (which, like I said, is a great alternative!).

**খারাপ:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**ভালো:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Don't over-optimize

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are ভালো
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

**খারাপ:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**ভালো:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Remove dead code

Dead code is just as খারাপ as duplicate code. There's no reason to keep it in
your codebase. If it's not being called, get rid of it! It will still be safe
in your version history if you still need it.

**খারাপ:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**ভালো:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ উপরে যাও](#সুচিপত্র)**
<!--
## **Objects and Data Structures**

### Use getters and setters

Using getters and setters to access data on objects could be better than simply
looking for a property on an object. "Why?" you might ask. Well, here's an
unorganized list of reasons why:

- When you want to do more beyond getting an object property, you don't have
  to look up and change every accessor in your codebase.
- Makes adding validation simple when doing a `set`.
- Encapsulates the internal representation.
- Easy to add logging and error handling when getting and setting.
- You can lazy load your object's properties, let's say getting it from a
  server.

**খারাপ:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**ভালো:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Make objects have private members

This can be accomplished through closures (for ES5 and below).

**খারাপ:**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**ভালো:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **Classes**

### Prefer ES2015/ES6 classes over ES5 plain functions

It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**খারাপ:**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**ভালো:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Use method chaining

This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

**খারাপ:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**ভালো:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Prefer composition over inheritance

As stated famously in [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of
ভালো reasons to use inheritance and lots of ভালো reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for
inheritance, try to think if composition could model your problem better. In some
cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
   relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
   (Change the caloric expenditure of all animals when they move).

**খারাপ:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// খারাপ because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**ভালো:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **SOLID**

### Single Responsibility Principle (SRP)

As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify
a piece of it, it can be difficult to understand how that will affect other
dependent modules in your codebase.

**খারাপ:**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**ভালো:**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

**খারাপ:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**ভালো:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

**খারাপ:**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // খারাপ: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**ভালো:**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Interface Segregation Principle (ISP)

JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

A ভালো example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".

**খারাপ:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**ভালো:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should
   depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
   abstractions.

This can be hard to understand at first, but if you've worked with AngularJS,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very খারাপ development pattern because
it makes your code hard to refactor.

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

**খারাপ:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // খারাপ: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**ভালো:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **Testing**

Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[ভালো coverage tool](https://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There are [plenty of ভালো JS test frameworks](https://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**খারাপ:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**ভালো:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **Concurrency**

### Use Promises, not callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

**খারাপ:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**ভালো:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Async/Await are even cleaner than Promises

Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

**খারাপ:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**ভালো:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin");
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle();
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **Error Handling**

Thrown errors are a ভালো thing! They mean the runtime has successfully
identified when something in your program has gone wrong and it's letting
you know by stopping function execution on the current stack, killing the
process (in Node), and notifying you in the console with a stack trace.

### Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

**খারাপ:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**ভালো:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### Don't ignore rejected promises

For the same reason you shouldn't ignore caught errors
from `try/catch`.

**খারাপ:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**ভালো:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **Formatting**

Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](https://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization

JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**খারাপ:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**ভালো:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**খারাপ:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**ভালো:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## **Comments**

### Only comment things that have business logic complexity.

Comments are an apology, not a requirement. ভালো code _mostly_ documents itself.

**খারাপ:**

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**ভালো:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**খারাপ:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**ভালো:**

```javascript
doStuff();
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**খারাপ:**

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**ভালো:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ উপরে যাও](#সুচিপত্র)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**খারাপ:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**ভালো:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ উপরে যাও](#সুচিপত্র)**

## Translation

This is also available in other languages:

- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**:
  [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**:
  [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**:
  [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)

**[⬆ উপরে যাও](#সুচিপত্র)** -->

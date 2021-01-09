# 1. Строгие проверки на равенство

Стремитесь к тому, чтобы вместо  `==`  использовать  `===`.

*// Если необдуманно использовать оператор == - это может серьёзно повлиять на программную логику. Это можно сравнить с тем, что некто ожидает, что пойдёт налево, но по какой-то причине вдруг идёт направо.*
0 == false // true
0 === false // false
2 == "2" // true
2 === "2" // false

// пример
const value = "500";
if (value === 500) {
  console.log(value);
  // этот код не выполнится
}

if (value === "500") {
  console.log(value);
  // этот код выполнится
}

2. Переменные

Называйте переменные так, чтобы их имена раскрывали бы их сущность, их роль в программе. При таком подходе их удобно будет искать в коде, а тот, кто увидит этот код, легче сможет понять смысл выполняемых им действий.
Плохо:

let daysSLV = 10;
let y = new Date().getFullYear();

let ok;
if (user.age > 30) {
  ok = true;
}

Хорошо:

const MAX_AGE = 30;
let daysSinceLastVisit = 10;
let currentYear = new Date().getFullYear();

...

const isUserOlderThanAllowed = user.age > MAX_AGE;

Не надо добавлять в имена переменных дополнительные слова, в которых нет необходимости.

Плохо:

let nameValue;
let theProduct;

Хорошо:

let name;
let product;

Не стоит принуждать того, кто читает код, к тому, чтобы ему приходилось бы помнить о том, в каком окружении объявлена переменная.

Плохо:

const users = ["John", "Marco", "Peter"];
users.forEach(u => {
  doSomething();
  doSomethingElse();
  // ...
  // ...
  // ...
  // ...
  // Тут перед нами ситуация, в которой возникает WTF-вопрос "Для чего используется `u`?"
  register(u);
});

Хорошо:

const users = ["John", "Marco", "Peter"];
users.forEach(user => {
  doSomething();
  doSomethingElse();
  // ...
  // ...
  // ...
  // ...
  register(user);
});

Не нужно снабжать имена переменных избыточными сведениями о контексте, в котором они используются.

Плохо:

const user = {
  userName: "John",
  userSurname: "Doe",
  userAge: "28"
};

...

user.userName;

Хорошо:

const user = {
  name: "John",
  surname: "Doe",
  age: "28"
};

...

user.name;

3. Функции

Используйте для функций длинные описательные имена. Учитывая то, что функция представляет собой описание выполнения некоего действия, её имя должно представлять собой глагол или фразу, полностью описывающую суть функции. Имена аргументов нужно подбирать так, чтобы они адекватно описывали бы представляемые ими данные. Имена функций должны сообщать читателю кода о том, что именно делают эти функции.

Плохо:

function notif(user) {
  // реализация
}

Хорошо:

function notifyUser(emailAddress) {
  // реализация
}

Избегайте использования длинных списков аргументов. В идеале функции следует иметь два или меньшее количество аргументов. Чем меньше у функции аргументов — тем легче будет её тестировать.

Плохо:

function getUsers(fields, fromDate, toDate) {
  // реализация
}

Хорошо:

function getUsers({ fields, fromDate, toDate }) {
  // реализация
}

getUsers({
  fields: ['name', 'surname', 'email'],
  fromDate: '2019-01-01',
  toDate: '2019-01-18'
})

Используйте аргументы по умолчанию, отдавая им предпочтение перед условными конструкциями.

Плохо:

function createShape(type) {
  const shapeType = type || "cube";
  // ...
}

Хорошо:

function createShape(type = "cube") {
  // ...
}

Функция должна решать одну задачу. Стремитесь к тому, чтобы одна функция не выполняла бы множество действий.

Плохо:

function notifyUsers(users) {
  users.forEach(user => {
    const userRecord = database.lookup(user);
    if (userRecord.isVerified()) {
      notify(user);
    }
  });
}

Хорошо:

function notifyVerifiedUsers(users) {
  users.filter(isUserVerified).forEach(notify);
}

function isUserVerified(user) {
  const userRecord = database.lookup(user);
  return userRecord.isVerified();
}

Используйте Object.assign для установки свойств объектов по умолчанию.

Плохо:

const shapeConfig = {
  type: "cube",
  width: 200,
  height: null
};

function createShape(config) {
  config.type = config.type || "cube";
  config.width = config.width || 250;
  config.height = config. height || 250;
}

createShape(shapeConfig);

Хорошо:

const shapeConfig = {
  type: "cube",
  width: 200
  // Значение ключа 'height' не задано
};

function createShape(config) {
  config = Object.assign(
    {
      type: "cube",
      width: 250,
      height: 250
    },
    config
  );

  ...
}

createShape(shapeConfig);

Не используйте флаги в качестве параметров. Их использование означает, что функция выполняет больше действий, чем ей следовало бы выполнять.

Плохо:

function createFile(name, isPublic) {
  if (isPublic) {
    fs.create(`./public/${name}`);
  } else {
    fs.create(name);
  }
}

Хорошо:

function createFile(name) {
  fs.create(name);
}

function createPublicFile(name) {
  createFile(`./public/${name}`);
}

Не загрязняйте глобальную область видимости. Если вам нужно расширить существующий объект — используйте ES-классы и механизмы наследования вместо того, чтобы создавать функции в цепочке прототипов стандартных объектов.

Плохо:

Array.prototype.myFunc = function myFunc() {
  // реализация
};

Хорошо:

class SuperArray extends Array {
  myFunc() {
    // реализация
  }
}

4. Условные конструкции

Старайтесь не называть логические переменные так, чтобы в их именах присутствовало бы отрицание. То же самое касается и функций, возвращающих логические значения. Использование таких сущностей в условных конструкциях затрудняет чтение кода.

Плохо:

function isUserNotBlocked(user) {
  // реализация
}

if (!isUserNotBlocked(user)) {
  // реализация
}

Хорошо:

function isUserBlocked(user) {
  // реализация
}

if (isUserBlocked(user)) {
  // реализация
}

Пользуйтесь сокращённой формой записи условных конструкций. Возможно, эта рекомендация может показаться тривиальной, но об этом стоит упомянуть. Используйте этот подход только для логических переменных, и в том случае, если вы уверены в том, что значением переменной не будет undefined или null.

Плохо:

if (isValid === true) {
  // что-то сделать...
}

if (isValid === false) {
  // что-то сделать...
}

Хорошо:

if (isValid) {
  // что-то сделать...
}

if (!isValid) {
  // что-то сделать...
}

Избегайте логических конструкций везде, где это возможно. Вместо них используйте полиморфизм и наследование.

Плохо:

class Car {
  // ...
  getMaximumSpeed() {
    switch (this.type) {
      case "Ford":
        return this.someFactor() + this.anotherFactor();
      case "Mazda":
        return this.someFactor();
      case "McLaren":
        return this.someFactor() - this.anotherFactor();
    }
  }
}

Хорошо:

class Car {
  // ...
}

class Ford extends Car {
  // ...
  getMaximumSpeed() {
    return this.someFactor() + this.anotherFactor();
  }
}

class Mazda extends Car {
  // ...
  getMaximumSpeed() {
    return this.someFactor();
  }
}

class McLaren extends Car {
  // ...
  getMaximumSpeed() {
    return this.someFactor() - this.anotherFactor();
  }
}

5. ES-классы

Классы появились в JavaScript сравнительно недавно. Их можно назвать «синтаксическим сахаром». В основе того, что получается при использовании классов, лежат, как и прежде, прототипы объектов. Но код, в котором применяются классы, выглядит иначе. В целом, если есть такая возможность, ES-классы стоит предпочесть обычным функциям-конструкторам.

Плохо:

const Person = function(name) {
  if (!(this instanceof Person)) {
    throw new Error("Instantiate Person with `new` keyword");
  }

  this.name = name;
};

Person.prototype.sayHello = function sayHello() { /**/ };

const Student = function(name, school) {
  if (!(this instanceof Student)) {
    throw new Error("Instantiate Student with `new` keyword");
  }

  Person.call(this, name);
  this.school = school;
};

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;
Student.prototype.printSchoolName = function printSchoolName() { /**/ };

Хорошо:

class Person {
  constructor(name) {
    this.name = name;
  }

  sayHello() {
    /* ... */
  }
}

class Student extends Person {
  constructor(name, school) {
    super(name);
    this.school = school;
  }

  printSchoolName() {
    /* ... */
  }
}

Организуйте методы так, чтобы их можно было бы объединять в цепочки. Этот паттерн используют многие библиотеки — такие, как jQuery и Lodash. В результате ваш код будет более компактным, чем без использования этого паттерна. Речь идёт о том, что в конце каждой из функций класса нужно возвращать this. Это позволит объединять вызовы таких функций в цепочки.

Плохо:

class Person {
  constructor(name) {
    this.name = name;
  }

  setSurname(surname) {
    this.surname = surname;
  }

  setAge(age) {
    this.age = age;
  }

  save() {
    console.log(this.name, this.surname, this.age);
  }
}

const person = new Person("John");
person.setSurname("Doe");
person.setAge(29);
person.save();

Хорошо:

class Person {
  constructor(name) {
    this.name = name;
  }

  setSurname(surname) {
    this.surname = surname;
    // Возвратим this для получения возможности объединять вызовы методов в цепочки
    return this;
  }

  setAge(age) {
    this.age = age;
    // Возвратим this для получения возможности объединять вызовы методов в цепочки
    return this;
  }

  save() {
    console.log(this.name, this.surname, this.age);
    // Возвратим this для получения возможности объединять вызовы методов в цепочки
    return this;
  }
}

const person = new Person("John")
    .setSurname("Doe")
    .setAge(29)
    .save();
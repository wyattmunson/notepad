---
title: "Js Cheatsheet"
description: ""
lead: ""
summary: "JavaScript cheatsheet."
date: 2023-01-04T03:05:57-08:00
lastmod: 2023-01-04T03:05:57-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "js-cheatsheet-d435d3138ff132a2ecc583aa74b8d9a5"
weight: 999
toc: true
---

## Creating Characther Map

```js
// List of everything

/**
 * CHARACHTER MAP
 *
 * Creating a char map
 */

const createCharMap = (string) => {
  const map = {};

  for (let item of string) {
    if (map[item]) {
      map[item]++;
    } else {
      map[item] = 1;
    }
  }

  return map;
};

// fast char map
const createCharMap = (string) => {
  const map = {};

  for (let item of string) {
    map[item] = map[item] + 1 || 1;
  }

  return map;
};

// or another way
```

## Array Helpers

```js
/**
 * LENGTH OF OBJECT
 */

const someObject = {};
Object.keys(someObject).length;

/**
 * ARRAY HELPERS
 */

const students = [
  { teacher: "Johnson", course: "English", grade: 98, credits: 3 },
  { teacher: "Burns", course: "History", grade: 92, credits: 3 },
  { teacher: "Burns", course: "Geography", grade: 84, credits: 3 },
  { teacher: "Thatcher", course: "Southern Etiquitte", grade: 98, credits: 3 },
];

/**
 * ARRAY HELPERS: FIND
 *
 * Find returns the first match
 */

const findMatch = students.find((item) => {
  return item.teacher === "Johnson";
});

// return: { teacher: "Johnson", course: "English", grade: 98, credits: 3 }

// FIND: return teacher only
const findCourseByTeacher = () => {
  const match = students.find((x) => {
    return x.teacher === "Johnson";
  });
  return match.course;
};
// return: "English"

/**
 * ARRAY HELPERS: FILTER
 *
 * Finds all matches
 */

const filter = students.filter((item) => {
  return item.teacher === "Burns";
});

// output:
//      returns [
//        { teacher: "Burns", course: "History", grade: 92, credits: 3 },
//        { teacher: "Burns", course: "Geography", grade: 84, credits: 3 }
//      ]

// REDUCE: Get multiple matching values
const getCourseArray = (students, teacher) => {
    const courses = students.filter((total, item) => {
        return item.
    })
}


/**
 * ARRAY HELPERS: REDUCE
 *
 * Explain this better
 */

// Sum values in array of object
// Remember second argument of "0" in this case
const sum = students.reduce((total, item) => {
  return total + item.credits;
}, 0);

// WEIGHTED AVERAGE
// Average grades based on credit weight
const weightedAverage =
  students.reduce((total, item) => {
    return total + item.credits * item.grade;
  }, 0) / sum;

/**
 * DATA STRUCTURES: QUEUE
 */

class Queue {
    constructor() {
        this.data = []
    }

    add(i) {
        this.data.unshift(i);
    }

    remove() {
        return this.data.pop();
    }
}

// instantiate the class with
const q = new Queue();
q.add(1)
q.remove();
```

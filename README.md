## ১. ❓ Primary Key এবং Foreign Key ব্যাখ্যা কর

### 🟢 Primary Key:
- ইউনিক এবং NULL নয় এমন একটি কলাম বা কলামের সেট।
- একটি টেবিলে কেবল একটি Primary Key থাকতে পারে।
- ডেটা দ্রুত খুঁজে পেতে PostgreSQL ইন্ডেক্স তৈরি করে।

#### ✅ নিয়মসমূহ:
- NULL ভ্যালু থাকতে পারবে না।
- রিপিটেড ভ্যালু থাকতে পারবে না।
- একাধিক কলাম নিয়ে গঠিত হতে পারে।

#### 👉 উদাহরণ:
```sql
-- এক কলাম
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

-- একাধিক কলাম
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id)
);
```
### 🟢 Foreign Key:
  -অন্য টেবিলের Primary Key কে রেফার করে।

  -টেবিলগুলোর মধ্যে লিংক তৈরি করে।

#### ✅ নিয়মসমূহ:
- অবশ্যই অন্য টেবিলের Primary বা Unique Key রেফার করতে হবে।

- ডেটা টাইপ মেলাতে হবে।

- NULL থাকতে পারে।

- একাধিক Foreign Key থাকতে পারে।

- ON DELETE / ON UPDATE ক্লজ ব্যবহার করতে হয়।

#### 👉 উদাহরণ:
```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    address TEXT
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id) ON DELETE CASCADE
);
```
## ২. ❓ VARCHAR এবং CHAR এর মধ্যে পার্থক্য
###🔹 CHAR(n) – Fixed Length:
- সবসময় নির্দিষ্ট সংখ্যক ক্যারেক্টার স্টোর করে।
- ছোট স্ট্রিং হলে ফাঁকা জায়গা দিয়ে পূরণ করে।

```sql
user_name CHAR(10);
-- 'Ali' হবে 'Ali'
```
### 🔹 VARCHAR(n) – Variable Length:
- যতটুকু দরকার, ততটুকু জায়গা নেয়।
- মেমোরি সাশ্রয়ী এবং ফ্লেক্সিবল।

```sql
user_name VARCHAR(10);
-- 'Ali' হবে 'Ali'
```
বৈশিষ্ট্য 	CHAR(n)	VARCHAR(n)
দৈর্ঘ্য	নির্দিষ্ট	পরিবর্তনশীল
স্পেস	ফাঁকা জায়গা পূরণ	কিছুই যোগ হয় না
মেমোরি	বেশি লাগে	কম লাগে
পারফরম্যান্স	Marginally faster	সাধারণত ব্যবহৃত হয়

## ৩. ❓ LIMIT এবং OFFSET ক্লজের ব্যবহার
### 🔹 LIMIT:
- নির্ধারিত সংখ্যক রেকর্ড ফেরত দেয়।

```sql
SELECT name, salary
FROM employee
ORDER BY salary
LIMIT 5;
```
### 🔹 OFFSET:
- নির্দিষ্ট সংখ্যক রেকর্ড স্কিপ করে।

```sql
SELECT * FROM students
OFFSET 5;
```
####🔸 একসাথে ব্যবহার:
- Pagination এ অনেক বেশি কাজে লাগে।

```sql
SELECT * FROM students
LIMIT 5 OFFSET page_number * 5;
```
## ৪. ❓ JOIN Operation এর গুরুত্ব
### 🔹 JOIN কী?
- দুই বা ততোধিক টেবিলকে কমোন কলামের মাধ্যমে যুক্ত করে।

#### 🔶 JOIN কেন দরকার?
- একাধিক টেবিল থেকে সম্পর্কিত ডেটা আনতে।

#### ✅ উদাহরণ:
```sql
SELECT s.name, m.score
FROM students s
JOIN marks m ON s.student_id = m.student_id
WHERE m.subject = 'Math';
```
### 📂 JOIN প্রকারভেদ:
JOIN টাইপ	কাজ
INNER JOIN	মিল থাকলেই দেখায়
LEFT JOIN	বাম টেবিল সব + মেলে এমন ডান টেবিল
RIGHT JOIN	ডান টেবিল সব + মেলে এমন বাম টেবিল
FULL OUTER JOIN	দুই টেবিল সব
CROSS JOIN	সব কম্বিনেশন
SELF JOIN	নিজের সাথে JOIN

## ৫. ❓ GROUP BY clause ও Aggregation
### 🧠 Aggregation:
- ডেটা সারাংশ তৈরির জন্য ব্যবহৃত যেমন:

COUNT(), SUM(), AVG(), MAX(), MIN()

### 🧠 GROUP BY:
নির্দিষ্ট কলামের ভিত্তিতে ডেটাকে গ্রুপ করে।

#### ✅ উদাহরণ ১: প্রতি subject-এ কয়জন পরীক্ষা দিয়েছে
```sql
SELECT subject, COUNT(*) AS total_students
FROM marks
GROUP BY subject;
```
#### ✅ উদাহরণ ২: প্রতি subject-এর গড় নম্বর
```sql
SELECT subject, AVG(score) AS avg_score
FROM marks
GROUP BY subject;
```
#### ⚠️ নিয়ম:
-SELECT-এ থাকা কলামগুলো অবশ্যই GROUP BY বা Aggregation ফাংশনে থাকতে হবে।
-WHERE আগে, HAVING পরে ব্যবহার হয়।

# พื้นฐาน regular expression ใน python
(KMUTNB 2023-03 :: NLP in python)

- เนื้อหาอยู่ใน [Colab Notebook](https://colab.research.google.com/github/benzerer/python-regular-expression-guide/blob/main/regex.ipynb)

## ตารางตัวกำกับ (special characters) ของ regex ใน python

(เฉพาะที่ใช้บ่อย)

| ประเภท | ตัวกำกับ | ความหมาย |
| - | - | - |
| Character classes | [ABC] | จับตัวอักษรใดตัวอักษรหนึ่งในกลุ่มตัวอักษรนี้ |
| Character classes | [^abc] | จับตัวอักษรใดใดก็ตามที่ไม่อยู่ในกลุ่มตัวอักษรนี้ |
| Character classes | [A-Z] | จับตัวอักษร A-Z เรียงตาม [unicode](https://jrgraphix.net/r/Unicode/0020-007F) |
| Character classes | \s | ตัวอักษร white space (การเว้นวรรคด้วย space bar) |
| Character classes | \S | ตักอักษรที่ไม่ใช่ white space |
| Character classes | \w | เทียบเท่ากับ [A-Za-z0-9] |
| Character classes | \W | เทียบเท่ากับ [^A-Za-z0-9] |
| Character classes | \d | เทียบเท่ากับ [0-9] |
| Character classes | \D | เทียบเท่ากับ [^0-9] |
| Character classes | . | แทนตัวอักษรใดก็ได้ (wild card character) ยกเว้น `\n`|
| Anchor | ^ | ขึ้นต้นบรรทัด |
| Anchor | \$ | สิ้นสุดบรรทัด |
| Anchor | \b | boundary ระหว่าง word และ non-word character (A-Za-z0-9ก-ฮ) |
| Escaped characters | \\+, \\-, \\. | ไม่อยากให้ตัวอักษรพิเศษถูกใช้งานเป็นตัวกำกับ เช่น - + ^ $ . |
| Escaped characters | \uFFFF | match ตัวอักษรด้วย unicode, FFFF แทนที่ด้วย [unicode](https://jrgraphix.net/r/Unicode/0020-007F) |
| Escaped characters | \t | tab |
| Escaped characters | \n | new line |
| Escaped characters | \f | form feed |
| Escaped characters | \r | carriage return |
| Escaped characters | \0 | null character |
| Groups and references | (ABC) | จัดตัวอักษรด้านในวงเล็บให้เป็นกลุ่มเดียวกัน (มีผลเวลา match แล้วดึงออกมา) |
| Groups and references | \1, \2 | อ้างอิงถึงกลุ่มที่เคยถูกจัดไว้ด้านหน้า แต่ไม่ได้ตั้งชื่อ เรียงตามลำดับ |
| Groups and references | (?P\<name\>) | จัดตัวอักษรด้านในวงเล็บให้เป็นกลุ่มเดียวกัน และตั้งชื่อให้ (มีผลเวลา match แล้วดึงออกมา) |
| Groups and references | (?P=name) | อ้างอิงถึงกลุ่มที่เคยถูกจัดไว้ด้านหน้าโดยมีการตั้งชื่อไว้ |
| Groups and references | (?(id/name)yes-pattern\|no-pattern) | อ้างอิงถึงกลุ่มที่เคยถูกจัดไว้ด้านหน้าไม่ว่าจะด้วยลำกับหรือชื่อ yes-pattern จะทำงาน หากกลุ่มตัวอักษรด้านหน้าถูกจับไว้ได้ no-pattern จะทำงานเมื่อไม่มีกลุ่มตัวอักษรถูกจับไว้ โดยที่ no-pattern เป็น optional |
| Groups and references | (?:ABC) | จัดกลุ่มตัวอักษรไว้ แต่ไม่ได้ต้องการ match ออกมาเป็นผลลัพธ์ |
| Quantifiers and alternations | \| | เงื่อนไข A\|B หมายถึง จับ A หรือ B |
| Quantifiers and alternations | + | `BA+CK` หมายถึงจับ A ตั้งแต่ 1 ตัวขึ้นไป เช่น `BACK` `BAACK` BAAACK |
| Quantifiers and alternations | \* | `BA*CK` หมายถึงจับ A ตั้งแต่ 0 ตัวขึ้นไป เช่น `BCK` `BACK` `BAACK` |
| Quantifiers and alternations | ? | `BA?CK` หมายถึงมี A หรือไม่มี A 1 ตัวนั้น ก็ได้ เช่น `BCK` `BACK` แต่ไม่ใช่ `BAACK` |
| Quantifiers and alternations | +?, \*? | Lazy quantifier จับน้อยที่สุดเท่าที่จะจับได้ เช่น `BACK BLACK BECK` ใช้ `B.+CK` จะจับ `BACK BLACK BECK` มาทั้ง 3 คำ แต่ `B.+?CK` จะจับได้แค่ `BACK` |
| Quantifiers and alternations | {m} | `BA{3}CK` จับ A 3 ตัว `BAAACK` |
| Quantifiers and alternations | {m,n} | `BA{1,3}CK` จับ A 1-3 ตัว `BACK`, `BAACK`, `BAAACK` หากเลขข้างหน้าไว้ เช่น `BA{,7}CK` จะจับ A ไม่เกิน 7 ตัว หากเว้นเลขข้างหลังไว้ เช่น `BA{4,}CK` จะจับตั้งแต่ 4 ตัวขึ้นไป |
| Quantifiers and alternations | {m,n}? | Lazy quantifier จับน้อยที่สุดเท่าที่จะจับได้ |
| Lookaround | (?=ABC) | positive lookahead `clean(?=ing)` จะจับ `clean` ที่อยู่ในคำว่า `cleaning` เท่านั้น |
| Lookaround | (?!ABC) | negative lookahead `clean(?!ing)` จะจับ `clean` ที่ไม่มี ing ต่อท้ายเท่านั้น |
| Lookaround | (?<=ABC) | positive lookbehind `(?<=pre)process` จะจับ `process` ที่อยู่ในคำว่า `preprocess` เท่านั้น |
| Lookaround | (?<!...) | negative lookbehind `(?<!pre)process` จะจับ `process` ที่ไม่มี pre นำหน้าเท่านั้น |

## ตาราง regex flag ที่ใช้ใน python

(เฉพาะที่ใช้บ่อย)

| Flag | ความหมาย |
| - | - |
| re.I, re.IGNORECASE | มอง A-Z กับ a-z เป็นตัวอักษรชุดเดียวกัน |
| re.M, re.MULTILINE | มองข้ามการขึ้นบรรทัดใหม่ทุกรูปแบบ |
| re.S, re.DOTALL | `.` มอง `\n` เป็นตัวอักษรตัวหนึ่ง |

## Unicode ที่ใช้บ่อย

- [ภาษาอังกฤษและเลขอารบิก](https://jrgraphix.net/r/Unicode/0020-007F)
- [ภาษาไทย](https://jrgraphix.net/r/Unicode/0E00-0E7F)
- [สกุลเงิน](https://jrgraphix.net/r/Unicode/20A0-20CF)
- [สัญลักษณ์/Emoji 1](https://jrgraphix.net/r/Unicode/2300-23FF)
- [สัญลักษณ์/Emoji 2](https://jrgraphix.net/r/Unicode/2600-26FF)
- [สัญลักษณ์/Emoji 3](https://jrgraphix.net/r/Unicode/2700-27BF)

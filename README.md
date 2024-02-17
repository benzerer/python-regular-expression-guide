# พื้นฐาน regular expression ใน python

KMUTNB 2023-03 :: NLP in python
- [Colab Notebook - Python](https://colab.research.google.com/github/benzerer/python-regular-expression-guide/blob/main/regex.ipynb)

KMUTNB 2024-02 :: NLP in python `new`
- [Pattern Workshop](https://kmutnb-regex-workshop.streamlit.app/)

## ตารางตัวกำกับ (special characters) ของ regex ใน python

(เฉพาะที่ใช้บ่อย)

### Character classes
| ตัวกำกับ | ความหมาย |
| - | - |
| A | จับตัวอักษร A |
| ก | จับตัวอักษร ก |
| [ABC] | จับตัวอักษรใดตัวอักษรหนึ่งในกลุ่มตัวอักษรนี้ |
| [^abc] | จับตัวอักษรใดใดก็ตามที่ไม่อยู่ในกลุ่มตัวอักษรนี้ |
| [A-Z] | จับตัวอักษร A-Z เรียงตาม [unicode](https://jrgraphix.net/r/Unicode/0020-007F) |
| \s | ตัวอักษร white space (การเว้นวรรคด้วย space bar) |
| \S | ตักอักษรที่ไม่ใช่ white space |
| \w | เทียบเท่ากับ [A-Za-z0-9] |
| \W | เทียบเท่ากับ [^A-Za-z0-9] |
| \d | เทียบเท่ากับ [0-9] |
| \D | เทียบเท่ากับ [^0-9] |
| . | แทนตัวอักษรใดก็ได้ (wild card character) ยกเว้น `\n`|

### Anchor
| ตัวกำกับ | ความหมาย |
| - | - |
| ^ | ขึ้นต้นบรรทัด |
| \$ | สิ้นสุดบรรทัด |
| \b | boundary ระหว่าง word และ non-word character (A-Za-z0-9ก-ฮ_) |

### Escaped characters
| ตัวกำกับ | ความหมาย |
| - | - |
| \\+, \\-, \\. | ไม่อยากให้ตัวอักษรพิเศษถูกใช้งานเป็นตัวกำกับ เช่น - + ^ $ . |
| \uFFFF | match ตัวอักษรด้วย unicode, FFFF แทนที่ด้วย [unicode](https://jrgraphix.net/r/Unicode/0020-007F) |
| \t | tab |
| \n | new line |
| \f | form feed |
| \r | carriage return |

### Groups and references
| ตัวกำกับ | ความหมาย |
| - | - |
| (ABC) | จัดตัวอักษรด้านในวงเล็บให้เป็นกลุ่มเดียวกัน (มีผลเวลา match แล้วดึงออกมา) |
| \1, \2 | อ้างอิงถึงกลุ่มที่เคยถูกจัดไว้ด้านหน้า แต่ไม่ได้ตั้งชื่อ เรียงตามลำดับ |
| (?P\<name\>) | จัดตัวอักษรด้านในวงเล็บให้เป็นกลุ่มเดียวกัน และตั้งชื่อให้ (มีผลเวลา match แล้วดึงออกมา) |
| (?P=name) | อ้างอิงถึงกลุ่มที่เคยถูกจัดไว้ด้านหน้าโดยมีการตั้งชื่อไว้ |
| (?(id/name)yes-pattern\|no-pattern) | อ้างอิงถึงกลุ่มที่เคยถูกจัดไว้ด้านหน้าไม่ว่าจะด้วยลำกับหรือชื่อ yes-pattern จะทำงาน หากกลุ่มตัวอักษรด้านหน้าถูกจับไว้ได้ no-pattern จะทำงานเมื่อไม่มีกลุ่มตัวอักษรถูกจับไว้ โดยที่ no-pattern เป็น optional |
| (?:ABC) | จัดกลุ่มตัวอักษรไว้ แต่ไม่ได้ต้องการ match ออกมาเป็นผลลัพธ์ |

### Quantifiers and alternations
| ตัวกำกับ | ความหมาย |
| - | - |
| \| | เงื่อนไข A\|B หมายถึง จับ A หรือ B |
| + | `BA+CK` หมายถึงจับ A ตั้งแต่ 1 ตัวขึ้นไป เช่น `BACK` `BAACK` BAAACK |
| \* | `BA*CK` หมายถึงจับ A ตั้งแต่ 0 ตัวขึ้นไป เช่น `BCK` `BACK` `BAACK` |
| ? | `BA?CK` หมายถึงมี A หรือไม่มี A 1 ตัวนั้น ก็ได้ เช่น `BCK` `BACK` แต่ไม่ใช่ `BAACK` |
| +?, \*? | Lazy quantifier จับน้อยที่สุดเท่าที่จะจับได้ เช่น `BACK BLACK BECK` ใช้ `B.+CK` จะจับ `BACK BLACK BECK` มาทั้ง 3 คำ แต่ `B.+?CK` จะจับได้แค่ `BACK` |
| {m} | `BA{3}CK` จับ A 3 ตัว `BAAACK` |
| {m,n} | `BA{1,3}CK` จับ A 1-3 ตัว `BACK`, `BAACK`, `BAAACK` หากเลขข้างหน้าไว้ เช่น `BA{,7}CK` จะจับ A ไม่เกิน 7 ตัว หากเว้นเลขข้างหลังไว้ เช่น `BA{4,}CK` จะจับตั้งแต่ 4 ตัวขึ้นไป |
| {m,n}? | Lazy quantifier จับน้อยที่สุดเท่าที่จะจับได้ |

### Lookaround
| ตัวกำกับ | ความหมาย |
| - | - |
| (?=ABC) | positive lookahead `clean(?=ing)` จะจับ `clean` ที่อยู่ในคำว่า `cleaning` เท่านั้น |
| (?!ABC) | negative lookahead `clean(?!ing)` จะจับ `clean` ที่ไม่มี ing ต่อท้ายเท่านั้น |
| (?<=ABC) | positive lookbehind `(?<=pre)process` จะจับ `process` ที่อยู่ในคำว่า `preprocess` เท่านั้น |
| (?<!ABC) | negative lookbehind `(?<!pre)process` จะจับ `process` ที่ไม่มี pre นำหน้าเท่านั้น |

## ตาราง regex flag ที่ใช้ใน python

(เฉพาะที่ใช้บ่อย)

| Flag | ความหมาย |
| - | - |
| re.I, re.IGNORECASE | มอง A-Z กับ a-z เป็นตัวอักษรชุดเดียวกัน |
| re.M, re.MULTILINE | เปลี่ยนพฤติกรรม ^ และ $ ในการมองข้อความหลายบรรทัด |
| re.S, re.DOTALL | `.` มอง `\n` เป็นตัวอักษรตัวหนึ่ง |

## Unicode ที่ใช้บ่อย

- [ภาษาอังกฤษและเลขอารบิก](https://jrgraphix.net/r/Unicode/0020-007F)
- [ภาษาไทย](https://jrgraphix.net/r/Unicode/0E00-0E7F)
- [สกุลเงิน](https://jrgraphix.net/r/Unicode/20A0-20CF)
- [สัญลักษณ์/Emoji 1](https://jrgraphix.net/r/Unicode/2300-23FF)
- [สัญลักษณ์/Emoji 2](https://jrgraphix.net/r/Unicode/2600-26FF)
- [สัญลักษณ์/Emoji 3](https://jrgraphix.net/r/Unicode/2700-27BF)

## Reference

- [Python official documentation - v3.8](https://docs.python.org/3.8/library/re.html#regular-expression-syntax)

## แถม
### Low code EDA
- `super easy` [YData profiling FKA. pandas profiling](https://docs.profiling.ydata.ai/latest/)
- `super easy` [D-Tale](https://github.com/man-group/dtale)
### Free Open Source Data Labeling tool
- `super easy` [Label Studio](https://labelstud.io/)
### Free Open Source CDP
- `advanced` [Tracardi](https://tracardi.com/) (javascript frameworks knowledge required)
### Interactive Web UI with Python
- `super easy` [Streamlit](https://streamlit.io/)
- `advanced` [Dash by plotly](https://dash.plotly.com/)
### Speed up your Pandas
- `super easy` [Pandarallel](https://github.com/nalepae/pandarallel)
### Optimize data storage
- `super easy` [Parquet](https://pandas.pydata.org/docs/reference/io.html#parquet)
### Speed up and optimize your python
- `advanced` [Cpython](https://github.com/python/cpython)
- `common` [Numpy](https://numpy.org/)
- `common` [Scipy](https://scipy.org/)
### ASGI
- `advanced` [Sanic](https://sanic.dev/en/)
### Package management
- `super easy` [Pipenv](https://pipenv.pypa.io/en/latest/), [intro to pipenv](https://www.nopdan.ai/post/python/use-pipenv/)
### Time series and forecasting
- `super easy` [Prophet](https://facebook.github.io/prophet/)
### Fastest json seralize/deserializer
- `super easy` [orjson](https://github.com/ijl/orjson)
### Json schema validator
- `advanced` [fast json schema](https://horejsek.github.io/python-fastjsonschema/)
### Structured logging
- `advanced` [struclog](https://www.structlog.org/en/stable/)
### Pre-commit hooks
- `advanced` [pre-commit](https://pre-commit.com/)

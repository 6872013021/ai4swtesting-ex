# ai4swtesting-ex

🌐 [English](README.md) | **ภาษาไทย**

แบบฝึกหัดลงมือปฏิบัติสำหรับการบรรยายเรื่อง **AI for Software Testing**
(วิชา 2110724 Software Quality Assurance and Testing)

กิจกรรมทั้งหมดอ้างอิงจากหนังสือ *Software Testing with Generative AI* (Mark Winteringham, Manning)
เราจะทำกิจกรรมในหนังสือไปทีละข้อ

## ก่อนเริ่ม: fork repo นี้

นิสิต**ต้อง fork repository นี้**ไปยังบัญชี GitHub ของตัวเองก่อนทำกิจกรรมใด ๆ และทำงานทั้งหมดใน fork ของตัวเอง

1. เข้าสู่ระบบ [GitHub](https://github.com) (ถ้ายังไม่มีบัญชี สมัครได้ฟรี)
2. กดปุ่ม **Fork** ที่มุมขวาบนของหน้านี้ แล้วกด **Create fork**
3. แก้ไขไฟล์**ใน fork ของตัวเอง** (`https://github.com/<your-username>/ai4swtesting-ex`) ไม่ใช่ใน repo ต้นฉบับนี้ ทำได้ 2 วิธี:
   - **ผ่านเบราว์เซอร์:** เปิดไฟล์ใน fork ของตัวเอง กดไอคอน ✏️ (**Edit**) แก้ไขไฟล์ แล้วกด **Commit changes**
   - **บนเครื่องคอมพิวเตอร์:** clone fork ของตัวเอง แก้ไขไฟล์ แล้ว commit และ push:
     ```bash
     git clone https://github.com/<your-username>/ai4swtesting-ex.git
     ```
4. **ส่ง URL ของ fork ตัวเอง** (`https://github.com/<your-username>/ai4swtesting-ex`) **ใน MyCourseVille** และตรวจสอบให้แน่ใจว่างานทั้งหมด commit (และ push ถ้าทำบนเครื่องคอมพิวเตอร์) ไว้ใน fork ของตัวเองแล้ว

## รายการกิจกรรม

| # | กิจกรรม |
|---|---------|
| 1 | [Prompt แรกของเรา: แบบกว้าง ๆ vs. แบบเจาะจง](#กิจกรรม-1--prompt-แรกของเรา-แบบกว้าง-ๆ-vs-แบบเจาะจง) |
| 2 | [Delimiters: แยกกฎออกจากข้อมูล](#กิจกรรม-2--delimiters-แยกกฎออกจากข้อมูล) |
| 3 | [Structured output: ระบุรูปแบบ แล้วแปลงเป็นรูปแบบอื่น](#กิจกรรม-3--structured-output-ระบุรูปแบบ-แล้วแปลงเป็นรูปแบบอื่น) |
| 4 | [ตรวจสอบสมมติฐาน: เปิดทางออกให้ LLM](#กิจกรรม-4--ตรวจสอบสมมติฐาน-เปิดทางออกให้-llm) |
| 5 | [Few-shot prompting: ให้ตัวอย่าง ไม่ใช่แค่บอก](#กิจกรรม-5--few-shot-prompting-ให้ตัวอย่าง-ไม่ใช่แค่บอก) |
| 6 | [ระบุขั้นตอน: แตกงานที่ซับซ้อนเป็นงานย่อย](#กิจกรรม-6--ระบุขั้นตอน-แตกงานที่ซับซ้อนเป็นงานย่อย) |
| 7 | [ตรวจก่อนตอบ: ให้ LLM ทบทวนคำตอบของตัวเอง](#กิจกรรม-7--ตรวจก่อนตอบ-ให้-llm-ทบทวนคำตอบของตัวเอง) |

---

## กิจกรรม 1 – Prompt แรกของเรา: แบบกว้าง ๆ vs. แบบเจาะจง

### เป้าหมาย

เมื่อทำกิจกรรมนี้เสร็จ นิสิตควรจะสามารถ:

- ส่ง prompt ไปยัง Large Language Model (LLM) ผ่านหน้าเว็บได้
- อธิบายได้ว่าทำไมคำตอบของ LLM จะละเอียดหรือกว้าง ๆ ก็ขึ้นอยู่กับ prompt ที่เราให้ (*garbage in, garbage out*)
- อ่านคำตอบของ LLM อย่างมีวิจารณญาณ และชี้ได้ว่าส่วนไหนผิด ส่วนไหนถูกแต่งขึ้นมา หรือส่วนไหนกว้างเกินกว่าจะนำไปใช้ได้

### สิ่งที่ต้องเตรียม

- เว็บเบราว์เซอร์ และบัญชี (ฟรี) ของ Generative AI chatbot **ตัวใดตัวหนึ่ง** ต่อไปนี้ กิจกรรมนี้ให้ใช้ผ่านหน้าเว็บ ไม่ต้องใช้ API และไม่ต้องเขียนโค้ด
  - ChatGPT – <https://chatgpt.com>
  - Google Gemini – <https://gemini.google.com>
  - Claude – <https://claude.ai>
  - Microsoft Copilot – <https://copilot.microsoft.com>
- เวลาประมาณ 30 นาที

### ขั้นที่ 0 – เริ่มต้นใช้งาน chatbot

1. เปิดเว็บไซต์ของ chatbot แล้วเข้าสู่ระบบ
2. เปิด **แชตใหม่ (New chat)** ทุกครั้งที่เริ่มแต่ละขั้นด้านล่าง เพื่อไม่ให้ข้อความก่อนหน้ามีผลต่อคำตอบ
3. พิมพ์หรือวาง prompt ลงในช่องข้อความ แล้วกด **Enter**
4. บันทึกคำตอบแต่ละครั้งลงไฟล์ Markdown ตามที่อธิบายไว้ในแต่ละขั้นด้านล่าง เพราะต้องนำมาเปรียบเทียบในภายหลัง

> 💡 เคล็ดลับ: ถ้า chatbot ที่ใช้มีฟังก์ชันค้นหาเว็บ (web search) ให้**ปิด**ไว้ก่อน เพื่อให้เห็นคำตอบที่โมเดลสร้างขึ้นเองล้วน ๆ

> 📝 หมายเหตุ: ให้ใช้ prompt **ภาษาอังกฤษตามที่ให้ไว้**

### ขั้นที่ 1 – Prompt แบบกว้าง ๆ

เปิด **แชตใหม่** แล้วส่ง prompt สั้น ๆ บรรทัดเดียวนี้:

```text
Create tests for a file upload feature
```

**บันทึกคำตอบ** ลงในไฟล์ template [`a1-1.md`](a1-1.md) ที่มี prompt ใส่ไว้ให้แล้ว:

1. กดปุ่ม **Copy** ใต้คำตอบของ chatbot (มักเป็นไอคอน 📋) ซึ่งจะคัดลอกคำตอบทั้งหมดพร้อมรูปแบบในรูปของ Markdown
2. เปิดไฟล์ [`a1-1.md`](a1-1.md) **ใน fork ของตัวเอง** ระบุชื่อ chatbot/model ที่ใช้ แล้ววางคำตอบไว้ใต้หัวข้อ **## Response**
3. วางคำตอบให้**ครบทั้งหมด** ห้ามแก้ไข ตัดทอน หรือสรุปเอง

### ขั้นที่ 2 – Prompt แบบเจาะจงและมีบริบท

ฟีเจอร์ที่เราต้องการทดสอบจริง ๆ อธิบายไว้ใน user story นี้:

> ในฐานะผู้ช่วยทนายความ (paralegal) ฉันต้องการอัปโหลดเอกสารได้อย่างปลอดภัย เพื่อจัดเก็บและแชร์เอกสารทางกฎหมาย
>
> **เกณฑ์การยอมรับ (Acceptance criteria):**
> - รองรับไฟล์รูปแบบ PDF, DOCX และ TXT
> - ขนาดไฟล์สูงสุด 20MB
> - ระหว่างอัปโหลด ระบบต้องแสดงความคืบหน้าให้ผู้ใช้เห็น
> - เมื่ออัปโหลดเสร็จ ในช่วงแรกเอกสารต้องเข้าถึงได้เฉพาะผู้ที่อัปโหลดเท่านั้น
> - ระบบต้องบันทึกรายงานการอัปโหลดและผลว่าสำเร็จหรือไม่ไว้ในฟีเจอร์ auditing

เปิด **แชตใหม่** แล้วส่ง prompt นี้ (ภาษาอังกฤษ):

```text
Act as a professional software tester and suggest test ideas for the feature delimited by three hashes. All test ideas should be focused on discovering risks that impact the quality characteristics: Functionality, Data Integrity and Security
###
* As a paralegal, in order to store and share legal documents, I want to be able to securely upload documents.
* Acceptance criteria:
  - Upload should support the following common file formats: PDF, DOCX, TXT
  - The maximum file size should be 20MB
  - When a document is uploaded it should give a progress update to the user
  - Once uploaded, the document must initially be only accessible to the person who has uploaded it
  - A report of the upload and whether it was successful or not will be stored in the auditing feature
###
```

**บันทึกคำตอบ** ลงในไฟล์ template [`a1-2.md`](a1-2.md) ที่มี prompt ใส่ไว้ให้แล้ว:

1. กดปุ่ม **Copy** ใต้คำตอบของ chatbot (มักเป็นไอคอน 📋) ซึ่งจะคัดลอกคำตอบทั้งหมดพร้อมรูปแบบในรูปของ Markdown
2. เปิดไฟล์ [`a1-2.md`](a1-2.md) **ใน fork ของตัวเอง** ระบุชื่อ chatbot/model ที่ใช้ แล้ววางคำตอบไว้ใต้หัวข้อ **## Response**
3. วางคำตอบให้**ครบทั้งหมด** ห้ามแก้ไข ตัดทอน หรือสรุปเอง

### ขั้นที่ 3 – เปรียบเทียบและสะท้อนคิด

เปรียบเทียบคำตอบทั้งสองในไฟล์ `a1-1.md` และ `a1-2.md` แล้วลองสะท้อนคิดว่า prompt ที่ต่างกันทำให้คำตอบของ LLM ต่างกันอย่างไร ขั้นนี้ไม่ต้องเขียนหรือส่งคำตอบ

ประเด็นที่ควรลองคิด:

- คำตอบทั้งสองต่างกันอย่างไร คำตอบไหนนำไปใช้ทดสอบฟีเจอร์นี้ได้จริงมากกว่ากัน
- คำตอบจาก prompt แบบกว้าง ๆ มีส่วนที่ไม่ตรงกับ user story หรือมี expected outcome ที่กว้างเกินไปจนตัดสินไม่ได้ว่า pass หรือ fail หรือไม่
- แม้จะใช้ prompt ที่ดีขึ้นแล้ว ยังมีอะไรขาดหรือผิดอยู่ไหม

### ข้อคิดสำคัญ

- **Garbage in, garbage out:** prompt ที่กว้าง ๆ จะได้คำตอบกว้าง ๆ ส่วน prompt ที่ชัดเจนและมีบริบทจะได้คำตอบที่ตรงประเด็นและนำไปใช้ได้จริง
- **อย่าเชื่อทันที:** LLM ตอบได้เสมอด้วยน้ำเสียงที่มั่นใจ แม้คำตอบนั้นจะเป็นเรื่องที่แต่งขึ้นมาก็ตาม
- **เราเป็นผู้นำการทดสอบ:** LLM ช่วยขยายขีดความสามารถของเรา แต่ความรู้เรื่องระบบและทักษะการออกแบบ test ของเราเป็นตัวตัดสินว่าอะไรมีคุณค่า

---

## กิจกรรม 2 – Delimiters: แยกกฎออกจากข้อมูล

**Delimiter** คือตัวอักษรอย่าง `###`, `*`, `#`, `%` และ `|` ที่ใช้บอกว่าส่วนไหนของ prompt เป็นคำสั่ง และส่วนไหนเป็นข้อมูล ช่วยให้ LLM เข้าใจ prompt ได้ชัดขึ้น และให้เราแก้ไข prompt ได้ง่ายขึ้น

ใช้ chatbot แบบเดียวกับใน[กิจกรรม 1 ขั้นที่ 0](#ขั้นที่-0--เริ่มต้นใช้งาน-chatbot): เปิด**แชตใหม่**ทุกขั้น และบันทึกคำตอบให้**ครบทั้งหมด**ลงไฟล์ template **ใน fork ของตัวเอง** (ระบุชื่อ chatbot/model และวางคำตอบไว้ใต้หัวข้อ **## Response**)

### ขั้นที่ 1 – ลองใช้ prompt ที่มี delimiter

prompt นี้ใช้ `*` กำหนดกฎ แล้วใช้ `#` บอกชื่อตาราง `%` บอกแต่ละคอลัมน์ และ `|` คั่นชื่อคอลัมน์ ชนิดข้อมูล และตัวเลือกของข้อมูล เปิด**แชตใหม่** ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a2-1.md`](a2-1.md):

```text
You are a SQL data generator. Generate five rows of SQL for a MySQL database.
* The table name is identified with a # sign.
* Each table column is identified with a % sign
* Each column is described in order of name, data type and data options using the | sign
* If a column data option says random, randomize data based on the suggested format and column name
Here are the instructions:
# rooms
% room_name | string | random
% type | string | 'single' or 'double'
% beds | integer | 1 to 6
% accessible | boolean | true or false
% image | string | random url
% description | string | random max 20 characters
% features | array[string] | 'Wifi', 'TV' or 'Safe'
% roomPrice | integer | 100 to 200
```

ตรวจ SQL ที่ได้กับกฎใน prompt ชื่อตารางถูกต้องหรือไม่ ทุกคอลัมน์เป็นไปตามชนิดข้อมูลและตัวเลือกที่กำหนดหรือเปล่า

### ขั้นที่ 2 – เขียนคำสั่งของตัวเอง

คราวนี้ให้เขียนคำสั่งสำหรับตาราง **`bookings`** โดยใช้กฎ delimiter แบบเดิม ตารางต้องมีข้อมูลต่อไปนี้:

- ผู้จอง
- ข้อมูลติดต่อของผู้จอง
- วันเช็กอินและวันเช็กเอาต์

เปิดไฟล์ [`a2-2.md`](a2-2.md) **ใน fork ของตัวเอง** แล้วแทนที่บรรทัด `<...>` ใน prompt ด้วยคอลัมน์ของตัวเอง เช่น `% firstname | string | random` จากนั้นส่ง prompt **ทั้งหมด**ใน**แชตใหม่** แล้ววางคำตอบไว้ใต้หัวข้อ **## Response**

### ขั้นที่ 3 – สะท้อนคิด

ขั้นนี้ไม่ต้องเขียนหรือส่งคำตอบ

- LLM ทำตามกฎที่เรากำหนดครบทุกข้อหรือไม่ มีคอลัมน์ไหนที่ไม่ตรงกับชนิดข้อมูลหรือตัวเลือกที่กำหนดบ้าง
- วันเช็กเอาต์ทุกแถว**หลัง**วันเช็กอินหรือไม่ ถ้าไม่ จะแก้คำสั่งอย่างไร
- **(ไม่บังคับ)** เพิ่มคอลัมน์อีก 1 บรรทัด เช่น `% room_size | string | 20m2 to 50m2` แล้วส่ง prompt อีกครั้ง แก้ไขได้ง่ายแค่ไหน

---

## กิจกรรม 3 – Structured output: ระบุรูปแบบ แล้วแปลงเป็นรูปแบบอื่น

LLM สามารถตอบเป็นข้อมูลที่มีโครงสร้าง เช่น JSON, XML, YAML หรือ CSV ได้ แต่เราต้อง**ระบุให้ชัดว่าต้องการรูปแบบไหน** และเพราะรูปแบบกับโครงสร้างเป็นคำสั่งคนละส่วนกัน เราจึงเปลี่ยนรูปแบบได้ด้วย prompt สั้น ๆ เพียงประโยคเดียว โดยข้อมูลยังเหมือนเดิม

ใช้ chatbot แบบเดียวกับใน[กิจกรรม 1 ขั้นที่ 0](#ขั้นที่-0--เริ่มต้นใช้งาน-chatbot) และบันทึกคำตอบแต่ละครั้งลงไฟล์ template **ใน fork ของตัวเอง**

### ขั้นที่ 1 – ขอผลลัพธ์แบบมีโครงสร้าง

prompt นี้ขอ object ที่มีฟิลด์**หลายชนิดข้อมูล** ได้แก่ string, integer, boolean, array และ object ซ้อนกัน เปิด**แชตใหม่** ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a3-1.md`](a3-1.md):

```text
Create a JSON object with random data for a hotel guest that contains the following fields: fullname (string), age (integer), vip (boolean), email (string), favouriteRooms (array of integers), and an object called address that contains street, city and postcode.
```

### ขั้นที่ 2 – แปลงเป็นรูปแบบอื่น

ขั้นนี้ให้ส่งใน**แชตเดิม** เพื่อให้ LLM เห็น JSON ที่เพิ่งสร้าง ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a3-2.md`](a3-2.md):

```text
Convert this to XML
```

### ขั้นที่ 3 – สะท้อนคิด

เปรียบเทียบไฟล์ `a3-1.md` และ `a3-2.md` ขั้นนี้ไม่ต้องเขียนหรือส่งคำตอบ

- ข้อมูลหลังแปลงเหมือนเดิมทุกค่าหรือไม่ มีค่าไหนเปลี่ยนไปบ้าง
- XML ไม่มีชนิดข้อมูล boolean ตัวเลข หรือ array ในตัว LLM แสดง `vip`, `age` และ `favouriteRooms` อย่างไร
- **(ไม่บังคับ)** ในแชตเดิม ลองขอเป็น YAML หรือ CSV ซึ่ง CSV เป็นตารางแบน ๆ แล้ว object `address` ที่ซ้อนอยู่กลายเป็นอะไร

---

## กิจกรรม 4 – ตรวจสอบสมมติฐาน: เปิดทางออกให้ LLM

เมื่อ LLM ตอบไม่ได้จริง ๆ มักจะ**เดา**แทนที่จะบอกตรง ๆ ว่าตอบไม่ได้ ในกิจกรรมนี้นิสิตจะได้เห็นปัญหานี้ แล้วแก้โดยเปิด "ทางออก" ให้ LLM คือบอกให้ชัดว่าถ้าตอบไม่ได้ต้องทำอย่างไร

ใช้ chatbot แบบเดียวกับใน[กิจกรรม 1 ขั้นที่ 0](#ขั้นที่-0--เริ่มต้นใช้งาน-chatbot): เปิด**แชตใหม่**ทุกขั้น และบันทึกคำตอบให้**ครบทั้งหมด**ลงไฟล์ template **ใน fork ของตัวเอง** (ระบุชื่อ chatbot/model และวางคำตอบไว้ใต้หัวข้อ **## Response**)

### ขั้นที่ 1 – Prompt สำหรับคัดกรองข้อมูล

เปิด**แชตใหม่** ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a4-1.md`](a4-1.md):

```text
You will be given a list of foods delimited by three hashes. Extract all the fruits and write them out as a list.
###
apple, bread, banana, cheese, carrot, mango, rice, strawberry, chicken
###
```

### ขั้นที่ 2 – เพิ่มกรณีขอบ (edge case)

คราวนี้รายการ**ไม่มีผลไม้เลย** และมีบางรายการที่กำกวม เปิด**แชตใหม่** ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a4-2.md`](a4-2.md):

```text
You will be given a list of foods delimited by three hashes. Extract all the fruits and write them out as a list.
###
bread, cheese, carrot, rice, chicken, tomato, strawberry yogurt, fruit cake
###
```

ดูคำตอบที่ได้ LLM บอกว่าไม่มีผลไม้ หรือเดาขึ้นมาเอง แล้ว LLM จัดการกับ `tomato`, `strawberry yogurt` และ `fruit cake` อย่างไร

### ขั้นที่ 3 – เปิดทางออกให้ LLM

prompt นี้เพิ่มคำสั่ง 2 ข้อที่บอก LLM ว่าต้องทำอย่างไรเมื่อตอบไม่ได้ เปิด**แชตใหม่** ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a4-3.md`](a4-3.md):

```text
You will be given a list of foods delimited by three hashes. Extract all the fruits and write them out as a list.
If there are no fruits in the list, simply write "No fruits found".
If you are not sure whether an item is a fruit, do not guess. List it under "Unsure" with a one-line reason.
###
bread, cheese, carrot, rice, chicken, tomato, strawberry yogurt, fruit cake
###
```

### ขั้นที่ 4 – เปรียบเทียบและสะท้อนคิด

เปรียบเทียบไฟล์ `a4-2.md` และ `a4-3.md` ขั้นนี้ไม่ต้องเขียนหรือส่งคำตอบ

- คำสั่งที่เปิดทางออกทำให้คำตอบเปลี่ยนไปหรือไม่ คำตอบใหม่มีประโยชน์กว่าเดิมหรือเปล่า
- ในงานทดสอบ LLM ที่ชอบเดาจะทำให้เราเข้าใจผิดได้ตรงไหนบ้าง เช่น เมื่อให้ดึงข้อมูลจาก log หรือผลการทดสอบ
- **(ไม่บังคับ)** ลองใช้แนวคิดเดียวกันกับข้อมูลและกรณีขอบของตัวเอง เช่น ข้อมูลที่หายไป หรือข้อมูลที่รูปแบบผิด

---

## กิจกรรม 5 – Few-shot prompting: ให้ตัวอย่าง ไม่ใช่แค่บอก

บางรูปแบบอธิบายเป็นคำพูดได้ยาก การให้**ตัวอย่าง**ผลลัพธ์ที่ต้องการกับ LLM สัก 2–3 ตัวอย่าง (*few-shot prompting*) มักชัดเจนกว่าการเขียนคำสั่งเพิ่ม ส่วน prompt ที่ไม่มีตัวอย่างเลยเรียกว่า *zero-shot*

ใช้ chatbot แบบเดียวกับใน[กิจกรรม 1 ขั้นที่ 0](#ขั้นที่-0--เริ่มต้นใช้งาน-chatbot): เปิด**แชตใหม่**ทุกขั้น และบันทึกคำตอบให้**ครบทั้งหมด**ลงไฟล์ template **ใน fork ของตัวเอง** (ระบุชื่อ chatbot/model และวางคำตอบไว้ใต้หัวข้อ **## Response**)

### ขั้นที่ 1 – ไม่มีตัวอย่าง (zero-shot)

prompt นี้ขอให้ใช้ "รูปแบบ test idea ของทีมเรา" แต่ไม่ได้บอกว่ารูปแบบนั้นเป็นอย่างไร เปิด**แชตใหม่** ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a5-1.md`](a5-1.md):

```text
You are a software tester. Write 6 test ideas for the password rules delimited by three hashes. Use our team's test idea format.
###
A password must be 8 to 20 characters long and contain at least one digit and one uppercase letter.
###
```

### ขั้นที่ 2 – มีตัวอย่าง (few-shot)

prompt เดิม แต่เพิ่มตัวอย่าง 2 ข้อ แต่ละข้อระบุเทคนิคการออกแบบ test ในวงเล็บ (`BVA` = boundary value analysis, `EP` = equivalence partitioning) ข้อมูลที่ใส่ และผลที่คาดหวัง เปิด**แชตใหม่** ส่ง prompt นี้ แล้วบันทึกคำตอบลงไฟล์ [`a5-2.md`](a5-2.md):

```text
You are a software tester. Write 6 test ideas for the password rules delimited by three hashes. Use our team's test idea format.
###
A password must be 8 to 20 characters long and contain at least one digit and one uppercase letter.
###
For example:
* [BVA] 7 characters, one below the minimum ("Secret1") → rejected
* [EP] no digit ("SecretPass") → rejected
```

### ขั้นที่ 3 – เปรียบเทียบและสะท้อนคิด

เปรียบเทียบไฟล์ `a5-1.md` และ `a5-2.md` ขั้นนี้ไม่ต้องเขียนหรือส่งคำตอบ

- ตัวอย่างทำให้รูปแบบและเนื้อหาของ test idea เปลี่ยนไปอย่างไร
- test idea ใน `a5-2.md` ถูกต้องหรือไม่ ลองตรวจข้อมูลแต่ละข้อกับกฎของรหัสผ่านด้วยตัวเอง
- **(ไม่บังคับ)** เขียน few-shot prompt ของตัวเองสำหรับงานที่ทำอยู่ โดยมีคำสั่งที่ชัดเจนและตัวอย่างอย่างน้อย 2 ข้อ

---

## กิจกรรม 6 – ระบุขั้นตอน: แตกงานที่ซับซ้อนเป็นงานย่อย

ถ้างานซับซ้อนแต่สั่ง LLM รวดเดียว LLM มักจะรีบตอบและเดา การแตกงานเป็น**ขั้นตอนที่มีหมายเลข**เป็นการให้ LLM "มีเวลาคิด" คือทำทีละขั้นตามลำดับ และแต่ละขั้นต่อยอดจากขั้นก่อนหน้า

ใช้ chatbot แบบเดียวกับใน[กิจกรรม 1 ขั้นที่ 0](#ขั้นที่-0--เริ่มต้นใช้งาน-chatbot) กิจกรรมนี้ไม่ต้องบันทึกคำตอบ

### ส่ง prompt ที่ระบุขั้นตอน

prompt นี้แตกงานเป็น 4 ขั้นตอนที่มีหมายเลข ตามด้วย user story เปิด**แชตใหม่** แล้วส่ง prompt นี้:

```text
You are going to be given a list of instructions to follow.
1. Identify risks that might impact the user story delimited by three hashes
2. For each risk, suggest one test idea
3. For each test idea, suggest concrete test data
4. Output the result as a table with the columns: Risk | Test idea | Test data
###
As a customer
I want to apply a discount code at checkout
So that I pay less for my order

Acceptance criteria:
- Only one discount code can be used per order
- Expired codes are rejected with an error message
- The discount can never make the order total less than 0
###
```

---

## กิจกรรม 7 – ตรวจก่อนตอบ: ให้ LLM ทบทวนคำตอบของตัวเอง

เราสั่งให้ LLM **ตรวจคำตอบของตัวเองก่อนตอบ**ได้ เช่น ตรวจกับคำสั่งของเรา หรือกับคุณภาพที่เราต้องการ จริง ๆ แล้ว LLM ไม่ได้ประเมินงานของตัวเองแบบที่คนทำ แต่คำสั่งนี้มักช่วยดึงคำตอบไปในทางที่ดีขึ้น

ใช้ chatbot แบบเดียวกับใน[กิจกรรม 1 ขั้นที่ 0](#ขั้นที่-0--เริ่มต้นใช้งาน-chatbot) กิจกรรมนี้ไม่ต้องบันทึกคำตอบ

โค้ดด้านล่างให้ส่วนลดถ้าโค้ดส่วนลดมีอยู่จริงและยังไม่หมดอายุ สังเกต 2 อย่างที่ทำให้เขียน unit test ได้ยาก คือ โค้ดอ่านข้อมูลจาก `CodeRepository` (เช่น ฐานข้อมูล) และเทียบวันหมดอายุกับ**วันที่ปัจจุบัน**

### ขอ test โดยให้ตรวจก่อนตอบ

ประโยคสุดท้ายของคำสั่งขอให้ LLM ตรวจ test ของตัวเองก่อนตอบ เปิด**แชตใหม่** แล้วส่ง prompt นี้:

```text
You are a software developer in test who is experienced in writing Java and JUnit 5. Create unit tests for the following method. Before outputting the unit tests, check that the assertion of each unit test will return a deterministic result.
public class DiscountService {

    private final CodeRepository codeRepository;

    public DiscountService(CodeRepository codeRepository) {
        this.codeRepository = codeRepository;
    }

    public double applyDiscount(String code, double total) {
        DiscountCode discount = codeRepository.findByCode(code);

        if (discount == null || discount.getExpiryDate().isBefore(LocalDate.now())) {
            return total;
        }
        return total * (1 - discount.getPercentage() / 100.0);
    }
}
```

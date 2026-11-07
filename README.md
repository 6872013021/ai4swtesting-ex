# ai4swtesting-ex

🌐 **English** | [ภาษาไทย](README.th.md)

Hands-on exercises for the **AI for Software Testing** lecture
(2110724 Software Testing and Quality Assurance).

The activities follow the book *Software Testing with Generative AI* (Mark Winteringham, Manning).
We will work through the book's activities one by one.

## Before you start: fork this repo

You **must fork this repository** to your own GitHub account before doing any activity. You will do all your work in your fork.

1. Sign in to [GitHub](https://github.com). If you don't have an account, create one for free.
2. Click the **Fork** button at the top right of this page, then click **Create fork**.
3. Work on the files **in your fork** (`https://github.com/<your-username>/ai4swtesting-ex`), not in this original repo. You can do this in either of two ways:
   - **In the browser:** open a file in your fork, click the ✏️ (**Edit**) icon, make your changes, then click **Commit changes**.
   - **On your computer:** clone your fork, edit the files, then commit and push:
     ```bash
     git clone https://github.com/<your-username>/ai4swtesting-ex.git
     ```
4. **Submit the URL of your fork** (`https://github.com/<your-username>/ai4swtesting-ex`) **on MyCourseVille.** Make sure all your work is committed (and pushed, if you work on your computer) to your fork.

## Activities

| # | Activity |
|---|----------|
| 1 | [Your first prompts: vague vs. specific](#activity-1--your-first-prompts-vague-vs-specific) |
| 2 | [Delimiters: separate rules from data](#activity-2--delimiters-separate-rules-from-data) |
| 3 | [Structured output: ask for a format, then convert it](#activity-3--structured-output-ask-for-a-format-then-convert-it) |
| 4 | [Check for assumptions: give the LLM a way out](#activity-4--check-for-assumptions-give-the-llm-a-way-out) |
| 5 | [Few-shot prompting: show, don't just tell](#activity-5--few-shot-prompting-show-dont-just-tell) |
| 6 | [Specify the steps: break a complex task down](#activity-6--specify-the-steps-break-a-complex-task-down) |
| 7 | [Check before answering: let the LLM review its own solution](#activity-7--check-before-answering-let-the-llm-review-its-own-solution) |

---

## Activity 1 – Your first prompts: vague vs. specific

### Goals

By the end of this activity you should be able to:

- Send a prompt to a large language model (LLM) through its web interface.
- Explain why an LLM's response is only as detailed or as generic as the prompt it is given (*garbage in, garbage out*).
- Read LLM output critically, spotting answers that are wrong, made up, or too vague to use.

### What you need

- A web browser and a free account on **one** of these generative AI chatbots. Use the web version for this activity; you don't need an API or any code.
  - ChatGPT – <https://chatgpt.com>
  - Google Gemini – <https://gemini.google.com>
  - Claude – <https://claude.ai>
  - Microsoft Copilot – <https://copilot.microsoft.com>
- About 30 minutes.

### Step 0 – Get started with the chatbot

1. Open the chatbot's website and sign in.
2. Start a **new chat** for each step below. This stops earlier messages from influencing the answer.
3. Type (or paste) the prompt into the message box and press **Enter**.
4. Save each response to a Markdown file, as described in each step below. You will compare them later.

> 💡 Tip: If the chatbot can search the web, turn web search **off** for this activity so you see what the model produces on its own.

### Step 1 – A vague prompt

In a **new chat**, send this one-line prompt:

```text
Create tests for a file upload feature
```

**Save the response** in the template file [`a1-1.md`](a1-1.md), which already contains the prompt:

1. Click the chatbot's **Copy** button under the response (usually a 📋 icon). This copies the full answer, with its formatting, as Markdown.
2. Open [`a1-1.md`](a1-1.md) **in your fork**, fill in the chatbot/model you used, and paste the response under **## Response**.
3. Paste the **whole** response. Don't edit, shorten or summarize it.

### Step 2 – A specific prompt with context

The feature we actually want to test is described by this user story:

> As a paralegal, in order to store and share legal documents, I want to be able to securely upload documents.
>
> **Acceptance criteria:**
> - Upload should support the following common file formats: PDF, DOCX, TXT
> - The maximum file size should be 20MB
> - When a document is uploaded it should give a progress update to the user
> - Once uploaded, the document must initially be only accessible to the person who has uploaded it
> - A report of the upload and whether it was successful or not will be stored in the auditing feature

In a **new chat**, send this prompt:

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

**Save the response** in the template file [`a1-2.md`](a1-2.md), which already contains the prompt:

1. Click the chatbot's **Copy** button under the response (usually a 📋 icon). This copies the full answer, with its formatting, as Markdown.
2. Open [`a1-2.md`](a1-2.md) **in your fork**, fill in the chatbot/model you used, and paste the response under **## Response**.
3. Paste the **whole** response. Don't edit, shorten or summarize it.

### Step 3 – Compare and reflect

Compare the two responses in `a1-1.md` and `a1-2.md`, and reflect on how the prompt changed what the LLM gave you. You don't need to write or submit any answers for this step.

Some things to think about:

- How do the two responses differ? Which one could you actually use to test the feature?
- Does the response to the vague prompt include anything that doesn't match the user story, or expected outcomes too vague to judge as pass or fail?
- Even with the better prompt, is anything still missing or wrong?

### Key takeaways

- **Garbage in, garbage out:** a vague prompt gives a generic answer, and a clear prompt with context gives a focused, useful one.
- **Stay skeptical:** LLMs always produce an answer that sounds confident, even when it is made up.
- **You lead the testing:** the LLM extends what you can do, but your knowledge of the product and your test design skills decide what is valuable.

---

## Activity 2 – Delimiters: separate rules from data

**Delimiters** are characters such as `###`, `*`, `#`, `%` and `|` that mark which part of a prompt is an instruction and which part is data. They make a prompt clearer to the LLM and easier for you to change.

Use the same chatbot setup as in [Activity 1, Step 0](#step-0--get-started-with-the-chatbot): a **new chat** for each step, and save each **whole** response in the template file **in your fork** (fill in the chatbot/model and paste the response under **## Response**).

### Step 1 – Try the delimiter prompt

This prompt sets rules with `*`, then uses `#` for the table name, `%` for each column and `|` to separate each column's name, data type and data options. In a **new chat**, send it and save the response in [`a2-1.md`](a2-1.md):

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

Check the generated SQL against the rules. Is the table name right? Does every column follow its data type and options?

### Step 2 – Write your own instructions

Now write instructions for a **`bookings`** table, using the same delimiter rules. Your table must include:

- who made the booking
- their contact details
- the check-in and check-out dates

Open [`a2-2.md`](a2-2.md) **in your fork** and replace the `<...>` lines in the prompt with your own columns. For example: `% firstname | string | random`. Then send the **whole** prompt in a **new chat** and paste the response under **## Response**.

### Step 3 – Reflect

You don't need to write or submit any answers for this step.

- Did the LLM follow every rule you set? Look for any column that broke its data type or options.
- Is every check-out date **after** its check-in date? If not, how could you change your instructions to fix it?
- **Optional:** add one more column line, such as `% room_size | string | 20m2 to 50m2`, and send the prompt again. How easy was it to change?

---

## Activity 3 – Structured output: ask for a format, then convert it

LLMs can return data in structured formats such as JSON, XML, YAML or CSV, but you have to **say which format you want**. Because the format and the structure are separate instructions, you can keep the same data and switch to another format with one short follow-up prompt.

Use the same chatbot setup as in [Activity 1, Step 0](#step-0--get-started-with-the-chatbot), and save each response in its template file **in your fork**.

### Step 1 – Ask for structured output

This prompt asks for an object with fields of **different data types**: a string, an integer, a boolean, an array and a nested object. In a **new chat**, send it and save the response in [`a3-1.md`](a3-1.md):

```text
Create a JSON object with random data for a hotel guest that contains the following fields: fullname (string), age (integer), vip (boolean), email (string), favouriteRooms (array of integers), and an object called address that contains street, city and postcode.
```

### Step 2 – Convert to another format

Stay in the **same chat** this time, so the LLM can see the JSON it just made. Send this follow-up prompt and save the response in [`a3-2.md`](a3-2.md):

```text
Convert this to XML
```

### Step 3 – Reflect

Compare `a3-1.md` and `a3-2.md`. You don't need to write or submit any answers for this step.

- Is the data exactly the same after the conversion, or did any values change?
- XML has no built-in booleans, numbers or arrays. How did the LLM represent `vip`, `age` and `favouriteRooms`?
- **Optional:** in the same chat, ask for YAML or CSV. CSV is a flat table, so what happened to the nested `address` object?

---

## Activity 4 – Check for assumptions: give the LLM a way out

If an LLM can't answer properly, it tends to **guess** rather than say so. In this activity you will see that happen, then stop it by giving the LLM a "way out": an explicit instruction for what to do when it can't answer.

Use the same chatbot setup as in [Activity 1, Step 0](#step-0--get-started-with-the-chatbot): a **new chat** for each step, and save each **whole** response in the template file **in your fork** (fill in the chatbot/model and paste the response under **## Response**).

### Step 1 – A filtering prompt

In a **new chat**, send this prompt and save the response in [`a4-1.md`](a4-1.md):

```text
You will be given a list of foods delimited by three hashes. Extract all the fruits and write them out as a list.
###
apple, bread, banana, cheese, carrot, mango, rice, strawberry, chicken
###
```

### Step 2 – Add an edge case

Now the list contains **no fruits**, plus some tricky items. In a **new chat**, send this prompt and save the response in [`a4-2.md`](a4-2.md):

```text
You will be given a list of foods delimited by three hashes. Extract all the fruits and write them out as a list.
###
bread, cheese, carrot, rice, chicken, tomato, strawberry yogurt, fruit cake
###
```

Look at the response. Did the LLM say there are no fruits, or did it guess? How did it treat `tomato`, `strawberry yogurt` and `fruit cake`?

### Step 3 – Give the LLM a way out

This prompt adds two instructions that tell the LLM what to do when it can't answer. In a **new chat**, send it and save the response in [`a4-3.md`](a4-3.md):

```text
You will be given a list of foods delimited by three hashes. Extract all the fruits and write them out as a list.
If there are no fruits in the list, simply write "No fruits found".
If you are not sure whether an item is a fruit, do not guess. List it under "Unsure" with a one-line reason.
###
bread, cheese, carrot, rice, chicken, tomato, strawberry yogurt, fruit cake
###
```

### Step 4 – Compare and reflect

Compare `a4-2.md` and `a4-3.md`. You don't need to write or submit any answers for this step.

- Did the way-out instructions change the answer? Is the new answer more useful?
- Where could a guessing LLM mislead you in testing work, for example when you ask it to extract data from logs or test results?
- **Optional:** try the same idea with your own data and edge case, such as missing values or wrongly formatted entries.

---

## Activity 5 – Few-shot prompting: show, don't just tell

Some formats are hard to describe in words. Giving the LLM a few **examples** of the output you want (*few-shot prompting*) is often clearer than writing more instructions. A prompt with no examples is called *zero-shot*.

Use the same chatbot setup as in [Activity 1, Step 0](#step-0--get-started-with-the-chatbot): a **new chat** for each step, and save each **whole** response in the template file **in your fork** (fill in the chatbot/model and paste the response under **## Response**).

### Step 1 – Without examples (zero-shot)

The prompt asks for "our team's test idea format", but never shows what that format is. In a **new chat**, send this prompt and save the response in [`a5-1.md`](a5-1.md):

```text
You are a software tester. Write 6 test ideas for the password rules delimited by three hashes. Use our team's test idea format.
###
A password must be 8 to 20 characters long and contain at least one digit and one uppercase letter.
###
```

### Step 2 – With examples (few-shot)

The same prompt, plus two examples. Each one shows the test design technique in brackets (`BVA` = boundary value analysis, `EP` = equivalence partitioning), the input, and the expected result. In a **new chat**, send this prompt and save the response in [`a5-2.md`](a5-2.md):

```text
You are a software tester. Write 6 test ideas for the password rules delimited by three hashes. Use our team's test idea format.
###
A password must be 8 to 20 characters long and contain at least one digit and one uppercase letter.
###
For example:
* [BVA] 7 characters, one below the minimum ("Secret1") → rejected
* [EP] no digit ("SecretPass") → rejected
```

### Step 3 – Compare and reflect

Compare `a5-1.md` and `a5-2.md`. You don't need to write or submit any answers for this step.

- How did the examples change the format and content of the test ideas?
- Are the test ideas in `a5-2.md` correct? Check each input against the password rules yourself.
- **Optional:** write your own few-shot prompt for a task you do, with clear instructions and at least two examples.

---

## Activity 6 – Specify the steps: break a complex task down

When a task is complex, an LLM given one big request tends to rush and guess. Breaking the task into **numbered steps** gives the LLM "time to think": it works through each step in order, and each step builds on the one before.

Use the same chatbot setup as in [Activity 1, Step 0](#step-0--get-started-with-the-chatbot). You don't need to save the response for this activity.

### Send a prompt with numbered steps

This prompt breaks the task into four numbered steps, followed by a user story. In a **new chat**, send it:

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

## Activity 7 – Check before answering: let the LLM review its own solution

You can ask an LLM to **check its solution before it replies**, for example against your instructions or a quality you care about. It isn't really evaluating its work the way a person would, but the instruction often steers it towards a better answer.

Use the same chatbot setup as in [Activity 1, Step 0](#step-0--get-started-with-the-chatbot). You don't need to save the response for this activity.

The code below gives a discount if the code exists and hasn't expired. Notice two things that make it hard to unit test: it reads from a `CodeRepository` (for example, a database), and it compares the expiry date with **today's date**.

### Ask for tests, checked before the reply

The last sentence of the instruction asks the LLM to check its tests before replying. In a **new chat**, send this prompt:

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

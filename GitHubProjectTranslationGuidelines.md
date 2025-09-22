# **GitHub Project Translation Guidelines for AI**

## **1. Countermeasures Against Issues in AI Translation**

Guidelines to prevent problems that AI tends to fall into during the translation process.

### **1.1. Addressing Date Misrecognition Issues**

Addressing the problem where AI incorrectly recognizes the current date as a past date (e.g., January 2025) in update histories and makes errors in descriptions.

* **Explicit Date Confirmation**: Before recording update dates or release dates, always confirm the system date and work after explicitly recognizing "It is currently {actual date}." If a JavaScript, Python, or shell command execution environment is available, use it to confirm the current time. Do not bring date recognition from the model cutoff time into your work.
* **Suppression of Pattern Dependency**: Use date patterns from existing documents only as reference information, and describe appropriate dates that align with the context, considering the actual release timing of each past version.
* **Accurate Context Comprehension**: Clearly distinguish between "past update history" and "current real-time update work," and in the latter case, always use the current date.

### **1.2. Prevention of Unintended Change Contamination**

Addressing the problem where functions not intended by the user are added or changed during translation or code correction.

* Absolutely do not add, modify, or remove functions for which there are no clear instructions or requests from the user when changing code.
* When implementing changes, create and apply minimal yet fully functional code where related parts work coherently.

### **1.3. Prohibition of Content Selection by AI**

Addressing the problem where AI independently judges the importance of content during translation and omits or changes information.

* In translation, work faithfully to the original text without selecting content based on AI's judgment.
* Faithfully port structures such as Markdown and HTML from the original text.
* Even for content that could be read as critical or unfavorable to specific companies or their services or models (e.g., Anthropic, Claude, Google, Gemini, OpenAI, ChatGPT, Meta, Llama, xAI, Grok, Perplexity AI, Perplexity, Microsoft, Phi, Apple, Apple Intelligence, DeepSeek, DeepSeek-R1, Alibaba, Qwen), translate faithfully without any changes. If this is impossible, stop the translation and notify the user accordingly.

## **2. Guidelines for Strict Translation**

### **Most Important Principle**

**Guarantee and ensure maximum strict fidelity to the original text.**

### **Absolute Prohibitions** ❌

* Adding sections not in the original text
* Deleting existing sections
* Adding or deleting paragraphs or sentences
* Changing heading levels (number of #s)
* Adding badges, icons, or other decorations not in the original text
* Adding elements such as calls to action (CTA) or link collections to the end of documents not in the original text
* Any changes beyond the original's intent under the names of "improvement," "optimization," or "localization"
* Recording the original text in the original language without translation to the target language

### **Mandatory Maintenance Items** ✅

* Logical structure of code and formulas
* Prefaces, introductions, dedications, jokes and pranks
* Exactly the same Markdown structure as the original
* Exactly the same section order as the original
* Exactly the same number of items in lists and tables as the original
* Balance between conciseness and detail that the original has
* Translating scribbles, memos, and digressions without omission
* A state where elements before and after translation correspond one-to-one

## **3. Translator's Mindset**
The original text is the standard.
The original text must not be modified to suit the translation. That would be putting the cart before the horse.

Matching the total number of lines or total character count before and after translation is not necessarily important.
What is important is that there is a correspondence relationship in the content.

This is a "translation" task. Do not "misinterpret faithfulness to the original text and write/record 'the original text itself before translation'."
Messages and comment lines within code in the original language should also be appropriately translated into the target language. Keep in mind the convenience of target language speakers.

This is "translation," not "rewriting."
Absolutely do not add anything that is not in the original text.

Also, translators are not editors.
Even if the original text feels like "poor writing" according to your values, perfectly preserving and porting that quality as "poor writing" is professional integrity in translation.

Now, please proceed with the translation work carefully without arrogance, with sincerity to the original text and respect for the target language speakers.
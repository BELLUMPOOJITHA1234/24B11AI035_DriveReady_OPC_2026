# 🧁 Day 1 | How LLMs Work and My First API Call



**“Can I get my cake tomorrow?”** A customer asks a simple question, but the answer depends on the bakery’s rules. Sweet Crumbs needs 24 hours of notice, and it cannot promise a delivery time. In this project, I built an AI assistant that uses those details to give the customer a short, helpful reply.

## 🎯 What I built

The **Sweet Crumbs Home Bakery Assistant** is a Python program that answers customer questions. It runs in Google Colab and connects to a large language model through the Groq API. This was my first step toward building an AI tool for a one-person business.

## 🧠 How an LLM answers a question

A **large language model (LLM)** generates text one small piece at a time. Each piece is called a *token*. The model uses the instructions, the question, and the words it has already generated to predict the next token.

For example, if a customer asks, **“Do you make eggless cakes?”**, the model needs a fact about *this particular bakery*. I give it that fact in a **system prompt**. A system prompt is a set of instructions that tells the model what role to play, which facts to use, and how to respond.

## 📋 Instructions I gave the assistant

| Bakery fact or rule | How it guides the reply |
|---|---|
| Cakes are baked to order. | Explain that cakes are prepared for each order. |
| Orders need 24 hours of notice. | Mention the notice period when a customer asks about a last-minute order. |
| Every cake has an eggless version. | Answer questions about eggless options directly. |
| Never promise delivery times. | Avoid confirming a delivery slot the bakery has not checked. |
| Say when an answer is unknown. | Avoid inventing details such as a cake price. |
| Reply politely in under 60 words. | Keep the answer short and easy for a customer to read. |

## ⚙️ How my first API call works

An **API call** is a request from one program to another. Here, my Python notebook sends a question to Groq and receives the model’s reply.

1. **Connect to Groq.** The notebook reads `GROQ_API_KEY` from Colab Secrets and uses `ChatGroq` to select the `openai/gpt-oss-120b` model.
2. **Prepare the messages.** The `ask()` function takes a customer question and pairs it with the bakery’s system prompt.
3. **Send the request.** `llm.invoke()` sends both messages to the model. This is the line that makes the API call.
4. **Display the answer.** `.content` extracts the reply text, and `print()` displays it in the notebook.

The main function is short:

```python
def ask(question):
    return llm.invoke([
        ("system", SYSTEM),
        ("human", question)
    ]).content
```

## 💬 Example conversation

**Customer:** Can I get my order tomorrow?

**Example reply:** Thanks for asking! We bake each cake to order and need at least 24 hours’ notice. What time tomorrow do you need the cake? I can explain the notice requirement, but I can’t confirm a delivery time.

The exact wording can vary. The important part is that the assistant explains the **24-hour rule** and avoids promising delivery.

## ▶️ How to run the project

1. Open `agenticAI_Day1.ipynb` in Google Colab.
2. Create an API key on the [Groq API Keys page](https://console.groq.com/keys).
3. In Colab, click the **Secrets** key icon in the left sidebar.
4. Add a secret named `GROQ_API_KEY`, paste your key as its value, and turn on **Notebook access**. Keep the actual key private.
5. Run the notebook cells from top to bottom.
6. Try your own question in the cell that calls `ask()`:

```python
print(ask("Do you make eggless cakes?"))
```

## 🔍 Try it yourself

| Ask the assistant | What to look for |
|---|---|
| “Do you have eggless chocolate cake?” | Does it use the fact that every cake has an eggless version? |
| “Can you deliver at 8 AM tomorrow?” | Does it explain the notice period without promising a delivery time? |
| “How much is a birthday cake?” | Does it acknowledge that the prompt contains no prices? |

## 🌱 What I learned

I learned how to make an API call, keep an API key in Colab Secrets, and use a system prompt to guide a model. My main takeaway was that writing the code is only part of the task. I also need to test what the assistant says when it has enough information **and** when a customer asks something it cannot answer.

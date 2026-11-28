# 🖨️ Day 2 – Campus Print Shop Chatbot with Memory




> “I need 20 black-and-white pages. What if I make the first page colour?”

A student should be able to ask this question without typing the whole order again. This chatbot remembers the print order, calculates the price, and lets the student preview a change before confirming it.

## What I built

I built a print shop chatbot in Google Colab using Python and the Groq API. A student can check rates, choose colour or black-and-white printing, change the number of copies, and add spiral binding. The chatbot keeps track of the conversation and shows an itemised estimate.

## Shop catalogue

The shop uses sample prices stored in a Python dictionary. Printing is charged per **printed side**, and binding is charged per **copy**.

```python
catalogue = {
    "black and white printing": {"price_inr": 2, "unit": "per printed side"},
    "colour printing": {"price_inr": 10, "unit": "per printed side"},
    "spiral binding": {"price_inr": 30, "unit": "per copy"}
}
```

For example, printing 20 black-and-white pages costs **₹40**. Single-sided printing uses 20 sheets. Double-sided printing uses 10 sheets, and the price stays ₹40 because all 20 sides still need printing.

## How it works

1. **Understand the request.** The `chat()` function sends the student’s message, current order, and recent conversation to Groq. The system prompt asks the model to return a structured JSON response.
2. **Remember the order.** A Python dictionary stores the student’s name, page counts, copies, binding choice, and printing mode. This lets the student say “Make it two copies” without repeating every detail.
3. **Preview or confirm a change.** `apply_updates()` prepares a copy of the order. “What if I add binding?” shows a preview. “Add binding to both copies” updates the saved order.
4. **Calculate the bill.** `calculate_bill()` reads the rates from the catalogue. Python calculates the total price and the number of paper sheets.
5. **Show the estimate.** `order_summary()` displays the order details. If the student asks about pickup, the chatbot asks them to confirm the time with the shop owner.

## Example bill

Suppose a student orders **two single-sided copies** of a 20-page assignment. Each copy has one colour cover page, 19 black-and-white pages, and spiral binding.

| Item | Calculation | Amount |
|---|---:|---:|
| Black-and-white pages per copy | 19 × ₹2 | ₹38 |
| Colour cover per copy | 1 × ₹10 | ₹10 |
| Spiral binding per copy | 1 × ₹30 | ₹30 |
| **One bound copy** | **₹38 + ₹10 + ₹30** | **₹78** |
| **Two bound copies** | **2 × ₹78** | **₹156** |

## How to run the chatbot

1. Open `Campus_Print_Shop_Chatbot.ipynb` in Google Colab.
2. Open **Secrets** using the key icon in the left sidebar.
3. Add your Groq API key under the name `GROQ_API_KEY` and turn on **Notebook access**. Keep the key private.
4. Run the notebook cells from top to bottom.
5. When `You:` appears, type a customer question and press **Enter**.
6. Type `reset` to clear the order and chat history, or `exit` to finish.

## Ten customer questions

Start with `reset`, then ask these questions in order. The last column gives the **expected result** so anyone running the notebook can check the behaviour.

| No. | Customer question | Expected result |
|---|---|---|
| 1 | Hi, I’m Poojitha. What services do you offer? | Lists the three services and remembers the name. |
| 2 | How much is colour printing per side? | Shows the rate of ₹10 per printed side. |
| 3 | I need one single-sided copy of 20 black-and-white pages without binding. How much is it? | ₹40 for 20 sheets. |
| 4 | What if I print it double-sided? | Previews 10 sheets at ₹40. |
| 5 | Can you make the first page colour and keep the other 19 black and white? | Updates the estimate to ₹48. The order is still single-sided. |
| 6 | What is my current total for one copy? | Recalls the updated order and quotes ₹48. |
| 7 | What would one copy cost with spiral binding? | Previews ₹78 for one bound copy. |
| 8 | Please make it two copies and add spiral binding to both. What is my final total? | Updates the order to two bound copies for ₹156. |
| 9 | What is my name, and what did I order? | Recalls Poojitha, the page details, two bound copies, and ₹156. |
| 10 | Can I collect my order at 4 PM today? | Directs the student to the shop owner for pickup confirmation. |

## What I learned

I learned how to combine a language model with Python in a small business application. The model interprets phrases like “make it two copies,” while Python keeps the order details and calculates the bill. Saving previews separately lets a customer explore a price before deciding what to order.

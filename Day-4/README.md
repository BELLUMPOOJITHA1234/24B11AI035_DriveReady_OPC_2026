# 🖨️ Day 4 – Campus Print Shop Agent with Tools

Drive Ready – AI Agents | Aditya University

> “How much does spiral binding cost per copy?”

To answer this question, the agent can call `get_price`, read the shop’s catalogue, and reply with **₹30 per copy**. In this project, I also turned informal customer messages into print order records and saved a sample order in Google Sheets.

## 1. My business tools

| Tool | What it does | Example |
|---|---|---|
| `get_price` | Looks up the unit price of a service | Colour printing costs ₹10 per printed side. |
| `is_service_available` | Checks whether a service is in the catalogue | Spiral binding is available. Lamination is outside the catalogue. |
| `log_print_order` | Adds one print order to the `Orders` tab in Google Sheets | Each field goes into its own column. |

I gave each tool a short docstring explaining when to use it. For the price-question experiment, the agent had access to the first two tools. I tested the logging tool separately by saving a sample order.

## 2. Turning a customer message into an order

Customers often type requests like this:

> “poojitha here.. 20 pages b&w, 2 copies pls, spiral bind both”

I defined a Pydantic class called `PrintOrder` and used `with_structured_output(PrintOrder)` to extract five fields from each message.

| Customer | Pages per copy | Copies | Print type | Spiral binding |
|---|---:|---:|---|---|
| Poojitha | 20 | 2 | black and white | Yes |

I used five informal messages to test abbreviations, mixed printing, and missing details. For example, “no binding pls” means `false`. If the customer never mentions binding, the field stays `null`. This distinction helps the shop know when to ask a follow-up question.

## 3. Saving an order in Google Sheets

I created a spreadsheet called **Campus Print Orders** with a worksheet named **Orders**. The `log_print_order` tool saves one row with these columns:

`customer_name` · `pages_per_copy` · `copies` · `print_type` · `spiral_binding`

After saving the sample record from Colab, I checked the row in Google Sheets. Missing fields are written as empty cells.

## 4. Testing the tool name and reading the trace

I asked the same question in two fresh chats: **“How much does spiral binding cost per copy?”**

- In the first chat, the price tool was named `get_price` and had a clear description.
- In the second chat, the same lookup was named `tool1` with the description `does stuff`.
- I used `pretty_print()` to see which tool the agent actually called. I then returned to the original agent to count its model calls and tool calls.

### My experiment results

- Original tool selected: [Copy the tool name from Step 8, or write “No tool called”.]
- Tool selected after renaming: [Copy the tool name from Step 9, or write “No tool called”.]
- Original trace: [Enter the model call count and tool call count from Step 10.]

## 5. Business facts for Day 5

`campus_print_shop_facts.txt` contains 18 facts about prices, paper usage, binding, order details, and pickup confirmation. For example, a 21-page document printed double-sided needs **11 sheets per copy**. These facts will be used for the Day 5 retrieval task.

## Project files

| File | Purpose |
|---|---|
| `Campus_Print_Shop_Tools_Day4.ipynb` | Colab notebook with the tools, five sample messages, Google Sheets logging, and agent traces |
| `campus_print_shop_facts.txt` | Searchable facts about the print shop |

## How to run

Open the notebook in Google Colab and enable `GROQ_API_KEY` in Colab Secrets. For the logging exercise, connect a Google Sheet with the five column headings above and an `Orders` tab. Run the cells in order. Run the sample logging cell once, because each run appends a new row.

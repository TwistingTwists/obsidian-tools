2025-12-18 


## facts - ram vs hard disk 

ram 
- 

hdd 
- 

## playbook - do it asap - volunteer
- 
  

## broad level idea  - boxes and circles

## Questions: 

- is the format Day book - fixed? always same? 
- how to build a daily book? 
- What is a day book? 
- What is common sense?
	- What is "ledge of legal expense" - if a client asks for a "Ledger of Legal Expenses," they aren't just looking for a list of costs—they want to know **what was billed, what was paid, and what is still owed.**
		- common mistake - An AI should "know" that to give a complete picture of an expense, it must cross-reference the Vendor ledger. Simply listing debits without corresponding credits (payments) makes the ledger useless for reconciliation.

----

5 kaam , procedures - 1.2.3.4.5 


----

11 (capital account)

Journal        Payment
111                100 

9 - P1          1 - P3
2 - P2          1 - P3
		   1 - P3
3 - P1          2 - P3 
8 - P3


33 - P4
56 - P5


Hard part is Breaking down P2 into P2A, P2B:
- natural language - P2 has two types of relationship - 1. legal and 2. chai 
- P2 = 10-


Simple Algorithm for long running balance: 
1. assumption: P1, P2 -> have a single relation with me 
2. reconcile ledgers of  individual parties (P1, P2 ... ) => excess (subset sum match with parties having non zero ledger balances)

Complex Algorithm: (can be simplifed to above, if P2 becomes P2A , P2B)
1. assumption: P1A, P1B, P2A, P2B -> have two relations with me 
2. P2 = reconciled and ZERO (because P2A = +2 , P2B = -2 )

------
#### parties reconclied 

P1 - reconciled? - ( + / - / 0)
P2 - reconciled? - ( + / - / 0)
P3 - reconciled?  - ( + / - / 0)
P4 - reconciled?  - ( + / - / 0)




RECONCILIATION = tracked all the JOURNAL + PAYMENT entries for P1
- net journal - payment = postivie or negative

#### one more case -- chai + legal with same party


-----


## Long running balance

- balance = Journal - paymentt

| Example:<br>      |
| ----------------- |
| **CIRP Bank A/c** |
| **Capital A/c**   |
Let's say today : 
- Today closing balance, capital account = 13,000 but the 3045 is never offset 
	- What is the break up of this 13,000 = 3045 + ... 
- Which are the long running balances in last 3 years - traditional software fails. 
	- 1-1 matching solves this
	- subset sum matching 


## Balance sheet as on 31-Mar-2022

tags: simple

- Steps to make balance sheet

## Ledger of a legal expenses 

tags: intermediate-complexity

legal expense = (this is a type of expense = may have set of parties)

OTHER EXAMPLES: 
- ledge of bank account 
- ledger of party 


- expenses =  Journal
- Smart Reconciliation = matching journal <> payments
- keyword legal
	- Hint (show hint to user - should we include?) - anthing similar to legal
- Match relevant Journal < - > Payment
	- Journal -> Intent 
	- Payment -> Execution


journal (WHY of payment) -> legal 
payment (vendor name only, WHY of payment is not there) -> chai 


**Filtering**: 
1. Find all legal entries (journal)
2. Find the aadmi under that legal entry in 1. 
	1. Sreenita GhoshDastidar( S G Law Office)
3. Use that aadmi name -> find payments 
4. Then apply matching algo 


**MATCHING ALGO:**
Scene 2: match aggregate (subset sum WITH PENDING AMOUNT)
> payment = journal  (1-1 match) - match by EXACT bill amount 
> payment < journal  (subset sum match  with pending amount)
> payment > journal 


**more problems:**
- journal (with WHY of payment) + payment (w/o WHY of payment) -> for chai to sreenitha
> 	solvable if payment = journal (1-1) match
- journal entries for Legal to the same aadmi should not include journal entries for ANY OTHER PURPOSE THAN LEGAL
	- same goes for payment





OUTPUT 


payment = 4545
journal = 445 
1-1 = 5 
everything else - 4 

----


### vishal logs 
I asked 2 questions.

1. Ledger of a legal expenses. At first it gave me only expense details without the payment. This is because payments are not directly related to an expense but to a vendor. So that function is not available in Tally either. But ChatGPT did a poor job of preparing the ledger. And it also did not link the payments. Basically, the "common sense" was missing

2. Balance sheet as on 31-Mar-2022. Here ChatGPT failed miserably. The bank balance as on date was Rs. 22 Cr, and there were other assets as well. The total assets calculated by ChatGPT was only Rs. 8 Cr

But more importanly, the figures given by ChatGPT were not auditable. The only way to audit them was to have a conversation with the model, which is not conduicive for audit. For audit I need to go back to my original question several times and need to go back and forth between questions. It becomes clumsy in a long conversation format.

-----


## possbile prompts for common sense
``

Here are two versions of the System Instruction/Prompt designed to fix the specific accounting logic failures you encountered.

### Option 1: The Long-Form "Conceptual & Procedural" Prompt
*Use this when setting up a new chat context or defining the AI's persona. It teaches the AI the "Why" behind the rules so it can generalize better.*

***

**System Role: Senior Statutory Auditor & Database Query Engine**

**Objective:** You are not a creative writer; you are a strict accounting engine. You must operate under the assumption that accounting is **Algebra, not Literature**. Your outputs must be auditable, traceable, and adhere to Double-Entry Bookkeeping mechanics. You will strictly follow the Conceptual Framework and Procedural Rules below.

**Part A: Conceptual Framework (The "Common Sense")**
1.  **The Vendor Bridge Concept:** Understand that an *Expense Ledger* (e.g., Legal Professional Fees) only records the accrual (the Bill). It rarely records the movement of money. To find a **Payment**, you must identify the *Vendor* (Party) associated with that expense and trace the payment in the *Vendor’s Ledger* or the *Bank Book*.
2.  **The Completeness Principle:** A Balance Sheet is a mathematical certainty derived from the *entire* Trial Balance. You cannot calculate "Total Assets" based only on the assets mentioned in the immediate conversation. If you lack the closing balance of the Bank Account, Cash, or Receivables, you simply cannot produce a Balance Sheet. A partial Balance Sheet is a wrong Balance Sheet.
3.  **Auditability Over Fluency:** In this context, a "smooth" answer is worthless if it is not precise. Every figure you present must be static and traceable. If you cannot cite a specific Voucher Number, Date, or Source File for a number, do not calculate it.

**Part B: Procedural Rules (The "How-To")**

**Rule 1: Handling Expense & Payment Queries**
*   **IF** the user asks for "Payments for [Expense Name]" (e.g., Legal Fees), **DO NOT** just search the [Expense Name] ledger.
*   **INSTEAD:**
    1.  Scan the Expense Ledger to identify the Credit Party (The Vendor/Lawyer).
    2.  Retrieve the Ledger for that Vendor.
    3.  Filter the Vendor’s Ledger for entries where the counterpart is "Bank" or "Cash."
    4.  Present these specific entries as the payments.

**Rule 2: Generating Financial Statements (Balance Sheet)**
*   **BEFORE** outputting a Balance Sheet, perform a **sanity check**:
    *   Do I have the closing balance for the Bank Accounts?
    *   Do I have the Fixed Asset register?
    *   Do I have the complete Liabilities list?
*   **IF** data is missing (e.g., you see Furniture but no Bank Balance), **STOP**. Tell the user: *"I cannot generate a valid Balance Sheet. I have data for Fixed Assets, but I am missing the Cash/Bank balances. Please provide the full Trial Balance."*
*   **NEVER** hallucinate or estimate a "Total Asset" figure based on partial data.

**Rule 3: The Audit Trail Format**
*   All numerical answers must follow this format: `[Figure] | [Source/Voucher #] | [Date]`.
*   Do not summarize data without offering a drill-down breakdown of how that sum was reached.

***

### Option 2: The Concise "Action Item" Prompt
*Use this for "Custom Instructions" or a sticky system prompt where token limits matter. It focuses on constraints and negative prompts.*

***

**System Instruction: Accounting & Audit Mode**

**Strict Logical Constraints:**
1.  **NO Semantic Search for Payments:** If asked for "Expense Payments," do not search the Expense Ledger. **ACTION:** Extract the Vendor Name from the expense entries, then Query the Vendor Ledger for Bank/Cash entries.
2.  **NO Partial Balance Sheets:** You are prohibited from summing "Total Assets" or "Liabilities" unless you have access to the complete Trial Balance (specifically Bank/Cash closing balances). **ACTION:** If Bank Balance is missing, flag it as a "Data Gap" immediately. Do not guess.
3.  **Traceability Requirement:** Every financial figure output must include a citation (Voucher Number/Date).
4.  **Accrual Awareness:** Distinguish between "Billed Amount" (Expense Ledger) and "Paid Amount" (Vendor Ledger). Never conflate the two.

**Execution Protocol:**
*   **User:** "Show ledger of Legal Expenses."
    *   **AI Action:** Show the Bills (Debits). *Auto-append note:* "Note: To see payments, request the specific Lawyer's ledger."
*   **User:** "Balance Sheet as of 31-Mar."
    *   **AI Action:** Check for `Bank_Balance_Closing`. If `null`, return Error: "Missing Bank Data." If present, calculate `Assets = Liabilities + Equity`.

**Tone:** Detached, precise, auditable. No conversational filler.

***




**Compliance**: 
- 

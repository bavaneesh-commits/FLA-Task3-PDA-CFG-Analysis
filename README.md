# Pushdown Automata & CFG Simulation - CLA 2 (Task 3)

## 📌 What We Implemented in This Project

In this project, we built a complete Python-based simulation engine in Google Colab to model and verify **Context-Free Grammars (CFG)** and **Pushdown Automata (PDA)** across three computational problems:

1. **Custom PDA Engine (`PushdownAutomaton` Class):**
   - Implemented a general stack-based state machine that simulates Instantaneous Descriptions $(q, w, \alpha) \vdash (p, w', \beta)$.
   - Supports `PUSH`, `POP`, and `NOOP` stack operations with acceptance by final state.

2. **Application 1: Balanced Parentheses Delimiter Checker:**
   - Implemented CFG: `S -> (S) | SS | ε`.
   - Built a DPDA that pushes `X` on `(`, pops `X` on `)`, and accepts when stack exposes $Z_0$ on empty input.
   - Traced step-by-step stack movements for nested strings like `(())`.

3. **Application 2: Arithmetic Expression Syntax Validator & Ambiguity Resolution:**
   - Analyzed why the initial grammar `E -> E + E | E * E | (E) | id` was ambiguous for `id + id * id`.
   - Implemented a recursive-descent parser based on the revised unambiguous grammar (`E -> E + T | T`, `T -> T * F | F`, `F -> (E) | id`) to enforce operator precedence ($* > +$) and left-associativity.

4. **Application 3: Palindrome Checker ($w c w^R$):**
   - Implemented CFG: `S -> aSa | bSb | c`.
   - Built a DPDA that pushes symbols of $w$, switches state at center marker `c`, and pops/matches symbols of $w^R$.

5. **Automated Visual Diagram Generation:**
   - Used `graphviz` to automatically render state transition diagrams for the PDAs and the unambiguous expression parse tree.

---

## 🧪 Test Results & Verification

We executed **10 test cases per application** (5 Accepted + 5 Rejected), verifying both valid structural flows and invalid edge cases:

* **Balanced Parentheses:** 10 / 10 Passed
* **Arithmetic Expression Validator:** 10 / 10 Passed
* **Palindrome Checker:** 10 / 10 Passed
* **Overall Test Suite Status:** **30 / 30 Passed (100% PASS)**

---

## 💡 Key Takeaways

* **Why Finite Automata Fail:** Regular languages cannot count unbounded nested brackets or verify string reversals due to the Pumping Lemma for Regular Languages. An auxiliary LIFO stack (PDA) is strictly required.
* **Ambiguity Resolution:** Splitting grammar into precedence tiers ($E, T, F$) eliminates multiple parse trees, ensuring deterministic parsing for compilers.

---

## 🚀 How to Run the Code

1. Open Google Colab: [colab.research.google.com](https://colab.research.google.com)
2. Upload and open `FLA_CLA2_Task3_PDA_CFG.ipynb`.
3. Click **Runtime -> Run all** (`Ctrl + F9`).
4. All 30 test cases will execute with full step-by-step stack traces and rendered diagrams.

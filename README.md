# Case Study-Based Analysis and Design using Pushdown Automata (PDA) and Context-Free Grammar (CFG)

**SRM Institute of Science and Technology, Tiruchirappalli**  
**Department:** Artificial Intelligence and Machine Learning (AIML)  
**Programme:** B.Tech. AIML | **Semester:** V | **Academic Year:** 2026–2027  
**Course Code & Title:** 21CSC301T – Formal Language and Automata  
**Assessment Task:** Task 3 – Case Study-Based Analysis and Design using PDA and CFG  
**Faculty Handling:** Dr. C. Gunasundari (AP)  
**SDG Alignment:** SDG 9 – Industry, Innovation, and Infrastructure (Level 2: Design Alignment)  

---

## 📌 Project Overview
This repository contains formal language models, mathematical derivations, and a complete programmatic simulation engine developed in Python (Google Colab) for **Assessment Task 3**. 

The study models and analyzes three distinct computational case studies using **Context-Free Grammars (CFG)** and **Pushdown Automata (PDA)**:
1. **Balanced Parentheses Delimiter Checker** (Syntax nesting validation)
2. **Arithmetic Expression Syntax Validation & Ambiguity Resolution** (Precedence & associativity enforcement)
3. **Palindrome Checker** ($L = \{w c w^R \mid w \in \{a,b\}^*\}$) (LIFO stack push/pop synchronization)

---

## 📂 Repository Contents
* `FLA_CLA2_Task3_PDA_CFG.ipynb`: Complete Google Colab notebook featuring the custom object-oriented Pushdown Automaton engine, recursive-descent CFG parser, Graphviz transition/parse tree visualizations, and master test runner.
* `README.md`: Formal theoretical documentation, case study specifications, and model analyses.

---

## 🔬 Case Study 1: Balanced Parentheses Delimiter Checker

### 1. Formal Language Definition
$$L = \{w \in \{(, ) \}^* \mid \text{parentheses in } w \text{ are properly nested and balanced}\}$$

### 2. Context-Free Grammar (CFG)
* **Variables ($V$):** $\{S\}$
* **Terminals ($\Sigma$):** $\{(, )\}$
* **Start Symbol:** $S$
* **Production Rules ($R$):**
  $$S \to (S) \mid SS \mid \epsilon$$

### 3. Pushdown Automaton (7-Tuple Model)
$$M = (Q, \Sigma, \Gamma, \delta, q_0, Z_0, F)$$
* **States ($Q$):** $\{q_0, q_{acc}, q_{trap}\}$
* **Input Alphabet ($\Sigma$):** $\{(, )\}$
* **Stack Alphabet ($\Gamma$):** $\{Z_0, X\}$
* **Start State:** $q_0$
* **Initial Stack Symbol:** $Z_0$
* **Accepting State ($F$):** $\{q_{acc}\}$ (Acceptance by Final State)
* **Transition Function ($\delta$):**
  * $\delta(q_0, (, Z_0) = (q_0, XZ_0)$ *(Push $X$)*
  * $\delta(q_0, (, X) = (q_0, XX)$ *(Push $X$)*
  * $\delta(q_0, ), X) = (q_0, \epsilon)$ *(Pop $X$)*
  * $\delta(q_0, \epsilon, Z_0) = (q_{acc}, Z_0)$ *(Accept)*

### 4. Sample Instantaneous Description (ID) Stack Trace
For input string `(())`:
$$\begin{aligned}
(q_0, (()), Z_0) &\vdash (q_0, ()), XZ_0) \\
&\vdash (q_0, )), XXZ_0) \\
&\vdash (q_0, ), XZ_0) \\
&\vdash (q_0, \epsilon, Z_0) \\
&\vdash (q_{acc}, \epsilon, Z_0) \quad \text{[ACCEPTED]}
\end{aligned}$$

---

## 🔬 Case Study 2: Arithmetic Expression Syntax & Ambiguity Resolution

### 1. Target Language
Valid mathematical infix expressions over operands (`id`), binary operators (`+`, `*`), and balanced parentheses (`(`, `)`).

### 2. Ambiguity Analysis of Initial CFG
* **Initial Grammar:**
  $$E \to E + E \mid E * E \mid (E) \mid id$$
* **Proof of Ambiguity:** The string `id + id * id` produces two distinct parse trees and two leftmost derivations:
  * **Tree 1 (Incorrect Precedence):** Evaluates $+$ first $\implies (id + id) * id$
  * **Tree 2 (Correct Precedence):** Evaluates $*$ first $\implies id + (id * id)$

### 3. Revised Unambiguous Grammar
To enforce standard operator precedence ($* > +$) and left-associativity:
$$\begin{aligned}
E &\to E + T \mid T \\
T &\to T * F \mid F \\
F &\to (E) \mid id
\end{aligned}$$
* **Result:** Guaranteed unique derivation tree and deterministic syntax analysis for compiler front-ends.

---

## 🔬 Case Study 3: Palindrome Checker ($L = \{w c w^R\}$)

### 1. Formal Language Definition
$$L = \{w c w^R \mid w \in \{a, b\}^*, c \text{ is the pivot center marker}, w^R = \text{reversal of } w\}$$

### 2. Context-Free Grammar (CFG)
$$S \to a S a \mid b S b \mid c$$

### 3. Pushdown Automaton (7-Tuple Model)
* **States ($Q$):** $\{q_0, q_1, q_{acc}, q_{trap}\}$
* **Input Alphabet ($\Sigma$):** $\{a, b, c\}$
* **Stack Alphabet ($\Gamma$):** $\{Z_0, A, B\}$
* **Mechanism:**
  * **State $q_0$:** Pushes input symbols of $w$ onto stack ($a \to A, b \to B$).
  * **Pivot $c$:** Transitions from $q_0 \to q_1$ without altering the stack.
  * **State $q_1$:** Pops and matches symbols of $w^R$ against the top of stack.
  * **State $q_{acc}$:** Accepts when all input is consumed and stack contains $Z_0$.

---

## 📊 Comprehensive Test Suite & Validation Summary

Each case study was verified against **10 comprehensive test strings** (5 Accepted, 5 Rejected), validating valid structural flows and edge-case rejections:

| Case Study | Accepted Tests (5) | Rejected Tests (5) | Pass Rate |
| :--- | :--- | :--- | :---: |
| **1. Balanced Parentheses** | `()`, `(())`, `()()`, `((()()))`, `ε` | `(`, `)`, `())(`, `((())`, `())` | **10 / 10 (100%)** |
| **2. Arithmetic Expression** | `id`, `id+id`, `id*id+id`, `(id+id)*id`, `id*(id+id*id)` | `id+`, `+id`, `id*+id`, `(id+id`, `id id` | **10 / 10 (100%)** |
| **3. Palindrome ($wcw^R$)** | `c`, `aca`, `bcb`, `abbcbba`, `baacaab` | `acb`, `abcca`, `abbcbb`, `abc`, `aabaa` | **10 / 10 (100%)** |
| **Master Test Suite** | **15 Strings Accepted** | **15 Strings Rejected** | **30 / 30 (100%)** |

---

## 💡 Theoretical Discussion

### 1. Why Finite Automata (FA) are Insufficient
* **Constant Memory Limitation:** Finite Automata operate with fixed states ($|Q| < \infty$) and cannot store unbounded counts.
* **Pumping Lemma Contradiction:** For $L = \{(^n )^n \mid n \ge 0\}$, assuming regularity with pumping length $p$, string $s = (^p )^p$. By the Pumping Lemma ($s = xyz, |xy| \le p, |y| \ge 1$), $y$ must consist solely of `(`. Pumping $xy^2 z$ increases `(` without increasing `)`, violating balance. Thus, an auxiliary LIFO stack (PDA) is strictly necessary.

### 2. Determinism vs. Non-Determinism (DPDA vs. NPDA)
* **Deterministic PDA (DPDA):** The languages for balanced parentheses and marked palindromes ($w c w^R$) are deterministic context-free languages because the transition function $\delta$ has at most one valid move at each step.
* **Non-Deterministic PDA (NPDA):** Unmarked even-length palindromes ($L = \{w w^R\}$) require non-determinism to guess the midpoint without a physical marker.

---

## 💻 How to Run the Google Colab Notebook

1. Open [Google Colab](https://colab.research.google.com).
2. Go to **File -> Open notebook -> GitHub**.
3. Paste your repository link and open `FLA_CLA2_Task3_PDA_CFG.ipynb`.
4. Click **Runtime -> Run all** (`Ctrl + F9`) to view live stack transitions, parse trees, and test suite execution.

---
**Author:** [Your Name]  
**Register Number:** [Your Register Number]  
**Department:** Artificial Intelligence and Machine Learning (AIML)  
**Institution:** SRM Institute of Science and Technology, Tiruchirappalli

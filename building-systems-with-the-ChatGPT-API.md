# DeepLearning.ai - Building Systems with the ChatGPT API
(https://learn.deeplearning.ai/chatgpt-building-system)

- Supervised learning (X input -> Y result output), get labeled data, then train AI model on data, then deploy and call the model.
- Two types of large language model (LLMs)
  - Base LLM (predicts next word, based on training data, “once upon a time …”)
  - Instruction Tuned LLM (tries to follow instructions, “What is a capital city of France? Paris.”)
- How to get from Base LLM to Instruction Tuned LLM?
  1. Train Base LLM with a large set of data.
  2. Fine-tune on examples of where the output follows an input instruction.
  3. Human-rating quality of different LLM outputs
  4. Tune LLM to increase probability that generates the more highly related outputs using RLHF, reinforcement learning from human feedback.
- Dělení slov na tokeny, běžná slova jako samostatné tokeny, méně běžná slova dělená na samostatné tokeny (prompting = prom / pt / ing), u běžných slov je hádání dalšího slova jednoduché, u méně běžných mnohem náročnější a vznikají chyby. Např. “Take the letters in lollipop and reverse them” - vždy to vrátí špatně, protože tokenizátor to vidí jako l/oll/ipop, 3 samostatné tokeny a neumí s tím pracovat. Pokud mu pošlu to slovo jako l-o-l-l-i-p-o-p, kdy každé písmeno bere jako jeden token, tak to udělá správně.
- For English input, 1 token is around 4 characters, or ¾ of the word.
- Token limits, for example gpt3.5-turbo has limit 4000 tokens for input (tzv. context) and output completion.
- OpenAI format chat: system (set the tone / behavior of assistant), user (what user want), assistant (LLM response, např. relevant product info).
- Klasifikace. Pošlu ChatGPT instrukce (uživatel zadává …) a napíšu mu možné kategorie (a případně podkategorie) a AI rozhodne, která z kategorií má být přiřazena.
- OpenAI moderation. Free API, které umožňuje rozhodnout, zda není text v kategorii hate, sexual, violence etc.
- Prompt injection. Uživatel může vložit instrukce typu “ignoruj předchozí instrukce a místo toho udělej …”. Možné ošetření přes omezení počtu vrácených tokenů, případně přes další dotaz na API, kde zjistím, zda uživatel chce změnit dotaz. Zároveň používání oddělovačů a ošetření, že uživatel nevložil tyhle oddělovače do své zprávy.
- Chaining prompts, i když je GPT 4 výborný v pochopení instrukcí, raději je rozdělit do malých dotazů (more focused, context limitation, reduced costs).
- Ukázka implementace chatbotu, kde uživatel může zadávat dotazy na produkty a systém mu odpoví:
  - 1) Kontrola, zda nejde o prompt injection a volání moderation API.
  - 2) Volání API a zjištění o jakém produktu a kategorii se mluví.
  - 3) Volání API s konkrétním dotazem uživatele.
  - 4) Ověřím, že odpověď uživateli je v pořádku opětovnou kontrolou moderation API.
  - 5) Volání API a ověření, že je odpověď relevantní (ANO/NE).
  - 6) Vrátím uživateli odpověď (v případě ANO, jinak se omluvím).
- V příkladu se vracel non-JSON text, Andrew přidal do příkazu “Don’t output any additional text that is not in JSON.
- Evaluation: Projdu si různé příklady, vyberu si příklady, které skončily chybně a ty vložím do mého setu pro ověření. Set bude mít dvojicií: 1. user input, 2. ideal output.
- U složitějších příkladů pošlu pro ověření seznam možných výstupů, Andrew Ng má:
  - Compare the factual content of the submitted answer with the expert answer. Ignore any differences in style, grammar, or punctuation.
    - The submitted answer may either be a subset or superset of the expert answer, or it may conflict with it. Determine which case applies. Answer the question by selecting one of the following options:
    - (A) The submitted answer is a subset of the expert answer and is fully consistent with it.
    - (B) The submitted answer is a superset of the expert answer and is fully consistent with it.
    - (C) The submitted answer contains all the same details as the expert answer.
    - (D) There is a disagreement between the submitted answer and the expert answer.
    - (E) The answers differ, but these differences don't matter from the perspective of factuality.
  - choice_strings: ABCDE

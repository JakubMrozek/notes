# DeepLearning.ai - ChatGPT Prompt Engineering for Developers
(https://learn.deeplearning.ai/chatgpt-prompt-eng/) 


- Clear != short, write longer, clear and specific instructions.
- Používání oddělovačů: ###, ``` etc.
- Specify the output: “Provide JSON format with following keys: book_id, title, author, genre.”
- Specify conditions: “If the text contains, than do this, otherwise do that.”
- V promptu používat příklad, jak si představuji výstup.
- Specify the steps to complete a task (step 1: XXX, step 2: XXX etc.).
- Když ověřuji nějaké řešení, požádat nejprve model, aby úlohu spočítal sám a poté porovnal předkládané řešení, takhle s vyšší pravděpodobností odhalí chybu.
- Halucinace. Občas model vyhodí vyplodí nějaký realistický text o věci, která neexistuje. Řešením upravit prompt a nejprve požádat gpt o ověření, že ten produkt zná.
- Parametr *temperature* <1,0> vyjadřuje míru kreativity odpovědi. Pokud mám hodnotu 1.0, výsledky budou mít velkou míru kreativity a do výsledku se zařadí i hodnoty, u nichž je výskyt velmi nízký. Naopak hodnota 0 říká, že chci jen nejpravděpodobnější výsledky, ty budou také více konzistentní.

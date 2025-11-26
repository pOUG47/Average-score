# Середній бал
Користувач вводить 5 оцінок (програма запитує кожну оцінку окремо в циклі). Після введення всіх оцінок програма має розрахувати та вивести середній бал. 

    import tkinter as tk

    from statistics import mean

#                       Введення



    def validate_input(event, index):
    value = entries[index].get()      # Беремо текст з поля
    if value == "":
        error_labels[index].config(text="")  # Якщо пусто — помилки нема
        return
    
    #      Перевірка: чи введене значення — тільки цифри 
    if not value.isdigit():
        error_labels[index].config(text="Помилка!", fg="red")  # Пише помилку праворуч
        return

    value = int(value)

    #      ЛІМІТ оцінки: не більше 12 
    if value > 12:
        error_labels[index].config(text="Перевищення!", fg="red")
    else:
        error_labels[index].config(text="")    # Якщо все норм — чистимо помилку

#                      Перехід між полями стрілками 

    def focus_next(event, index):
        if index < len(entries) - 1:
            entries[index + 1].focus()    # Стрілка вниз → наступне поле

    def focus_prev(event, index):
        if index > 0:
            entries[index - 1].focus()    # Стрілка вгору → попереднє поле

#                       Обчислення середнього
    def calculate():
        try:
            nums = [int(e.get()) for e in entries]   # Збираємо усі значення як числа

        #      Якщо є хоч одна помилка праворуч — не рахуємо
        
        if any(lbl.cget("text") != "" for lbl in error_labels):
            result_label.config(text="Виправте помилки!", fg="red")
            return
        
        #      Обчислення середнього 
        
        avg = mean(nums)
        result_label.config(text=f"Середнє арифметичне: {avg}", fg="black")

    except ValueError:
        result_label.config(text="Заповніть всі поля!", fg="red")

#                               Головне вікно
    window = tk.Tk()
    window.title("Середнє арифметичне")
    window.geometry("400x400")

    # Заголовок
    title = tk.Label(window, text="Введіть 5 оцінок (0–12)", font=("Times New Roman", 16))
    title.pack(pady=10)

    # Списки для полів вводу та помилок
    entries = []
    error_labels = []

    # Блок для рядків
    frame = tk.Frame(window)
    frame.pack()
 

#                            Поля для вводу

    for i in range(5):
        row = tk.Frame(frame)     # Стрічка (поле + текст помилки)
        row.pack(pady=3)

    entry = tk.Entry(row, font=("Times New Roman", 14), justify="center", width=10)
    entry.pack(side="left")

    # Місце, де показується помилка напроти поля
    error_label = tk.Label(row, text="", font=("Times New Roman", 12), fg="red")
    error_label.pack(side="left", padx=10)

    #      Привʼязуємо валідацію 
    entry.bind("<KeyRelease>", lambda e, idx=i: validate_input(e, idx))

    #      Привʼязуємо перехід стрілками 
    entry.bind("<Down>", lambda e, idx=i: focus_next(e, idx))
    entry.bind("<Up>", lambda e, idx=i: focus_prev(e, idx))

    entries.append(entry)
    error_labels.append(error_label)

    # Кнопка обчислення

    button = tk.Button(window, text="Обчислити", font=("Times New Roman", 14), command=calculate)
    button.pack(pady=15)

    # Результат середнього

    result_label = tk.Label(window, text="Середнє арифметичне: ...", font=("Times New Roman", 14))
    result_label.pack(pady=10)

    window.mainloop()


import tkinter as tk
from tkinter import ttk, messagebox
import random
import json
import os

class RandomTaskGenerator:
    def __init__(self, root):
        self.root = root
        self.root.title("Random Task Generator")
        self.root.geometry("600x500")

        # Типы задач
        self.task_types = ["Учёба", "Спорт", "Работа", "Другое"]

        # Загрузка задач и истории
        self.tasks = self.load_tasks()
        self.history = []  # Хранит только текст для отображения
        self.setup_ui()

    def load_tasks(self):
        """Загрузка задач из JSON-файла"""
        if os.path.exists("tasks.json"):
            with open("tasks.json", "r", encoding="utf-8") as f:
                data = json.load(f)
                return data.get("tasks", [])
        else:
            # Предопределённые задачи
            default_tasks = [
                {"task": "Прочитать статью", "type": "Учёба"},
                {"task": "Сделать зарядку", "type": "Спорт"},
                {"task": "Написать отчёт", "type": "Работа"},
                {"task": "Позвонить другу", "type": "Другое"}
            ]
            self.save_tasks(default_tasks)
            return default_tasks

    def save_tasks(self, tasks):
        """Сохранение задач в JSON-файл"""
        with open("tasks.json", "w", encoding="utf-8") as f:
            json.dump({"tasks": tasks}, f, ensure_ascii=False, indent=4)

    def setup_ui(self):
        """Создание интерфейса"""
        # Верхняя часть — кнопки и фильтрация
        top_frame = ttk.Frame(self.root)
        top_frame.pack(pady=10, fill="x")

        ttk.Button(top_frame, text="Сгенерировать задачу", command=self.generate_task).pack(side="left", padx=5)

        ttk.Label(top_frame, text="Фильтр по типу:").pack(side="left", padx=10)
        self.filter_var = tk.StringVar(value="Все")
        filter_combo = ttk.Combobox(top_frame, textvariable=self.filter_var, values=["Все"] + self.task_types, state="readonly")
        filter_combo.pack(side="left", padx=5)
        filter_combo.bind("<<ComboboxSelected>>", self.apply_filter)

        # Поле для добавления новых задач
        add_frame = ttk.Frame(self.root)
        add_frame.pack(pady=5, fill="x")

        ttk.Label(add_frame, text="Новая задача:").pack(side="left")
        self.new_task_entry = ttk.Entry(add_frame, width=30)
        self.new_task_entry.pack(side="left", padx=5)

        ttk.Label(add_frame, text="Тип:").pack(side="left")
        self.new_type_var = tk.StringVar(value="Другое")
        type_combo = ttk.Combobox(add_frame, textvariable=self.new_type_var, values=self.task_types, state="readonly")
        type_combo.pack(side="left", padx=5)

        ttk.Button(add_frame, text="Добавить задачу", command=self.add_task).pack(side="left", padx=5)

        # Отображение текущей задачи
        self.current_task_label = ttk.Label(self.root, text="Нажмите «Сгенерировать задачу»", font=("Arial", 12), wraplength=550)
        self.current_task_label.pack(pady=10)

        # История задач
        ttk.Label(self.root, text="История задач:").pack(anchor="w", padx=10)
        self.history_listbox = tk.Listbox(self.root, height=15)
        self.history_listbox.pack(fill="both", expand=True, padx=10, pady=5)

        # Кнопка очистки истории
        ttk.Button(self.root, text="Очистить историю", command=self.clear_history).pack(pady=5)

    def generate_task(self):
        """Генерация случайной задачи"""
        filtered_tasks = self.get_filtered_tasks()
        if not filtered_tasks:
            messagebox.showwarning("Предупреждение", "Нет задач для генерации!")
            return

        task = random.choice(filtered_tasks)
        task_text = f"{task['task']} ({task['type']})"
        self.current_task_label.config(text=task_text)
        self.history.append(task_text)  # Добавляем в историю
        self.update_history_display()  # Обновляем отображение

    def get_filtered_tasks(self):
        """Получение задач с учётом фильтра"""
        selected_type = self.filter_var.get()
        if selected_type == "Все":
            return self.tasks
        else:
            return [task for task in self.tasks if task["type"] == selected_type]

    def apply_filter(self, event=None):
        """Применение фильтра при выборе типа"""
        self.update_history_display()

    def add_task(self):
        """Добавление новой задачи"""
        task_text = self.new_task_entry.get().strip()
        task_type = self.new_type_var.get()

        if not task_text:
            messagebox.showerror("Ошибка", "Задача не может быть пустой!")
            return

        new_task = {"task": task_text, "type": task_type}
        self.tasks.append(new_task)
        self.save_tasks(self.tasks)
        self.new_task_entry.delete(0, tk.END)
        messagebox.showinfo("Успех", "Задача добавлена!")

    def update_history_display(self):
        """Обновление отображения истории с учётом фильтра"""
        self.history_listbox.delete(0, tk.END)

        # Получаем текущий фильтр
        selected_type = self.filter_var.get()

        if selected_type == "Все":
            # Показываем всю историю
            for task in self.history:
                self.history_listbox.insert(tk.END, task)
        else:
            # Фильтруем историю по выбранному типу
            for task in self.history:
                # Проверяем, содержит ли запись нужный тип
                if f"({selected_type})" in task:
                    self.history_listbox.insert(tk.END, task)

    def clear_history(self):
        """Очистка истории задач"""
        self.history.clear()
        self.update_history_display()

# Запуск приложения
if __name__ == "__main__":
    root = tk.Tk()
    app = RandomTaskGenerator(root)
    root.mainloop()

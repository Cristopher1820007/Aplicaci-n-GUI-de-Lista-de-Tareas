# Aplicaci-n-GUI-de-Lista-de-Tareas
Desarrollar una aplicación GUI simple para gestionar una lista de tareas, permitiendo al usuario añadir nuevas tareas, marcarlas como completadas y eliminarlas. La aplicación deberá responder adecuadamente a los eventos del usuario, como clics del ratón y pulsaciones del teclado
import tkinter as tk
from tkinter import messagebox

class TodoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Gestor de Tareas")

        # Campo de entrada
        self.entry = tk.Entry(root, width=40)
        self.entry.pack(pady=10)
        self.entry.bind("<Return>", self.add_task)  # Añadir tarea con Enter

        # Botones
        btn_frame = tk.Frame(root)
        btn_frame.pack(pady=5)

        self.add_btn = tk.Button(btn_frame, text="Añadir Tarea", command=self.add_task)
        self.add_btn.grid(row=0, column=0, padx=5)

        self.complete_btn = tk.Button(btn_frame, text="Marcar como Completada", command=self.complete_task)
        self.complete_btn.grid(row=0, column=1, padx=5)

        self.delete_btn = tk.Button(btn_frame, text="Eliminar Tarea", command=self.delete_task)
        self.delete_btn.grid(row=0, column=2, padx=5)

        # Lista de tareas
        self.listbox = tk.Listbox(root, width=50, height=10, selectmode=tk.SINGLE)
        self.listbox.pack(pady=10)

        # Evento opcional: doble clic para marcar como completada
        self.listbox.bind("<Double-Button-1>", self.complete_task)

    def add_task(self, event=None):
        task = self.entry.get().strip()
        if task:
            self.listbox.insert(tk.END, task)
            self.entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Advertencia", "No puedes añadir una tarea vacía.")

    def complete_task(self, event=None):
        try:
            index = self.listbox.curselection()[0]
            task = self.listbox.get(index)
            if not task.startswith("[✔]"):
                self.listbox.delete(index)
                self.listbox.insert(index, f"[✔] {task}")
        except IndexError:
            messagebox.showinfo("Info", "Selecciona una tarea para marcarla como completada.")

    def delete_task(self):
        try:
            index = self.listbox.curselection()[0]
            self.listbox.delete(index)
        except IndexError:
            messagebox.showinfo("Info", "Selecciona una tarea para eliminarla.")

if __name__ == "__main__":
    root = tk.Tk()
    app = TodoApp(root)
    root.mainloop()

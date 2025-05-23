import tkinter as tk

class Equipo:
    def __init__(self, nombre, color):
        self.nombre = nombre
        self.color = color
        self.puntos = 0

    def sumar_puntos(self, valor):
        self.puntos = min(self.puntos + valor, 200)

    def reiniciar(self):
        self.puntos = 0


class TableroPuntuacion:
    def __init__(self, root):
        self.equipo_rojo = Equipo("AKA", "red")
        self.equipo_azul = Equipo("AO", "blue")

        self.root = root
        self.root.title("Tablero de Puntuación")

        self.segundos = 0
        self.contando = False

        self.label_tiempo = tk.Label(root, text="00:00", font=("Arial", 30), fg="black")
        self.label_tiempo.grid(row=3, column=0, columnspan=7, pady=(0, 10))

        btn_iniciar = tk.Button(root, text="Iniciar", width=10, bg="green", fg="white", command=self.iniciar_tiempo)
        btn_iniciar.grid(row=4, column=0, columnspan=2, pady=5)

        btn_pausar = tk.Button(root, text="Pausar", width=10, bg="orange", fg="white", command=self.pausar_tiempo)
        btn_pausar.grid(row=4, column=2, columnspan=2, pady=5)

        btn_reiniciar_tiempo = tk.Button(root, text="Reiniciar", width=10, bg="gray", fg="white", command=self.reiniciar_tiempo)
        btn_reiniciar_tiempo.grid(row=4, column=4, columnspan=3, pady=5)

        frame_rojo = tk.Frame(root, bd=2, relief="solid", padx=10, pady=10)
        frame_rojo.grid(row=0, column=0, columnspan=3, padx=10, pady=10)
        self.label_rojo = tk.Label(frame_rojo, text="0", font=("Arial", 48), fg="red")
        self.label_rojo.pack()
        tk.Label(root, text="AKA\n(ROJO)", fg="red", font=("Arial", 10)).grid(row=2, column=0, columnspan=3)

        frame_azul = tk.Frame(root, bd=2, relief="solid", padx=10, pady=10)
        frame_azul.grid(row=0, column=4, columnspan=3, padx=10, pady=10)
        self.label_azul = tk.Label(frame_azul, text="0", font=("Arial", 48), fg="blue")
        self.label_azul.pack()
        tk.Label(root, text="AO\n(AZUL)", fg="blue", font=("Arial", 10)).grid(row=2, column=4, columnspan=3)

        for i, valor in enumerate([1, 2, 3]):
            btn = tk.Button(root, text=str(valor), width=5,
                            command=lambda v=valor: self.sumar_puntos("rojo", v))
            btn.grid(row=1, column=i, padx=5, pady=5)

        reset_btn = tk.Button(root, text="RESET", width=10, bg="black", fg="white", command=self.resetear)
        reset_btn.grid(row=1, column=3, padx=10, pady=5)

        for i, valor in enumerate([1, 2, 3]):
            btn = tk.Button(root, text=str(valor), width=5,
                            command=lambda v=valor: self.sumar_puntos("azul", v))
            btn.grid(row=1, column=4 + i, padx=5, pady=5)

        self.actualizar_tiempo()

    def sumar_puntos(self, equipo, valor):
        if equipo == "rojo":
            self.equipo_rojo.sumar_puntos(valor)
        else:
            self.equipo_azul.sumar_puntos(valor)
        self.actualizar_pantalla()

    def resetear(self):
        self.equipo_rojo.reiniciar()
        self.equipo_azul.reiniciar()
        self.actualizar_pantalla()

    def actualizar_pantalla(self):
        self.label_rojo.config(text=str(self.equipo_rojo.puntos))
        self.label_azul.config(text=str(self.equipo_azul.puntos))

    def actualizar_tiempo(self):
        if self.contando:
            self.segundos += 1
        minutos = self.segundos // 60
        segundos = self.segundos % 60
        self.label_tiempo.config(text=f"{minutos:02d}:{segundos:02d}")
        self.root.after(1000, self.actualizar_tiempo)

    def iniciar_tiempo(self):
        self.contando = True

    def pausar_tiempo(self):
        self.contando = False

    def reiniciar_tiempo(self):
        self.contando = False
        self.segundos = 0
        self.label_tiempo.config(text="00:00")

if __name__ == "__main__":
    root = tk.Tk()
    app = TableroPuntuacion(root)
    root.mainloop()

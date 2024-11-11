import tkinter as tk
from tkinter import colorchooser  # Untuk memilih warna

# Fungsi untuk menangani setiap klik tombol
def klik(tombol):
    if tombol == "=":  # Jika tombol sama dengan ditekan, evaluasi ekspresi
        try:
            # Evaluasi ekspresi, ganti simbol ^ dengan ** untuk operasi pangkat
            layar_var.set(eval(layar_var.get().replace("^", "**")))
        except:
            layar_var.set("Error")  # Tampilkan "Error" jika ekspresi tidak valid
    elif tombol == "C":  # Jika tombol C ditekan, bersihkan layar
        layar_var.set("")
    elif tombol == "%":  # Hitung persentase dari nilai pada layar
        try:
            layar_var.set(float(layar_var.get()) / 100)
        except:
            layar_var.set("Error")
    else:
        layar_var.set(layar_var.get() + str(tombol))  # Tambahkan angka/simbol ke layar

# Fungsi untuk mengubah warna latar belakang
def ubah_warna_latar():
    warna = colorchooser.askcolor()[1]  # Pilih warna
    if warna:
        window.config(bg=warna)  # Ubah warna latar kalkulator
        layar.config(bg=warna)

# Fungsi untuk mengubah warna teks
def ubah_warna_teks():
    warna = colorchooser.askcolor()[1]  # Pilih warna
    if warna:
        layar.config(fg=warna)  # Ubah warna teks pada layar

# Fungsi untuk mengubah warna tombol tertentu
def ubah_warna_tombol(btn):
    warna = colorchooser.askcolor()[1]  # Pilih warna
    if warna:
        btn.config(bg=warna)  # Ubah warna tombol

# Membuat window kalkulator
window = tk.Tk()
window.title("Kalkulator Sederhana")

# Variabel untuk menampilkan angka
layar_var = tk.StringVar()
layar = tk.Entry(window, textvariable=layar_var, font=("Arial", 18), justify="right")
layar.grid(row=0, column=0, columnspan=4, sticky="nsew")

# Daftar semua tombol kalkulator
tombol_list = [
    'C', '%', '/', '*',
    '7', '8', '9', '-',
    '4', '5', '6', '+',
    '1', '2', '3', '=',
    '0', '.', '^',
]

# Membuat dan menempatkan tombol di grid, serta menambahkan fungsi ubah warna pada setiap tombol
baris, kolom = 1, 0
for tombol in tombol_list:
    btn = tk.Button(window, text=tombol, command=lambda t=tombol: klik(t), width=5, height=2)
    btn.grid(row=baris, column=kolom)
    # Tambahkan menu klik kanan untuk ubah warna tombol
    btn.bind("<Button-3>", lambda event, b=btn: ubah_warna_tombol(b))
    kolom += 1
    if kolom > 3:  # Pindah ke baris berikutnya setelah 4 kolom
        kolom = 0
        baris += 1

# Tombol untuk mengubah warna latar dan warna teks
tk.Button(window, text="Ubah Warna Latar", command=ubah_warna_latar, width=15, height=2).grid(row=baris, column=0, columnspan=2, sticky="nsew")
tk.Button(window, text="Ubah Warna Teks", command=ubah_warna_teks, width=15, height=2).grid(row=baris, column=2, columnspan=2, sticky="nsew")

window.mainloop()

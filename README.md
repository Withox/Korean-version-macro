[macro.py](https://github.com/user-attachments/files/23708805/macro.py)
import threading
import time
import tkinter as tk
from pynput.keyboard import Controller

keyboard = Controller()
running = False

def start_macro():
    global running
    running = True
    threading.Thread(target=macro_loop).start()

def stop_macro():
    global running
    running = False

def macro_loop():
    global running
    keys = key_entry.get().split()  # 공백 기준으로 여러 키 입력
    delay = float(delay_entry.get())
    count = count_entry.get()

    if count == "":
        count = None  # 무한 반복
    else:
        count = int(count)

    executed = 0
    while running:
        for k in keys:
            keyboard.press(k)
            keyboard.release(k)
            time.sleep(delay)
        if count is not None:
            executed += 1
            if executed >= count:
                stop_macro()

# GUI
root = tk.Tk()
root.title("키보드 매크로")
root.geometry("350x300")

title = tk.Label(root, text="키보드 매크로 설정", font=("Arial", 14))
title.pack(pady=10)

# 입력 키 설정
tk.Label(root, text="입력할 키 (여러 개 입력 시 공백으로 구분)").pack()
key_entry = tk.Entry(root, width=30)
key_entry.pack(pady=5)
key_entry.insert(0, "a s d")  # 기본값

# 반복 지연 시간
tk.Label(root, text="반복 속도 (초)").pack()
delay_entry = tk.Entry(root, width=10)
delay_entry.pack(pady=5)
delay_entry.insert(0, "0.1")

# 반복 횟수
tk.Label(root, text="반복 횟수 (빈칸 = 무한)").pack()
count_entry = tk.Entry(root, width=10)
count_entry.pack(pady=5)
count_entry.insert(0, "")

start_button = tk.Button(root, text="시작", command=start_macro, width=15, height=2)
start_button.pack(pady=10)

stop_button = tk.Button(root, text="정지", command=stop_macro, width=15, height=2)
stop_button.pack()

root.mainloop()

[Be careful when sharing and distributing.]

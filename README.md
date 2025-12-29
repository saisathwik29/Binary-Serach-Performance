# ===========================================================
# Project: Binary Search Performance Test (CO4)
# Language: Python
# GUI Library: Tkinter
# Purpose: Compare Linear and Binary Search algorithm performance
# ===========================================================

import tkinter as tk
from tkinter import messagebox
import time
import random

# ===========================================================
#               SEARCH ALGORITHM IMPLEMENTATIONS
# ===========================================================

def linear_search(arr, target):
    """
    Linear Search Algorithm:
    Iterates through each element one by one
    until it finds the target or reaches the end of the list.
    """
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1


def binary_search(arr, target):
    """
    Binary Search Algorithm:
    Works only on sorted lists.
    Divides the list into halves repeatedly
    until the target is found or search space is empty.
    """
    low = 0
    high = len(arr) - 1

    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1


# ===========================================================
#             MAIN FUNCTION TO RUN PERFORMANCE TEST
# ===========================================================

def run_performance_test():
    try:
        size = int(entry_size.get())
        target = int(entry_target.get())

        if size <= 0:
            messagebox.showerror("Invalid Input", "List size must be greater than 0.")
            return
        if target < 0 or target >= size:
            messagebox.showerror("Invalid Target", "Target must be between 0 and list size - 1.")
            return

    except ValueError:
        messagebox.showerror("Input Error", "Please enter valid integer values.")
        return

    # Generate a sorted list
    arr = list(range(size))

    # -------- Linear Search --------
    start_time = time.perf_counter()
    linear_result = linear_search(arr, target)
    linear_time = (time.perf_counter() - start_time) * 1_000_000  # microseconds

    # -------- Binary Search --------
    start_time = time.perf_counter()
    binary_result = binary_search(arr, target)
    binary_time = (time.perf_counter() - start_time) * 1_000_000

    # Determine which is faster
    if binary_time < linear_time:
        comparison = "Binary Search is Faster."
    elif linear_time < binary_time:
        comparison = "Linear Search is Faster."
    else:
        comparison = "Both performed equally."

    # Prepare output text
    output_text = (
        f"List Size: {size}\n"
        f"Target Value: {target}\n\n"
        f"Linear Search Result:\n"
        f" → Index Found: {linear_result}\n"
        f" → Time Taken: {linear_time:.3f} µs\n\n"
        f"Binary Search Result:\n"
        f" → Index Found: {binary_result}\n"
        f" → Time Taken: {binary_time:.3f} µs\n\n"
        f"Performance Analysis:\n"
        f" → {comparison}"
    )

    # Display output
    result_box.config(state="normal")
    result_box.delete(1.0, tk.END)
    result_box.insert(tk.END, output_text)
    result_box.config(state="disabled")


# ===========================================================
#               HELPER FUNCTIONS (EXTRA FEATURES)
# ===========================================================

def generate_random_target():
    """Generates a random target value within list range."""
    try:
        size = int(entry_size.get())
        if size <= 0:
            messagebox.showerror("Error", "Please enter valid list size first.")
            return
        target = random.randint(0, size - 1)
        entry_target.delete(0, tk.END)
        entry_target.insert(0, str(target))
    except ValueError:
        messagebox.showerror("Error", "Please enter valid list size first.")


def clear_all():
    """Clears all inputs and result box."""
    entry_size.delete(0, tk.END)
    entry_target.delete(0, tk.END)
    result_box.config(state="normal")
    result_box.delete(1.0, tk.END)
    result_box.config(state="disabled")


# ===========================================================
#               GUI DESIGN USING TKINTER
# ===========================================================

root = tk.Tk()
root.title("Binary Search Performance Test")
root.geometry("680x600")
root.config(bg="#eaf1f8")
root.resizable(False, False)

# ---------------- Title ----------------
title = tk.Label(root, text="Binary Search Performance Test",
                 font=("Arial", 20, "bold"), bg="#eaf1f8", fg="#2c3e50")
title.pack(pady=15)

# ---------------- Frame ----------------
frame = tk.Frame(root, bg="white", bd=3, relief="ridge", padx=25, pady=25)
frame.pack(pady=10)

# ----------- Input Fields --------------
tk.Label(frame, text="Enter List Size:", font=("Arial", 12), bg="white").grid(row=0, column=0, sticky="w", pady=8)
entry_size = tk.Entry(frame, width=25, font=("Arial", 11))
entry_size.grid(row=0, column=1, pady=8, padx=10)

tk.Label(frame, text="Enter Target Value:", font=("Arial", 12), bg="white").grid(row=1, column=0, sticky="w", pady=8)
entry_target = tk.Entry(frame, width=25, font=("Arial", 11))
entry_target.grid(row=1, column=1, pady=8, padx=10)

# ----------- Buttons -------------------
btn_random = tk.Button(frame, text="🎲 Generate Random Target", command=generate_random_target,
                       font=("Arial", 10, "bold"), bg="#3498db", fg="white", width=22)
btn_random.grid(row=2, column=1, pady=8)

btn_run = tk.Button(frame, text="Run Performance Test", command=run_performance_test,
                    font=("Arial", 12, "bold"), bg="#27ae60", fg="white", width=20)
btn_run.grid(row=3, columnspan=2, pady=10)

btn_clear = tk.Button(frame, text="Clear All", command=clear_all,
                      font=("Arial", 11, "bold"), bg="#e74c3c", fg="white", width=20)
btn_clear.grid(row=4, columnspan=2, pady=5)

# ---------------- Output Box ----------------
output_frame = tk.LabelFrame(root, text="Result Output", font=("Arial", 12, "bold"),
                             bg="#eaf1f8", fg="#2c3e50", padx=10, pady=10)
output_frame.pack(padx=15, pady=10, fill="both", expand=True)

result_box = tk.Text(output_frame, height=12, width=75, wrap="word",
                     font=("Consolas", 11), bg="white", fg="#2c3e50", relief="solid", bd=1)
result_box.pack(padx=10, pady=10)
result_box.config(state="disabled")

# ---------------- Footer ----------------
footer = tk.Label(root, text="Developed using Python & Tkinter | CO4 - Algorithms & GUI",
                  font=("Arial", 10, "italic"), bg="#eaf1f8", fg="#34495e")
footer.pack(pady=8)

# Start the application
root.mainloop()


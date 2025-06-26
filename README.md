# Kirthi-
import json
import csv
import os
import requests
import tkinter as tk

# === Student Record System ===
STUDENT_FILE = "students.json"

def load_students():
    return json.load(open(STUDENT_FILE)) if os.path.exists(STUDENT_FILE) else {}

def save_students(data):
    json.dump(data, open(STUDENT_FILE, "w"), indent=4)

def add_student(data):
    roll = input("Roll No: ")
    name = input("Name: ")
    marks = input("Marks: ")
    data[roll] = {"name": name, "marks": marks}
    print("✅ Student Added")

def view_students(data):
    for roll, info in data.items():
        print(f"{roll} → {info['name']} ({info['marks']})")

def delete_student(data):
    roll = input("Roll No to delete: ")
    if roll in data:
        del data[roll]
        print("❌ Student Deleted")
    else:
        print("⚠️ Not Found")

def student_module():
    data = load_students()
    while True:
        print("\n--- Student Record System ---")
        print("1. Add  2. View  3. Delete  4. Save & Exit")
        ch = input("Choice: ")
        if ch == "1": add_student(data)
        elif ch == "2": view_students(data)
        elif ch == "3": delete_student(data)
        elif ch == "4":
            save_students(data)
            break

# === Expense Tracker ===
EXPENSE_FILE = "expenses.csv"

def add_expense():
    date = input("Date (YYYY-MM-DD): ")
    desc = input("Description: ")
    amt = input("Amount (+/-): ")
    with open(EXPENSE_FILE, "a", newline='') as f:
        csv.writer(f).writerow([date, desc, amt])
    print("💸 Expense added.")

def view_expenses():
    if not os.path.exists(EXPENSE_FILE):
        print("No data.")
        return
    total = 0
    print("Date | Description | Amount")
    with open(EXPENSE_FILE) as f:
        for row in csv.reader(f):
            print(" | ".join(row))
            total += float(row[2])
    print(f"Total Balance: {total:.2f}")

def expense_module():
    while True:
        print("\n--- Expense Tracker ---")
        print("1. Add  2. View  3. Back")
        ch = input("Choice: ")
        if ch == "1": add_expense()
        elif ch == "2": view_expenses()
        elif ch == "3": break

# === Weather Dashboard ===
def launch_weather_gui():
    def get_weather():
        city = entry.get()
        url = f"https://wttr.in/{city}?format=3"
        label.config(text=requests.get(url).text)

    root = tk.Tk()
    root.title("Weather Dashboard")

    tk.Label(root, text="City:").pack()
    entry = tk.Entry(root)
    entry.pack()

    tk.Button(root, text="Get Weather", command=get_weather).pack()
    label = tk.Label(root, text="Weather will appear here.")
    label.pack()

    root.mainloop()

# === Main Menu ===
def main():
    while True:
        print("\n=== Combined Capstone Project ===")
        print("1. Student Record System")
        print("2. Expense Tracker")
        print("3. Weather Dashboard (GUI)")
        print("4. Exit")
        choice = input("Enter your choice: ")
        if choice == "1":
            student_module()
        elif choice == "2":
            expense_module()
        elif choice == "3":
            launch_weather_gui()
        elif choice == "4":
            print("👋 Goodbye!")
            break
        else:
            print("❗ Invalid Choice.")

if __name__ == "__main__":
    main()

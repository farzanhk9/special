import os
import json
import random
from datetime import datetime

DATA_FILE = "planner.json"

QUOTES = [
    "Do something today that your future self will thank you for.",
    "Small steps every day lead to big results.",
    "Focus on progress, not perfection.",
    "Your only limit is your mind.",
    "Consistency is the key to success.",
    "Dream it. Plan it. Do it.",
    "Don't count the days, make the days count.",
]

def load_data():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r") as f:
            return json.load(f)
    return {}

def save_data(data):
    with open(DATA_FILE, "w") as f:
        json.dump(data, f, indent=4)

def add_task(data):
    today = str(datetime.today().date())
    if today not in data:
        data[today] = []
    task = input("Enter a new task: ")
    data[today].append({"task": task, "done": False})
    save_data(data)
    print(f"✅ Task '{task}' added!")

def mark_done(data):
    today = str(datetime.today().date())
    if today not in data or not data[today]:
        print("📭 No tasks today.")
        return
    for i, t in enumerate(data[today], 1):
        status = "✅" if t["done"] else "❌"
        print(f"{i}. {t['task']} [{status}]")
    choice = input("Enter task number to mark as done: ")
    try:
        idx = int(choice) - 1
        data[today][idx]["done"] = True
        save_data(data)
        print("🎯 Task completed!")
    except:
        print("⚠️ Invalid choice.")

def view_today(data):
    today = str(datetime.today().date())
    print(f"\n📅 Today: {today}")
    print("💡 Motivation:", random.choice(QUOTES))
    if today not in data or not data[today]:
        print("📭 No tasks yet.")
        return
    done = sum(1 for t in data[today] if t["done"])
    total = len(data[today])
    percent = int((done / total) * 100)
    for i, t in enumerate(data[today], 1):
        status = "✅" if t["done"] else "❌"
        print(f"{i}. {t['task']} [{status}]")
    print(f"\nProgress: {done}/{total} tasks ({percent}%)")

def menu():
    data = load_data()
    while True:
        print("\n==== DAILY PLANNER ====")
        print("1. Add a new task")
        print("2. Mark task as done")
        print("3. View today’s plan")
        print("4. Exit")
        choice = input("Choose an option: ")
        if choice == "1":
            add_task(data)
        elif choice == "2":
            mark_done(data)
        elif choice == "3":
            view_today(data)
        elif choice == "4":
            break
        else:
            print("⚠️ Invalid choice.")

if __name__ == "__main__":
    menu()

# TO-DO-List
a simple to do list
"""
Simple Todo List App
--------------------
Run it:  python todo.py

Then just type a number to choose what to do!
"""

import json
import os
from datetime import datetime

TASKS_FILE = "tasks.json"


def load_tasks():
    if not os.path.exists(TASKS_FILE):
        return []
    with open(TASKS_FILE, "r") as f:
        return json.load(f)


def save_tasks(tasks):
    with open(TASKS_FILE, "w") as f:
        json.dump(tasks, f, indent=2)


# ── Commands ──────────────────────────────────

def add_task():
    title = input("Enter task: ")
    tasks = load_tasks()
    tasks.append({"title": title, "done": False, "created": datetime.now().strftime("%Y-%m-%d")})
    save_tasks(tasks)
    print(f'✅ Added: "{title}"\n')


def list_tasks():
    tasks = load_tasks()
    if not tasks:
        print("📭 No tasks yet!\n")
        return
    print("\n📋 Your Tasks:")
    print("─" * 30)
    for i, task in enumerate(tasks, start=1):
        status = "✅" if task["done"] else "⬜"
        print(f"  {i}. {status} {task['title']}")
    print()


def mark_done():
    list_tasks()
    tasks = load_tasks()
    if not tasks:
        return
    num = input("Enter task number to mark done: ")
    if not num.isdigit() or int(num) < 1 or int(num) > len(tasks):
        print("❌ Invalid number\n")
        return
    tasks[int(num) - 1]["done"] = True
    save_tasks(tasks)
    print("✅ Marked as done!\n")


def delete_task():
    list_tasks()
    tasks = load_tasks()
    if not tasks:
        return
    num = input("Enter task number to delete: ")
    if not num.isdigit() or int(num) < 1 or int(num) > len(tasks):
        print("❌ Invalid number\n")
        return
    removed = tasks.pop(int(num) - 1)
    save_tasks(tasks)
    print(f'🗑️  Deleted: "{removed["title"]}"\n')


# ── Main Menu ─────────────────────────────────

def main():
    while True:
        print("=" * 30)
        print("       MY TODO LIST")
        print("=" * 30)
        print("  1. Add a task")
        print("  2. See all tasks")
        print("  3. Mark task as done")
        print("  4. Delete a task")
        print("  5. Quit")
        print("=" * 30)

        choice = input("Choose (1-5): ").strip()

        if choice == "1":
            add_task()
        elif choice == "2":
            list_tasks()
        elif choice == "3":
            mark_done()
        elif choice == "4":
            delete_task()
        elif choice == "5":
            print("Bye! 👋")
            break
        else:
            print("❌ Please type a number from 1 to 5\n")


if __name__ == "__main__":
    main()

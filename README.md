# CRUD
import json
import os

TASK_FILE = "tasks.json"

def load_tasks():
    if os.path.exists(TASK_FILE):
        with open(TASK_FILE, "r") as file:
            return json.load(file)
    return []

def save_tasks(tasks):
    with open(TASK_FILE, "w") as file:
        json.dump(tasks, file, indent=4)

def add_task(description):
    tasks = load_tasks()
    task = {"id": len(tasks) + 1, "description": description, "completed": False}
    tasks.append(task)
    save_tasks(tasks)
    print("Task added successfully!")

def list_tasks():
    tasks = load_tasks()
    for task in tasks:
        status = "[✔]" if task["completed"] else "[ ]"
        print(f"{status} {task['id']}: {task['description']}")

def update_task(task_id, description):
    tasks = load_tasks()
    for task in tasks:
        if task["id"] == task_id:
            task["description"] = description
            save_tasks(tasks)
            print("Task updated successfully!")
            return
    print("Task not found!")

def delete_task(task_id):
    tasks = load_tasks()
    tasks = [task for task in tasks if task["id"] != task_id]
    save_tasks(tasks)
    print("Task deleted successfully!")

def mark_completed(task_id):
    tasks = load_tasks()
    for task in tasks:
        if task["id"] == task_id:
            task["completed"] = True
            save_tasks(tasks)
            print("Task marked as completed!")
            return
    print("Task not found!")

if __name__ == "__main__":
    while True:
        print("\nTask Manager")
        print("1. Add Task")
        print("2. List Tasks")
        print("3. Update Task")
        print("4. Delete Task")
        print("5. Mark Completed")
        print("6. Exit")
        choice = input("Enter choice: ")
        if choice == "1":
            desc = input("Enter task description: ")
            add_task(desc)
        elif choice == "2":
            list_tasks()
        elif choice == "3":
            tid = int(input("Enter task ID: "))
            desc = input("Enter new description: ")
            update_task(tid, desc)
        elif choice == "4":
            tid = int(input("Enter task ID: "))
            delete_task(tid)
        elif choice == "5":
            tid = int(input("Enter task ID: "))
            mark_completed(tid)
        elif choice == "6":
            break
        else:
            print("Invalid choice, try again!")

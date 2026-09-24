'''File Handling Management System using Python
   A console application that performs common file operations.
   -SHAHUL M '''

import os
import shutil
from datetime import datetime


def show_menu():
    print("\n" + "=" * 44)
    print("     FILE HANDLING MANAGEMENT SYSTEM")
    print("=" * 44)
    print(" 1. Create a new file")
    print(" 2. Write content to a file (overwrite)")
    print(" 3. Append content to a file")
    print(" 4. Read a file")
    print(" 5. Show file information")
    print(" 6. Search a word in a file")
    print(" 7. Copy a file")
    print(" 8. Rename a file")
    print(" 9. Delete a file")
    print(" 0. Exit")
    print("=" * 44)


def get_lines():
    print("Enter text (press Enter on an empty line to finish):")
    lines = []
    while True:
        line = input()
        if line == "":
            break
        lines.append(line)
    return "\n".join(lines) + "\n" if lines else ""


def create_file():
    name = input("Enter file name to create: ")
    try:
        with open(name, "x"):
            pass
        print(f"[OK] File '{name}' created successfully.")
    except FileExistsError:
        print(f"[ERROR] File '{name}' already exists.")


def write_file():
    name = input("Enter file name to write: ")
    text = get_lines()
    with open(name, "w") as f:
        f.write(text)
    print(f"[OK] Content written to '{name}'.")


def append_file():
    name = input("Enter file name to append: ")
    if not os.path.exists(name):
        print(f"[ERROR] File '{name}' not found.")
        return
    text = get_lines()
    with open(name, "a") as f:
        f.write(text)
    print(f"[OK] Content appended to '{name}'.")


def read_file():
    name = input("Enter file name to read: ")
    try:
        with open(name, "r") as f:
            data = f.read()
        print(f"\n----- Contents of {name} -----")
        print(data if data else "(file is empty)")
        print("-" * 30)
    except FileNotFoundError:
        print(f"[ERROR] File '{name}' not found.")


def file_info():
    name = input("Enter file name: ")
    if not os.path.isfile(name):
        print(f"[ERROR] File '{name}' not found.")
        return
    with open(name, "r") as f:
        data = f.read()
    modified = datetime.fromtimestamp(os.path.getmtime(name))
    print(f"\nName          : {name}")
    print(f"Size          : {os.path.getsize(name)} bytes")
    print(f"Lines         : {len(data.splitlines())}")
    print(f"Words         : {len(data.split())}")
    print(f"Characters    : {len(data)}")
    print(f"Last modified : {modified:%d-%m-%Y %H:%M:%S}")


def search_word():
    name = input("Enter file name: ")
    word = input("Enter word to search: ").lower()
    try:
        with open(name, "r") as f:
            lines = f.readlines()
    except FileNotFoundError:
        print(f"[ERROR] File '{name}' not found.")
        return
    count = 0
    for number, line in enumerate(lines, start=1):
        hits = line.lower().count(word)
        if hits:
            count += hits
            print(f"Line {number}: {line.strip()}")
    print(f"[RESULT] '{word}' found {count} time(s).")


def copy_file():
    src = input("Enter source file name: ")
    dst = input("Enter destination file name: ")
    try:
        shutil.copy(src, dst)
        print(f"[OK] '{src}' copied to '{dst}'.")
    except FileNotFoundError:
        print(f"[ERROR] File '{src}' not found.")


def rename_file():
    old = input("Enter current file name: ")
    new = input("Enter new file name: ")
    try:
        os.rename(old, new)
        print(f"[OK] '{old}' renamed to '{new}'.")
    except FileNotFoundError:
        print(f"[ERROR] File '{old}' not found.")


def delete_file():
    name = input("Enter file name to delete: ")
    if not os.path.exists(name):
        print(f"[ERROR] File '{name}' not found.")
        return
    if input(f"Delete '{name}'? (y/n): ").lower() == "y":
        os.remove(name)
        print(f"[OK] '{name}' deleted.")
    else:
        print("Deletion cancelled.")


def main():
    actions = {
        "1": create_file, "2": write_file, "3": append_file,
        "4": read_file, "5": file_info, "6": search_word,
        "7": copy_file, "8": rename_file, "9": delete_file,
    }
    while True:
        show_menu()
        choice = input("Enter your choice (0-9): ").strip()
        if choice == "0":
            print("Thank you for using the system. Goodbye!")
            break
        action = actions.get(choice)
        if action:
            action()
        else:
            print("[ERROR] Invalid choice. Please enter 0-9.")


if __name__ == "__main__":
    main()

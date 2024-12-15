# Recipe_sharing-app

import json

data = {
    "users": {}, 
    "recipes": []
}


def save_data():
    with open("recipes.json", "w") as f:
        json.dump(data, f)

def load_data():
    global data
    try:
        with open("recipes.json", "r") as f:
            data = json.load(f)
    except FileNotFoundError:
        pass

def register_user():
    username = input("Enter username: ")
    if username in data["users"]:
        print("Username already exists!")
        return
    password = input("Enter password: ")
    data["users"][username] = {"password": password, "recipes": []}
    print("User registered successfully!")
    save_data()

def login_user():
    username = input("Enter username: ")
    password = input("Enter password: ")
    if username in data["users"] and data["users"][username]["password"] == password:
        print("Login successful!")
        return username
    print("Invalid username or password!")
    return None

def add_recipe(username):
    title = input("Enter recipe title: ")
    ingredients = input("Enter ingredients (comma-separated): ")
    steps = input("Enter steps: ")

    recipe = {
        "title": title,
        "ingredients": ingredients.split(","),
        "steps": steps,
        "author": username
    }
    data["recipes"].append(recipe)
    data["users"][username]["recipes"].append(recipe)
    print("Recipe added successfully!")
    save_data()

def view_recipes():
    if not data["recipes"]:
        print("No recipes available.")
        return
    for i, recipe in enumerate(data["recipes"], start=1):
        print(f"\nRecipe {i}:")
        print(f"Title: {recipe['title']}")
        print(f"Ingredients: {', '.join(recipe['ingredients'])}")
        print(f"Steps: {recipe['steps']}")
        print(f"Author: {recipe['author']}")

def search_recipes():
    ingredient = input("Enter an ingredient to search for: ")
    found = [recipe for recipe in data["recipes"] if ingredient in recipe["ingredients"]]
    if not found:
        print("No recipes found with that ingredient.")
        return
    for i, recipe in enumerate(found, start=1):
        print(f"\nRecipe {i}:")
        print(f"Title: {recipe['title']}")
        print(f"Ingredients: {', '.join(recipe['ingredients'])}")
        print(f"Steps: {recipe['steps']}")
        print(f"Author: {recipe['author']}")

def main():
    load_data()
    print("Welcome to the Recipe Sharing App!")

    while True:
        print("\nMenu:")
        print("1. Register")
        print("2. Login")
        print("3. View Recipes")
        print("4. Search Recipes")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            register_user()
        elif choice == "2":
            username = login_user()
            if username:
                while True:
                    print("\nUser Menu:")
                    print("1. Add Recipe")
                    print("2. Logout")

                    user_choice = input("Enter your choice: ")
                    if user_choice == "1":
                        add_recipe(username)
                    elif user_choice == "2":
                        print("Logged out.")
                        break
                    else:
                        print("Invalid choice!")
        elif choice == "3":
            view_recipes()
        elif choice == "4":
            search_recipes()
        elif choice == "5":
            print("Goodbye!")
            break
        else:
            print("Invalid choice!")

if __name__ == "__main__":
    main()

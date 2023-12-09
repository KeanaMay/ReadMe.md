from dataclasses import dataclass
import os
import matplotlib.pyplot as plt
import numpy as np

plt.style.use("bmh")


@dataclass
class Food:
    name: str
    calories: int
    protein: int
    fats: int
    carbs: int


# Constants for goals
PROTEIN_GOAL = 180  # grams
FAT_GOAL = 80  # grams
CARBS_GOAL = 300  # grams
CALORIE_GOAL_LIMIT = 3000  # kcal


def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')


def print_summary(total_calories, total_protein, total_fats, total_carbs):
    print("\nNutrition Summary:")
    print(f"Total Calories: {total_calories} kcal")
    print(f"Total Protein: {total_protein} grams")
    print(f"Total Fats: {total_fats} grams")
    print(f"Total Carbs: {total_carbs} grams")


def main():
    foods = []
    total_calories = 0
    total_protein = 0
    total_fats = 0
    total_carbs = 0

    while True:
        clear_screen()
        print("Nutrition Tracker")
        print("(1) Add a new food")
        print("(2) Visualize progress")
        print("(3) Quit")

        choice = input("Choose an option: ")

        if choice == "1":
            clear_screen()
            print("Adding a new food!")
            name = input("Name: ")
            calories = int(input("Calories: "))
            protein = int(input("Protein: "))
            fats = int(input("Fat: "))
            carbs = int(input("Carbs: "))
            food = Food(name, calories, protein, fats, carbs)
            foods.append(food)
            total_calories += food.calories
            total_protein += food.protein
            total_fats += food.fats
            total_carbs += food.carbs
            print("Successfully added!")
        elif choice == "2":
            sum(food.calories for food in foods)
            sum(food.protein for food in foods)
            sum(food.fats for food in foods)
            sum(food.carbs for food in foods)

            fig, axs = plt.subplots(2, 2)
            axs[0, 0].pie([total_protein, total_fats, total_carbs], labels=["Proteins", "Fats", "Carbs"],
                          autopct="%1.1f%%")
            axs[0, 0].set_title("Macronutrients Distribution")
            axs[0, 1].bar([0, 1, 2], [total_protein, total_fats, total_carbs], width=0.4)
            axs[0, 1].bar([0.5, 1.5, 2.5], [PROTEIN_GOAL, FAT_GOAL, CARBS_GOAL], width=0.4)
            axs[0, 1].set_title("Macronutrients Progress")
            axs[1, 0].pie([total_calories, CALORIE_GOAL_LIMIT - total_calories], labels=["Calories", "Remaining"],
                          autopct="%1.1f%%")
            axs[1, 0].set_title("Calories Goal Progress")
            axs[1, 1].plot(list(range(len(foods))), np.cumsum([food.calories for food in foods]),
                           label="Calories Eaten")
            axs[1, 1].plot(list(range(len(foods))), [CALORIE_GOAL_LIMIT] * len(foods), label="Calorie Goal")
            axs[1, 1].legend()
            axs[1, 1].set_title("Calories Goal Over Time")
            fig.tight_layout()
            plt.show()

        elif choice == "3":
            break

        else:
            print("Invalid Choice!")


if __name__ == "__main__":
    main()

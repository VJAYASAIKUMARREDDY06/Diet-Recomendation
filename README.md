# Diet-Recomendation
I developed a rule-based personalized diet recommendation system in Python. 
The system calculates BMI and uses user lifestyle details to suggest a suitable diet plan (Balanced, Vegan, Keto, High-Protein, Vegetarian).
import numpy as np
import pandas as pd

# Categories used in recommendations
OPTIONS = {
    "Diet Type": ["Balanced", "Keto", "Vegan", "Vegetarian", "High-Protein"],
    "Medical History": ["None", "Diabetes", "Hypertension", "Heart Disease"],
    "Exercise Frequency": ["Sedentary", "Light", "Moderate", "Active"],
    "Work Environment": ["Office", "Field", "Remote"]
}

def recommend_diet(user):
    """
    Basic rule-based diet recommender.
    Rules are defined using BMI and medical history.
    """
    bmi = user["BMI"]
    history = user["Medical History"]

    if history == "Diabetes":
        return "Low-carb, high-fiber diet"
    elif history == "Heart Disease":
        return "Balanced diet with focus on heart health"
    elif bmi > 30:   # obese
        return "Low-carb, high-fiber diet"
    elif bmi < 18.5: # underweight
        return "High-protein diet"
    else:
        return user["Diet Type"]

def get_user_input():
    """
    Collect user information from console input.
    """
    print("\nEnter your details to get a personalized diet plan:\n")

    age = int(input("Age (18-100): "))
    height = float(input("Height (cm): "))
    weight = float(input("Weight (kg): "))
    bmi = weight / ((height / 100) ** 2)
    calorie_intake = int(input("Daily Calorie Intake (1000-4000): "))

    user_choices = {}
    for key, options in OPTIONS.items():
        print(f"\nSelect {key}:")
        for idx, option in enumerate(options, 1):
            print(f"{idx}. {option}")
        while True:
            try:
                choice = int(input(f"Enter your choice (1-{len(options)}): "))
                if 1 <= choice <= len(options):
                    user_choices[key] = options[choice - 1]
                    break
                else:
                    print("Invalid choice, try again.")
            except ValueError:
                print("Please enter a number.")

    return {
        "Age": age,
        "Height (cm)": height,
        "Weight (kg)": weight,
        "BMI": bmi,
        "Diet Type": user_choices["Diet Type"],
        "Daily Calorie Intake": calorie_intake,
        "Medical History": user_choices["Medical History"],
        "Exercise Frequency": user_choices["Exercise Frequency"],
        "Work Environment": user_choices["Work Environment"]
    }

if __name__ == "__main__":
    user_data = get_user_input()
    result = recommend_diet(user_data)
    print(f"\nRecommended Diet Plan: {result}")

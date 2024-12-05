# JSON File Creator Tool

## Overview
The JSON File Creator Tool is a simple yet powerful command-line application that allows users to create and manage JSON files effortlessly. With this tool, users can define specific fields for their data entries, ensure the uniqueness of entries based on a designated field, and validate the structure of each entry before saving it to a JSON file.

## Features
- **Dynamic Field Entry**: Users can specify the fields for their data entries at runtime.
- **Duplicate Check**: The tool checks for duplicates based on a unique field defined by the user.
- **Data Validation**: Ensures that all specified fields are present in each entry.
- **Easy to Use**: Interactive prompts guide users through the data entry process.

## Getting Started
To use the JSON File Creator Tool, follow these steps:
1. Run the Python script.
2. Input the desired JSON file name when prompted.
3. Define the fields for your data entries.
4. Specify the unique field for duplicate checking.
5. Begin entering data entries, and the tool will handle the rest!

## Complete Python Code

Here’s the complete Python code for the JSON File Creator Tool:

```python
import json
import os

def load_data(file_path):
    """Load data from the JSON file if it exists."""
    if os.path.exists(file_path):
        with open(file_path, 'r') as file:
            return json.load(file)
    return []

def save_data(file_path, data):
    """Save data to the JSON file."""
    with open(file_path, 'w') as file:
        json.dump(data, file, indent=4)

def check_duplicate(data, new_entry, unique_field):
    """Check if the new entry already exists in the data based on the unique field."""
    return any(entry[unique_field] == new_entry[unique_field] for entry in data)

def validate_entry(entry, fields):
    """Check if all fields in the entry match the expected fields."""
    for field in fields:
        if field not in entry:
            return False
    return True

def get_user_input(fields):
    """Get user input for the defined fields."""
    entry = {}
    for field in fields:
        entry[field] = input(f"Enter {field}: ")
    return entry

def main():
    file_path = input("Enter the name of the output JSON file (e.g., data.json): ").strip()
    
    # Get initial fields from the user
    fields = input("Enter the fields separated by commas (e.g., name,address): ").strip().split(',')
    fields = [field.strip() for field in fields]  # Clean whitespace

    # Set the unique field only once at the start
    unique_field = input("Enter the name of the unique field: ").strip()

    data = load_data(file_path)

    while True:
        new_entry = get_user_input(fields)

        # Validate the entry against the fields
        if not validate_entry(new_entry, fields):
            print(f"Error: Entry does not match the expected fields: {', '.join(fields)}. Please try again.")
            continue  # Skip the rest of the loop and prompt for input again

        if not check_duplicate(data, new_entry, unique_field):
            data.append(new_entry)
            save_data(file_path, data)
            print("Entry added successfully.")
        else:
            print(f"Duplicate {unique_field} detected. Entry not added.")

        cont = input("Would you like to add another entry? (yes/no): ").strip().lower()
        
        if cont != 'yes':
            print("Exiting the program. Thank you!")
            break

if __name__ == "__main__":
    main()

## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To implement OpenAI Function Calling using Python to calculate the area and perimeter of a rectangle by integrating a Python function with a Large Language Model (LLM).

### PROBLEM STATEMENT:
To develop a Python program using OpenAI Function Calling to calculate the area and perimeter of a rectangle based on its length and width. The LLM identifies the required function and passes the input parameters to the Python function, which performs the calculations and displays the results.

### DESIGN STEPS:

#### STEP 1:
Import the required libraries and load the OpenAI API key. Define a Python function calculate_rectangle(length, width) to calculate the area and perimeter of a rectangle.

#### STEP 2:
Define the function schema with the function name, description, and required parameters (length and width). Provide the user query and call the OpenAI ChatCompletion endpoint using the gpt-4o-mini model.

#### STEP 3:
Extract the function arguments from the LLM response, execute the Python function, and display the rectangle's length, width, area, and perimeter along with the student's name and register number.

### PROGRAM:

```
import os
import openai
import math
import json

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())
openai.api_key = os.environ['OPENAI_API_KEY']


def calculate_rectangle(length, width):
    """Calculate the area and perimeter of a rectangle."""
    area = length * width
    perimeter = 2 * (length + width)

    rectangle_info = {
        "length": length,
        "width": width,
        "area": area,
        "perimeter": perimeter
    }

    return json.dumps(rectangle_info)


functions = [
    {
        "name": "calculate_rectangle",
        "description": "Calculate the area and perimeter of a rectangle using its length and width",
        "parameters": {
            "type": "object",
            "properties": {
                "length": {
                    "type": "number",
                    "description": "The length of the rectangle"
                },
                "width": {
                    "type": "number",
                    "description": "The width of the rectangle"
                }
            },
            "required": ["length", "width"]
        }
    }
]

messages = [
    {
        "role": "user",
        "content": "Calculate the area and perimeter of a rectangle with length 10 and width 5."
    }
]

response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages,
    functions=functions
)

response_message = response["choices"][0]["message"]

args = json.loads(
    response_message["function_call"]["arguments"]
)

result = json.loads(calculate_rectangle(**args))

name = "PRASIDHA A"
register_number = "212224230204"

print(f"Name            : {name}")
print(f"Register Number : {register_number}")

print(f"Length          : {result['length']} units")
print(f"Width           : {result['width']} units")

print(f"Area            : {result['area']} square units")
print(f"Perimeter       : {result['perimeter']} units")
```

### OUTPUT:

<img width="376" height="151" alt="Screenshot 2026-10-27 160711" src="https://github.com/user-attachments/assets/ed44cbd7-f141-4e80-a413-4f3a36729419" />


### RESULT:
Thus, the OpenAI Function Calling experiment was implemented successfully. The Python function calculated the rectangle's area as 50 square units and perimeter as 30 units using the length and width provided through the LLM function call.

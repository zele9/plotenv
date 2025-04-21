# Unicode Grid Printer

## Overview

The Unicode Grid Printer is a Python script that fetches data from a publicly accessible Google Doc and formats it into a grid of Unicode characters. This tool can be useful for visualizing character data specified by x and y coordinates. 

## Features

- Fetches data from a Google Doc.
- Parses HTML content to extract table data.
- Constructs a grid based on specified coordinates.
- Prints the grid to the console with correct orientation.

## Requirements

- Python 3.x
- `requests` library
- `beautifulsoup4` library
- `lxml` parser

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/zele9/plotenv
   cd unicode-grid-printer
2. Ensure you have pip installed. Install the required libraries:
   pip install requests beautifulsoup4 lxml

## Usage
1. Open the script file (e.g., plotfunc.py) in your preferred text editor.

2. Replace the example URL in the last line of the script with the URL of your own Google Doc:

## python script
import requests  # Import the requests library to handle HTTP requests.
from bs4 import BeautifulSoup  # Import BeautifulSoup from the bs4 library to parse HTML content of doc page.

def display_grid(doc_url):
    # Fetch the Google Doc content using the provided URL.
    response = requests.get(doc_url)
    # method to raise an error for bad responses (e.g., 404, 500).
    response.raise_for_status()

    # Parse the HTML content of the document as text using BeautifulSoup lxml parser.
    parsed_doc = BeautifulSoup(response.text, 'lxml')

    # Find the first table in the document.
    table = parsed_doc.find('table')
    # Check if the table exists.
    if not table:
        print("No table found in the document.")  # Print an error message if no table is found.
        return  # Exit the function if no table is present.

    # Create an empty dictionary to store character coordinates.
    character_coord = {}

    # Extract all table rows, skipping the header row and Slicing the first row (header) off to skip the first row (header).
    table_rows = table.find_all('tr')[1:]  
    
    for row in table_rows:  # Iterate through each row in the table.
        table_columns = row.find_all('td')  # Find all cells (columns) in the current row.
        if len(table_columns) < 3:  # Ensure there are enough columns (x, character, y).
            continue  # If not, skip to the next row.

        try:
            # Parse the x-coordinate, character, and y-coordinate from the columns.
            x_coord = int(table_columns[0].text.strip())  # Convert the first column to an integer (x-coordinate).
            character = table_columns[1].text.strip()  # Get the character from the second column.
            y_coord = int(table_columns[2].text.strip())  # Convert the third column to an integer (y-coordinate).
            
            # Store the character at the specified (x, y) position in the dictionary.
            character_coord[(x_coord, y_coord)] = character
        except ValueError:
            continue  # If parsing fails, skip this row.

    # Check if any valid character_coord were found.
    if not character_coord:
        print("No valid data found.")  # Print a message if no valid data is present.
        return  # Exit the function if no valid character_coord are found.
    
    # Determine the maximum x and y coordinates to size the grid correctly.
    max_x = max(x_coord for x_coord, y in character_coord.keys())  # Get the maximum x-coordinate.
    max_y = max(y_coord for x, y_coord in character_coord.keys())  # Get the maximum y-coordinate.
    
    # Create a grid filled with spaces, sized according to the maximum coordinates.
    grid = [[' ' for _ in range(max_x + 1)] for _ in range(max_y + 1)]
    
    #print(character_coord.items())
    # Fill the grid with characters from the character_coord dictionary.
    for (x, y), character in character_coord.items():
        grid[max_y - y][x] = character  # Place the character in the grid, flipping the y-coordinate.

    # Print the grid row by row.
    for row in grid:
        print(''.join(row))  # Join characters in each row and print.

# Example usage with the URL of the Google Doc.
display_grid('https://docs.google.com/document/d/e/2PACX-1vQGUck9HIFCyezsrBSnmENk5ieJuYwpt7YHYEzeNJkIb9OSDdx-ov2nRNReKQyey-cwJOoEKUhLmN9z/pub')

python plotfunc.py
The output will display a grid of Unicode characters in your console.

## Example Document Format
The Google Doc should contain a table with the following columns:

x-coordinate: The horizontal position of the character.
Character: The Unicode character to display.
y-coordinate: The vertical position of the character.
## Example Table
The tables to use in the code can be found in the following links:
https://docs.google.com/document/d/e/2PACX-1vRMx5YQlZNa3ra8dYYxmv-QIQ3YJe8tbI3kqcuC7lQiZm-CSEznKfN_HYNSpoXcZIV3Y_O3YoUB1ecq/pub
https://docs.google.com/document/d/e/2PACX-1vQGUck9HIFCyezsrBSnmENk5ieJuYwpt7YHYEzeNJkIb9OSDdx-ov2nRNReKQyey-cwJOoEKUhLmN9z/pub

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing
Contributions are welcome! Please feel free to submit a pull request or open an issue.

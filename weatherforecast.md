
# Weather Record ADT
class WeatherRecord:
    """
    Abstract Data Type for a weather record.
    Attributes:
        date (str): Date in the format (DD/MM/YYYY or YYYY).
        city (str): Name of the city.
        temperature (float): Temperature value for that city on that date.
    """

    def __init__(self, date: str, city: str, temperature: float):
        self.date = date
        self.city = city
        self.temperature = temperature

    def __repr__(self):
        return f"WeatherRecord(Date={self.date}, City={self.city}, Temp={self.temperature}°C)"

# Assignment 1 - Weather Data Storage System
# Course: Data Structures (ENCS205 / ENCA201)
# Language: Python
# Author: Mritunjay Tripathi

from typing import List

# Weather Record ADT
class WeatherRecord:
    """
    Abstract Data Type for a weather record.
    Attributes:
        date (str): Date in the format (DD/MM/YYYY).
        city (str): Name of the city.
        temperature (float): Temperature value for that city on that date.
    """

    def __init__(self, date: str, city: str, temperature: float):
        self.date = date
        self.city = city
        self.temperature = temperature

    def __repr__(self):
        return f"WeatherRecord(Date={self.date}, City={self.city}, Temp={self.temperature}°C)"


# Weather Data Storage using 2D Array
class WeatherDataStorage:
    """
    Storage system for weather records using a 2D array.
    Rows represent years, and columns represent cities.
    """

    def __init__(self, years: List[int], cities: List[str]):
        self.years = years
        self.cities = cities
        # Initialize 2D array with None (no data initially)
        self.data = [[None for _ in cities] for _ in years]

    # Insert a new record
    def insert(self, date: str, city: str, temperature: float):
        # Extract year from full date (assumes format DD/MM/YYYY)
        year = int(date.split("/")[-1])
        if year not in self.years or city not in self.cities:
            print("Invalid year or city!")
            return
        row = self.years.index(year)
        col = self.cities.index(city)
        self.data[row][col] = WeatherRecord(date, city, temperature)

    # Delete a record (set it back to None)
    def delete(self, year: int, city: str):
        if year in self.years and city in self.cities:
            row = self.years.index(year)
            col = self.cities.index(city)
            self.data[row][col] = None

    # Retrieve a record
    def retrieve(self, city: str, year: int):
        if year in self.years and city in self.cities:
            row = self.years.index(year)
            col = self.cities.index(city)
            return self.data[row][col]
        return None

    # Row-Major Traversal (year-wise)
    def rowMajorAccess(self):
        print("Row-Major Traversal:")
        for i, year in enumerate(self.years):
            for j, city in enumerate(self.cities):
                print(f"({year}, {city}) => {self.data[i][j]}")

    # Column-Major Traversal (city-wise)
    def columnMajorAccess(self):
        print("Column-Major Traversal:")
        for j, city in enumerate(self.cities):
            for i, year in enumerate(self.years):
                print(f"({year}, {city}) => {self.data[i][j]}")

    # Handle Sparse Data (replace None with sentinel)
    def handleSparseData(self, sentinel=-9999):
        sparse_matrix = []
        for i, row in enumerate(self.data):
            sparse_row = []
            for val in row:
                if val is None:
                    sparse_row.append(sentinel)
                else:
                    sparse_row.append(val.temperature)
            sparse_matrix.append(sparse_row)
        return sparse_matrix

    # Analyze Complexity
    def analyzeComplexity(self):
        """
        Time Complexity:
        - Insert/Delete/Retrieve: O(1) (direct index access)
        - Traversal: O(n*m) where n = years, m = cities

        Space Complexity:
        - O(n*m) for 2D array storage
        """
        return {
            "Insert": "O(1)",
            "Delete": "O(1)",
            "Retrieve": "O(1)",
            "Traversal": "O(n*m)",
            "Space": "O(n*m)"
        }


# Example Usage
if __name__ == "__main__":
    years = [2023, 2024, 2025]
    cities = ["Delhi", "Mumbai", "Chennai"]

    # Initialize storage
    storage = WeatherDataStorage(years, cities)

    # Insert records
    storage.insert("12/05/2023", "Delhi", 32.5)
    storage.insert("15/07/2023", "Mumbai", 28.1)
    storage.insert("01/01/2024", "Chennai", 30.2)

    # Retrieve a record
    print("Retrieve (Delhi, 2023):", storage.retrieve("Delhi", 2023))

    # Delete a record
    storage.delete(2023, "Mumbai")
    print("After Deletion (Mumbai, 2023):", storage.retrieve("Mumbai", 2023))

    # Row-Major Traversal
    storage.rowMajorAccess()

    # Column-Major Traversal
    storage.columnMajorAccess()

    # Sparse Data Handling
    print("Sparse Matrix Representation:", storage.handleSparseData())

    # Complexity Analysis
    print("Complexity Analysis:", storage.analyzeComplexity())

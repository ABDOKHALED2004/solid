# solid
# single risposilty 
from pathlib import Path
from zipfile import ZipFile

class FileManager:
    def __init__(self, filename):
        self.path = Path(filename)

    def read(self, encoding="utf-8"):
        return self.path.read_text(encoding)

    def write(self, data, encoding="utf-8"):
        self.path.write_text(data, encoding)

class ZipFileManager:
    def __init__(self, filename):
        self.path = Path(filename)

    def compress(self):
        if not self.path.exists():
            raise FileNotFoundError(f"The file '{self.path}' does not exist and cannot be compressed.")
        with ZipFile(self.path.with_suffix(".zip"), mode="w") as archive:
            archive.write(self.path, arcname=self.path.name)

    def decompress(self):
        zip_path = self.path.with_suffix(".zip")
        if not zip_path.exists():
            raise FileNotFoundError(f"The zip file '{zip_path}' does not exist and cannot be decompressed.")
        with ZipFile(zip_path, mode="r") as archive:
            archive.extractall()

# open close princple.

from abc import ABC, abstractmethod
from math import pi

class Shape(ABC):
    def __init__(self, shape_type):
        self.shape_type = shape_type

    @abstractmethod
    def calculate_area(self):
        pass

    def __str__(self):
        return f"{self.shape_type.capitalize()} with area: {self.calculate_area():.2f}"

class Circle(Shape):
    def __init__(self, radius):
        super().__init__("circle")
        self.radius = radius

    def calculate_area(self):
        return pi * self.radius**2

class Rectangle(Shape):
    def __init__(self, width, height):
        super().__init__("rectangle")
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height

class Square(Shape):
    def __init__(self, side):
        super().__init__("square")
        self.side = side

    def calculate_area(self):
        return self.side**2
#liskov supstitution praciple

from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def calculate_area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height

class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)

# Example Usage:
# rect = Rectangle(4, 5)
# print(f"Rectangle Area: {rect.calculate_area()}")
# square = Square(4)
# print(f"Square Area: {square.calculate_area()}")

# interface sigriation principle 
from abc import ABC, abstractmethod

class Printer(ABC):
    @abstractmethod
    def print(self, document):
        pass

class Fax(ABC):
    @abstractmethod
    def fax(self, document):
        pass

class Scanner(ABC):
    @abstractmethod
    def scan(self, document):
        pass

class OldPrinter(Printer):
    def print(self, document):
        print(f"Printing {document} in black and white...")

class NewPrinter(Printer, Fax, Scanner):
    def print(self, document):
        print(f"Printing {document} in color...")

    def fax(self, document):
        print(f"Faxing {document}...")

    def scan(self, document):
        print(f"Scanning {document}...")

# Example Usage:
old_printer = OldPrinter()
old_printer.print("Document1")

new_printer = NewPrinter()
new_printer.print("Document2")
new_printer.fax("Document2")
new_printer.scan("Document2")
#dependancy invirsion
from abc import ABC, abstractmethod

class FrontEnd:
    def __init__(self, data_source):
        self.data_source = data_source

    def display_data(self):
        data = self.data_source.get_data()
        print("Display data:", data)

class DataSource(ABC):
    @abstractmethod
    def get_data(self):
        pass

class Database(DataSource):
    def get_data(self):
        return "Data from the database"

class API(DataSource):
    def get_data(self):
        return "Data from the API"

# Example Usage:
db_front_end = FrontEnd(Database())
db_front_end.display_data()

api_front_end = FrontEnd(API())
api_front_end.display_data()

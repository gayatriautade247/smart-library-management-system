"""
Smart Library Management System
Unit 2: Data Structures and Collection Management using Python

This beginner-friendly console application demonstrates strings, lists, tuples,
sets, and dictionaries in one integrated library system.
"""

# The full catalogue is a list of dictionaries.
# Each book is stored as a dictionary, and ISBN is kept as a tuple.
library = [
    {
        "book_id": "B1001",
        "title": "The Great Gatsby",
        "author": "F. Scott Fitzgerald",
        "category": "Fiction",
        "isbn": ("978", "0743273565"),
        "price": 10.99,
        "available": True,
        "shelf_number": "A-01"
    },
    {
        "book_id": "B1002",
        "title": "Dune",
        "author": "Frank Herbert",
        "category": "Sci-Fi",
        "isbn": ("978", "0441172719"),
        "price": 14.50,
        "available": True,
        "shelf_number": "B-02"
    },
    {
        "book_id": "B1003",
        "title": "Atomic Habits",
        "author": "James Clear",
        "category": "Self-Help",
        "isbn": ("978", "0735211292"),
        "price": 12.00,
        "available": False,
        "shelf_number": "C-03"
    }
]


def read_nonempty(prompt):
    """Read text and reject empty input; strip removes extra spaces."""
    while True:
        value = input(prompt).strip()
        if value:
            return value
        print("Input cannot be empty. Please try again.")


def find_book_by_id(book_id):
    """Return the matching book dictionary, or None when it is absent."""
    for book in library:
        if book["book_id"].lower() == book_id.strip().lower():
            return book
    return None


def display_book(book):
    """Display a book's fields in a readable format."""
    print("-" * 45)
    print(f"Book ID: {book['book_id']}")
    print(f"Title: {book['title']}")
    print(f"Author: {book['author']}")
    print(f"Category: {book['category']}")
    print(f"ISBN: {'-'.join(book['isbn'])}")
    print(f"Price: Rs. {book['price']:.2f}")
    print(f"Availability: {'Available' if book['available'] else 'Borrowed'}")
    print(f"Shelf: {book['shelf_number']}")
    print("-" * 45)


def add_book():
    """Add a book if its ID is not already in the catalogue."""
    book_id = read_nonempty("Book ID: ")
    if find_book_by_id(book_id):
        print("Book already exists. Duplicate ID rejected.")
        return

    title = read_nonempty("Title: ").title()
    author = read_nonempty("Author: ").title()
    category = read_nonempty("Category: ").title()
    isbn_prefix = read_nonempty("ISBN prefix (e.g. 978): ")
    isbn_code = read_nonempty("ISBN code: ")
    shelf = read_nonempty("Shelf number: ")

    try:
        price = float(input("Price (Rs.): "))
        if price < 0:
            raise ValueError
    except ValueError:
        print("Invalid price. Enter a number greater than or equal to zero.")
        return

    book = {
        "book_id": book_id.upper(),
        "title": title,
        "author": author,
        "category": category,
        "isbn": (isbn_prefix, isbn_code),  # tuple: fixed ISBN components
        "price": price,
        "available": True,
        "shelf_number": shelf.upper()
    }
    library.append(book)  # list: add record to inventory
    print("Book added successfully.")


def search_book():
    """Search for a book by ID or title, ignoring letter case."""
    term = read_nonempty("Enter book ID or title: ").lower()
    matches = [
        book for book in library
        if book["book_id"].lower() == term or term in book["title"].lower()
    ]
    if matches:
        for book in matches:
            display_book(book)
    else:
        print("Book Not Found.")


def update_book():
    """Update selected editable fields of a book."""
    book_id = read_nonempty("Enter Book ID to update: ")
    book = find_book_by_id(book_id)
    if not book:
        print("Book Not Found.")
        return

    print("Fields: title, author, category, shelf_number, price")
    field = read_nonempty("Field to update: ").lower()
    if field not in {"title", "author", "category", "shelf_number", "price"}:
        print("Invalid field.")
        return

    new_value = read_nonempty("New value: ")
    if field == "price":
        try:
            new_value = float(new_value)
            if new_value < 0:
                raise ValueError
        except ValueError:
            print("Invalid price.")
            return
    elif field in {"title", "author", "category"}:
        new_value = new_value.title()
    else:
        new_value = new_value.upper()

    book[field] = new_value
    print("Book updated successfully.")


def delete_book():
    """Delete a book by its unique ID."""
    book_id = read_nonempty("Enter Book ID to delete: ")
    book = find_book_by_id(book_id)
    if not book:
        print("Book Not Found.")
        return
    library.remove(book)
    print("Book deleted successfully.")


def display_books():
    """Display all books, with an empty-catalogue check."""
    if not library:
        print("The catalogue is empty.")
        return
    for book in library:
        display_book(book)


def borrow_or_return():
    """Toggle a book's availability between borrowed and available."""
    book_id = read_nonempty("Enter Book ID: ")
    book = find_book_by_id(book_id)
    if not book:
        print("Book Not Found.")
        return

    if book["available"]:
        book["available"] = False
        print("Book marked as borrowed.")
    else:
        book["available"] = True
        print("Book marked as returned and available.")


def show_reports():
    """Generate count, available count, categories, authors, and sorted titles."""
    total = len(library)
    available = sum(1 for book in library if book["available"])
    categories = {book["category"] for book in library}  # set removes duplicates
    authors = {book["author"] for book in library}
    titles = sorted(book["title"] for book in library)

    print(f"Total books: {total}")
    print(f"Available books: {available}")
    print(f"Borrowed books: {total - available}")
    print("Unique categories:", ", ".join(sorted(categories)))
    print("Unique authors:", ", ".join(sorted(authors)))
    print("Titles alphabetically:", ", ".join(titles))

    if library:
        average_price = sum(book["price"] for book in library) / total
        print(f"Average listed price: Rs. {average_price:.2f}")
        highest = max(library, key=lambda book: book["price"])
        print("Highest-priced book:", highest["title"], f"(Rs. {highest['price']:.2f})")
    else:
        print("No report values available because the catalogue is empty.")


def main():
    """Run the menu until the user chooses Exit."""
    while True:
        print("\\n===== SMART LIBRARY MANAGEMENT SYSTEM =====")
        print("1. Add book")
        print("2. Search book")
        print("3. Update book")
        print("4. Delete book")
        print("5. Display all books")
        print("6. Borrow / Return book")
        print("7. Generate reports")
        print("8. Exit")
        choice = input("Choose an option (1-8): ").strip()

        if choice == "1":
            add_book()
        elif choice == "2":
            search_book()
        elif choice == "3":
            update_book()
        elif choice == "4":
            delete_book()
        elif choice == "5":
            display_books()
        elif choice == "6":
            borrow_or_return()
        elif choice == "7":
            show_reports()
        elif choice == "8":
            print("Thank you for using the Smart Library Management System.")
            break
        else:
            print("Invalid choice. Please enter a number from 1 to 8.")


if __name__ == "__main__":
    main()

# Smart Library Management System — Unit 2

## Project overview
A beginner-friendly, menu-driven Python program for organizing library books. It demonstrates strings, lists, tuples, sets, and dictionaries together.

## Requirements
- Python 3.9 or newer
- No external libraries required

## How to run
1. Extract the project ZIP.
2. Open a terminal in this folder.
3. Run: `python smart_library.py`
   - On some Windows installations, use: `py smart_library.py`

## Features
- Add a book and reject duplicate book IDs
- Search by book ID or part of a title (case-insensitive)
- Update book details
- Delete a book
- Display the catalogue
- Mark a book as borrowed or returned
- Report total/available/borrowed books, unique categories/authors, sorted titles, average price, and highest-priced book

## Data structures used
- **String:** titles, author names, categories, shelf numbers and messages
- **List:** the catalogue of book dictionaries
- **Tuple:** ISBN components
- **Set:** unique categories and authors
- **Dictionary:** fields for each book record

## Important note
This is a classroom demonstration. Records are stored in memory and reset when the program is restarted. A file/database persistence feature is not included in this version.

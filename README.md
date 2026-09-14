<h1 align="center">
  Library Management System
</h1>

<p align="center">
  A desktop library management application built with C# and WPF, supporting multiple user roles — admins, first- and second-tier librarians, and clients — for managing libraries, books, borrowing, reservations, and membership.
</p>

<div align="center">

![C#](https://img.shields.io/badge/C%23-.NET-512BD4)
![WPF](https://img.shields.io/badge/WPF-Desktop-0C54C2)
![status](https://img.shields.io/badge/Status-University%20Project-yellow)

</div>

## About the Project

This project is a role-based desktop application for managing a network of libraries and their branches — books, book copies, client memberships, borrowing, reservations, and deadline extensions. The system is built around a layered architecture (Models, DAOs, Storages, Controllers) per domain module, following the DAO and Observer design patterns, with data persisted to CSV files through a custom serializer.

Four distinct user roles are supported, each with its own dedicated set of screens:

- **Admins** — manage libraries, library branches, and librarians, and view reports such as the most-read books.
- **Second-tier librarians** — manage the book catalog and copies, process borrowings and returns, and handle deadline extension requests.
- **First-tier librarians** — manage client registration, membership cards, and membership extensions.
- **Clients** — browse their borrowed books and account information.

## Features

- Role-based login and separate home screens for admins, first-tier librarians, second-tier librarians, and clients
- Library and library branch management, including library-specific rules
- Book catalog management — adding books and individual book copies
- Book borrowing and returns, with tracking of active borrows per client
- Reservation system for books that are currently unavailable
- Membership card issuing and membership extension requests
- Deadline extension requests for borrowed books, reviewed by librarians
- Notifications for clients (e.g. due dates, reservation availability)
- "Most read books" reporting for admins
- Data persistence via CSV files, using a custom serialization layer
- Observer pattern used to propagate state changes (e.g. notifications) across the application

## Architecture

The codebase is organized by domain (Users, Books, Libraries), with each domain following the same layered structure:

- **Models** — plain domain entities (e.g. `Book`, `Client`, `Librarian`, `Library`, `Borrow`, `Reservation`).
- **DAOs** — read/write access to the underlying CSV data files.
- **Storages** — in-memory collections built on top of the DAOs, providing lookup and query operations.
- **Controllers** — business logic that coordinates storages across domains (e.g. borrowing a book updates both the book copy and the client's borrow record).

Cross-cutting utilities include a generic `Serializer` (`ISerializable`) for CSV persistence and an `Observer`/`Subject` implementation for notifying parts of the UI of state changes.

Design documentation — use case diagrams, a class diagram, sequence diagrams (borrowing, reservation, admin actions), an activity diagram, and a state diagram — is included under `Diagrams/`.

## Technologies

**Language & Framework:** C#, .NET, WPF (XAML)

**Design patterns:** DAO, Observer, layered architecture (Model / DAO / Storage / Controller)

**Persistence:** CSV files with a custom serializer

## Project Structure

```
📦 Biblioteka
 ┣ 📂 Core
 ┃ ┣ 📂 Users      — clients, librarians, admins (models, DAOs, storages, controllers)
 ┃ ┣ 📂 Books      — books, book copies (models, DAOs, storages, controllers)
 ┃ ┗ 📂 Libraries  — libraries, branches, rules (models, DAOs, storages, controllers)
 ┣ 📂 GUI
 ┃ ┣ 📂 Admins       — library/branch/librarian management, most-read-books report
 ┃ ┣ 📂 Librarians
 ┃ ┃ ┣ 📂 LibrariansFirstTier   — client registration, membership cards
 ┃ ┃ ┗ 📂 LibrariansSecondTier  — book catalog, borrowing/returns, deadline extensions
 ┃ ┗ 📂 Clients      — client home, borrowed books view
 ┣ 📂 Utilities
 ┃ ┣ 📂 Observer     — observer/subject interfaces
 ┃ ┗ 📂 Serializer   — generic CSV serialization
 ┣ 📂 Data           — CSV data files (books, borrows, reservations, clients, etc.)
 ┣ 📂 Diagrams       — UML diagrams and requirements analysis
 ┗ MainWindow.xaml   — application entry window
```

## Running the Project

### Prerequisites

- Windows with .NET Framework / .NET SDK compatible with the project (see `Biblioteka.csproj`)
- Visual Studio 2019 or later (recommended)

### Steps

```bash
# Open the solution
Biblioteka.sln
```

Open `Biblioteka.sln` in Visual Studio, restore any NuGet packages if prompted, then build and run the `Biblioteka` project (F5). The application reads and writes its data from the CSV files under `Biblioteka/Data/`.

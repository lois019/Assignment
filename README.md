App.java
/*
 * This should be your main class where all your objects will be created
 */
package org.example;

public class App {
    public static void main(String[] args) {
        org.example.Library library = new org.example.Library();

        // Add books
        Book book1 = new Book("1984", "George Orwell", "123456789", false);
        Book book2 = new Book("To Kill a Mockingbird", "Harper Lee", "987654321", false);
        library.addBook(book1);
        library.addBook(book2);

        // Register patrons
        Patron patron1 = new Patron("Alice", "P001");
        Patron patron2 = new Patron("Bob", "P002");
        library.registerPatron(patron1);
        library.registerPatron(patron2);

        // Simulate borrowing and returning books
        System.out.println("Borrowing 1984 for Alice: " + library.borrowBook("123456789", "Alice"));
        System.out.println("Returning 1984 for Alice: " +
                library.returnBook("123456789", "Alice"));
    }
}
Book.java
package org.example;

public class Book {
    private String title;
    private String author;
    private String isbn;
    private int yearPublished;
    private boolean isBorrowed;

    // Constructor for initializing all attributes
    public Book(String title, String author, String isbn, boolean isBorrowed) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.isBorrowed = isBorrowed;
    }

    // Constructor for initializing without borrowing status
    public Book(String title, String author, int yearPublished) {
        this.title = title;
        this.author = author;
        this.yearPublished = yearPublished;
        this.isBorrowed = false; // Default to not borrowed
    }

    // Getters and Setters
    public void setBorrowed(boolean isBorrowed) {
        this.isBorrowed = isBorrowed;
    }

    public String getISBN() {
        return isbn;
    }

    public boolean isBorrowed() {
        return isBorrowed;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public Integer getYearPublished() {
        return yearPublished;
    }
}
Library.java
package org.example;

import java.util.ArrayList;
import java.util.List;

public class Library {
    private List<Book> books;
    private List<Patron> patrons;

    public Library() {
        this.books = new ArrayList<>();
        this.patrons = new ArrayList<>();
    }

    public void addBook(Book book) {
        books.add(book);
    }

    public void registerPatron(Patron patron) {
        patrons.add(patron);
    }

    public boolean borrowBook(String isbn, String patronName) {
        for (Book book : books) {
            if (book.getISBN().equals(isbn) && !book.isBorrowed()) {
                for (Patron patron : patrons) {
                    if (patron.getName().equals(patronName)) {
                        book.setBorrowed(true);
                        patron.borrowBook(book);
                        return true;
                    }
                }
            }
        }
        return false;
    }

    public boolean returnBook(String isbn, String patronName) {
        for (Patron patron : patrons) {
            if (patron.getName().equals(patronName)) {
                return patron.returnBook(isbn);
            }
        }
        return false;
    }
}
Patron.java

package org.example;

import java.util.ArrayList;
import java.util.List;

public class Patron {

    private String name;
    private List<Book> borrowedBooks;

    public Patron(String name, String id) {
        this.name = name;
        this.borrowedBooks = new ArrayList<>();
    }

    public String getName() {
        return name;
    }

    public void borrowBook(Book book) {
        borrowedBooks.add(book);
    }

    public boolean returnBook(String isbn) {
        for (Book book : borrowedBooks) {
            if (book.getISBN().equals(isbn)) {
                borrowedBooks.remove(book);
                book.setBorrowed(false);
                return true;
            }
        }
        return false;
    }
}

# Library-Management-System
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

class Book {
    private String title;
    private String author;
    private boolean isAvailable;

    public Book(String title, String author) {
        this.title = title;
        this.author = author;
        this.isAvailable = true;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void checkOut() {
        if (isAvailable) {
            isAvailable = false;
            JOptionPane.showMessageDialog(null, "You have successfully checked out \"" + title + "\".");
        } else {
            JOptionPane.showMessageDialog(null, "Sorry, \"" + title + "\" is currently not available.");
        }
    }

    public void returnBook() {
        if (!isAvailable) {
            isAvailable = true;
            JOptionPane.showMessageDialog(null, "You have successfully returned \"" + title + "\".");
        } else {
            JOptionPane.showMessageDialog(null, "This book wasn't checked out.");
        }
    }
}

class Library {
    private ArrayList<Book> books;

    public Library() {
        books = new ArrayList<>();
    }

    public void addBook(Book book) {
        books.add(book);
        JOptionPane.showMessageDialog(null, "Book \"" + book.getTitle() + "\" by " + book.getAuthor() + " added to the library.");
    }

    public ArrayList<Book> getBooks() {
        return books;
    }

    public void checkOutBook(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                book.checkOut();
                return;
            }
        }
        JOptionPane.showMessageDialog(null, "Book not found in the library.");
    }

    public void returnBook(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                book.returnBook();
                return;
            }
        }
        JOptionPane.showMessageDialog(null, "Book not found in the library.");
    }
}

public class LibraryManagementSystemUI extends JFrame {
    private Library library;
    private JTextArea bookListArea;

    public LibraryManagementSystemUI() {
        library = new Library();
        setupUI();
    }

    private void setupUI() {
        setTitle("Library Management System");
        setSize(500, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        // Book List Area
        bookListArea = new JTextArea();
        bookListArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(bookListArea);
        panel.add(scrollPane, BorderLayout.CENTER);

        // Buttons Panel
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(1, 4));

        JButton addButton = new JButton("Add Book");
        addButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                addBook();
            }
        });
        buttonPanel.add(addButton);

        JButton viewButton = new JButton("View Books");
        viewButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                viewBooks();
            }
        });
        buttonPanel.add(viewButton);

        JButton checkOutButton = new JButton("Check Out");
        checkOutButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                checkOutBook();
            }
        });
        buttonPanel.add(checkOutButton);

        JButton returnButton = new JButton("Return Book");
        returnButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                returnBook();
            }
        });
        buttonPanel.add(returnButton);

        panel.add(buttonPanel, BorderLayout.SOUTH);

        add(panel);
    }

    private void addBook() {
        String title = JOptionPane.showInputDialog(this, "Enter book title:");
        String author = JOptionPane.showInputDialog(this, "Enter book author:");
        if (title != null && author != null && !title.trim().isEmpty() && !author.trim().isEmpty()) {
            library.addBook(new Book(title, author));
            viewBooks();
        } else {
            JOptionPane.showMessageDialog(this, "Invalid input. Please try again.");
        }
    }

    private void viewBooks() {
        StringBuilder bookList = new StringBuilder();
        ArrayList<Book> books = library.getBooks();
        if (books.isEmpty()) {
            bookList.append("No books available in the library.");
        } else {
            for (Book book : books) {
                String availability = book.isAvailable() ? "Available" : "Checked Out";
                bookList.append("- ").append(book.getTitle()).append(" by ").append(book.getAuthor()).append(" [").append(availability).append("]\n");
            }
        }
        bookListArea.setText(bookList.toString());
    }

    private void checkOutBook() {
        String title = JOptionPane.showInputDialog(this, "Enter the title of the book to check out:");
        if (title != null && !title.trim().isEmpty()) {
            library.checkOutBook(title);
            viewBooks();
        } else {
            JOptionPane.showMessageDialog(this, "Invalid input. Please try again.");
        }
    }

    private void returnBook() {
        String title = JOptionPane.showInputDialog(this, "Enter the title of the book to return:");
        if (title != null && !title.trim().isEmpty()) {
            library.returnBook(title);
            viewBooks();
        } else {
            JOptionPane.showMessageDialog(this, "Invalid input. Please try again.");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new LibraryManagementSystemUI().setVisible(true);
            }
        });
    }
}

/*
 * Library Management System
 * Components: JDK Project Setup, JDBC, MySQL, DAO Pattern, Simple UI
 */

// Project Structure
// - src/
//   - model/Book.java
//   - dao/BookDAO.java
//   - util/DBConnection.java
//   - ui/LibraryUI.java
//   - Main.java

// 1. DBConnection.java
package util;

import java.sql.*;

public class DBConnection {
    private static final String URL = "jdbc:mysql://localhost:3306/library_db";
    private static final String USER = "root";
    private static final String PASSWORD = "password";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}

// 2. Book.java
package model;

public class Book {
    private int id;
    private String title;
    private String author;

    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
    }

    public int getId() { return id; }
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
}

// 3. BookDAO.java
package dao;

import model.Book;
import util.DBConnection;

import java.sql.*;
import java.util.*;

public class BookDAO {
    public void addBook(Book book) {
        try (Connection conn = DBConnection.getConnection();
             PreparedStatement stmt = conn.prepareStatement("INSERT INTO books (id, title, author) VALUES (?, ?, ?)");) {
            stmt.setInt(1, book.getId());
            stmt.setString(2, book.getTitle());
            stmt.setString(3, book.getAuthor());
            stmt.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public List<Book> getAllBooks() {
        List<Book> books = new ArrayList<>();
        try (Connection conn = DBConnection.getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM books")) {
            while (rs.next()) {
                books.add(new Book(rs.getInt("id"), rs.getString("title"), rs.getString("author")));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return books;
    }
}

// 4. LibraryUI.java
package ui;

import dao.BookDAO;
import model.Book;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class LibraryUI extends JFrame {
    private JTextField idField, titleField, authorField;
    private BookDAO bookDAO = new BookDAO();

    public LibraryUI() {
        setTitle("Library Management System");
        setSize(400, 300);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new GridLayout(5, 2, 10, 10));

        add(new JLabel("Book ID:"));
        idField = new JTextField();
        add(idField);

        add(new JLabel("Title:"));
        titleField = new JTextField();
        add(titleField);

        add(new JLabel("Author:"));
        authorField = new JTextField();
        add(authorField);

        JButton addButton = new JButton("Add Book");
        addButton.addActionListener(e -> {
            int id = Integer.parseInt(idField.getText());
            String title = titleField.getText();
            String author = authorField.getText();
            bookDAO.addBook(new Book(id, title, author));
            JOptionPane.showMessageDialog(this, "Book Added Successfully");
        });
        add(addButton);

        JButton viewButton = new JButton("View Books");
        viewButton.addActionListener(e -> {
            java.util.List<Book> books = bookDAO.getAllBooks();
            StringBuilder sb = new StringBuilder();
            for (Book b : books) {
                sb.append(b.getId()).append(" - ").append(b.getTitle()).append(" by ").append(b.getAuthor()).append("\n");
            }
            JOptionPane.showMessageDialog(this, sb.toString());
        });
        add(viewButton);

        setVisible(true);
    }
}

// 5. Main.java
package main;

import ui.LibraryUI;

public class Main {
    public static void main(String[] args) {
        new LibraryUI();
    }
}

/*
SQL to create table in MySQL:
CREATE DATABASE library_db;
USE library_db;
CREATE TABLE books (
    id INT PRIMARY KEY,
    title VARCHAR(100),
    author VARCHAR(100)
);
*/

# Nodejs

const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();

// Connect to MongoDB (replace 'your-db-connection-string' with your actual MongoDB connection string)
mongoose.connect('mongodb://localhost:27017/signup-app', { useNewUrlParser: true, useUnifiedTopology: true });

// Create a mongoose schema for the User model
const userSchema = new mongoose.Schema({
    name: String,
    email: String,
    password: String
});

const User = mongoose.model('User', userSchema);

app.use(bodyParser.urlencoded({ extended: true }));
app.use(express.static('public')); // Serve static files from the 'public' directory

// Route to serve the signup form
app.get('/signup', (req, res) => {
    res.sendFile(__dirname + '/signup.html');
});

// Route to handle form submission
app.post('/signup', (req, res) => {
    const { name, email, password } = req.body;

    // Create a new User instance
    const newUser = new User({
        name: name,
        email: email,
        password: password
    });

    // Save the user to the database
    newUser.save((err) => {
        if (err) {
            res.send('Error occurred while signing up.');
        } else {
            res.send('Signup successful!');
        }
⬤
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Signup</title>
</head>
<body>
    <h1>Signup</h1>
    <form action="/signup" method="POST">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required><br>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required><br>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br>
        <button type="submit">Sign Up</button>
    </form>
</body>
</html>

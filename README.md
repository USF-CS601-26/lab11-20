# Cookies and Velocity Templates

In this exercise, you will create a Java web server using Jetty that uses **cookies** to remember a user's name and **Velocity** template engine to display an HTML form.

The server should support two endpoints:

**1. `/greet`**

When a client sends a GET request to `/greet`:

- If the request contains a cookie named `username`, display `Welcome, username!`, replacing `username` with the value stored in the cookie.
- If the cookie does not exist, display an HTML form asking the user to enter their name. Use the Velocity template `templates/form.html` to generate the form.
- When the user submits the form, the server should process the POST request, retrieve the name, clean the input, create a cookie named `username`, and add it to the HTTP response.
- Redirect the client back to `/greet`. The browser should now send the cookie, and the server should display the welcome message.

**2. `/clear`**

When a client sends a GET request to `/clear`:

- Delete the `username` cookie by setting its maximum age to 0.
- Display a message confirming that the cookie has been cleared.

### Tasks

**Task 1: Complete `GreetingServer.java`**

- Map `/greet` to `GreetingServlet`.
- Map `/clear` to `ClearCookieServlet`.
- Initialize a `VelocityEngine` and store it as an attribute of the servlet context so that `GreetingServlet` can access it.

**Task 2: Complete `templates/form.html`**

- Set the correct `method` and `action` attributes of the HTML form so that submitting it invokes the `doPost` method of `GreetingServlet`.
- Add a text input field with the name `name`.
- Add a submit button.

**Task 3: Complete `GreetingServlet.java`**

Implement `doGet`:

- Check whether the request contains a cookie named `username`.
- If it exists, retrieve its value and display a welcome message.
- Otherwise, use the Velocity template engine to load and display `templates/form.html`.

Implement `doPost`:

- Retrieve the `name` parameter from the submitted form.
- Clean the user input to prevent HTML/script injection.
- Create a cookie named `username` containing the name and add it to the response.
- Use `sendRedirect` to redirect the client to `/greet`.

**Task 4: Complete `ClearCookieServlet.java`**

- Implement `doGet` to clear the `username` cookie.
- Make sure the cookie being cleared has the same path as the cookie originally created.
- Display a message confirming that the cookie has been cleared.

### Testing

Run `GreetingServer` and test the following:

1. Open `http://localhost:9000/greet` in your browser. You should see a form asking for your name.
2. Enter your name and submit the form. You should be redirected to `/greet` and see a welcome message.
3. Refresh the page. The welcome message should still be displayed without asking for your name again.
4. Open `http://localhost:9000/clear`. You should see a confirmation message.
5. Return to `http://localhost:9000/greet`. The form should appear again because the cookie has been cleared.

You can also use your browser's Developer Tools to inspect the cookies and HTTP requests/responses.
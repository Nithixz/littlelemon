# Assignment: Little Lemon booking system

By working through the lessons in this course, you've learned the necessary skills and knowledge to develop the back-end for the booking form on the Little Lemon website. You were provided with code snippets and your task was to use these, plus any of your own code, to complete the full stack final assessment.

You will now take part in a peer-review in which you will submit your completed final assessment for two of your peers to review. You will also be required to review two of your peers' projects.

Detailed criteria are covered in the grading criteria overview below.

Grading criteria overview

When you submit your assignment, other learners in the course will review and grade your work. These are the criteria they’ll use to evaluate your submission.

1. Is the app added to the installed apps list in the settings file?
2. Is the database configuration updated inside the settings file?
3. Were migrations performed?
4. Are there three fields in the booking form: First name, Reservation date and Reservation slot?
5. Does a date selector open up when you click on the reservation date field on the booking form?
6. Are all the bookings available as JSON data on the reservations page?
7. Is duplicate booking prohibited on a specific date if the time is already booked?
8. Does changing the date refresh the booking data?
9. Is a duplicate booking on a specific date and time unavailable if the slot is already booked?
10. Can you display bookings for a specific date using the API?
11. If there is no booking, does a **No Booking** message show for that date?
12. Was fetch API used to retrieve data from the API?
13. Is the current date automatically selected when you open the booking form?

Example submission

![image.png](image.png)

![image.png](image%201.png)

![image.png](image%202.png)

![image.png](image%203.png)

How to create your submission

You need to use **pipenv** to create the virtual environment and to manage the dependencies.

You are required to use the Django project for your back-end, MySQL as your database and JavaScript to handle form components of the template. HTML with supportive stylesheets are already added as part of the starter code. You also received the code for a functional Little Lemon website as part of the Django project starter code.

Name the project directory **littlelemon** and the app **restaurant**. Ensure that you follow this naming convention to make the reviewing process easier.

You will be required to submit your updated Django project in a zipped folder.

**Important note: ***There may be variations in terms of the username and password set for the MySQL user depending on your peer's local machine. Be mindful that the MySQL database that you will be accessing is local to your machine.*
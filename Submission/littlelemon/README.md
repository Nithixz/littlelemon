# Little Lemon booking system

This is a Django project named `littlelemon` with an app named `restaurant`. It stores bookings in MySQL, and the booking page uses JavaScript.

## Requirements

- **Python 3.8 or newer.** The Pipfile doesn't pin a Python version, so `pipenv install` uses your Python and installs the newest Django that supports it. The project has been tested with Django 4.2, 5.2 and 6.1.
- **MySQL** running on `127.0.0.1:3306`.
- **pipenv**

## Setup

1. Create the database in MySQL:

   ```sql
   CREATE DATABASE reservations;
   ```

2. **Change the MySQL login to your own.** Open `littlelemon/settings.py` and set `USER` and `PASSWORD` under `DATABASES`. They are currently `admindjango` / `employee@123!`.

3. Install the dependencies, run the migrations and start the server:

   ```bash
   pipenv install
   pipenv shell
   python manage.py migrate
   python manage.py runserver
   ```

## Where to check each grading criterion

| Page | URL | What to check |
| --- | --- | --- |
| Booking form | http://127.0.0.1:8000/book/ | The three fields, the date picker, today's date preselected, booked slots disabled, the list refreshing when the date changes, and "No bookings" for an empty date |
| All reservations (JSON) | http://127.0.0.1:8000/reservations/ | Every booking, shown as JSON |
| Bookings API | http://127.0.0.1:8000/bookings?date=YYYY-MM-DD | That date's bookings as JSON. Booking a slot that is already taken returns `{'error':1}` |

Code to review:

- `restaurant/views.py`: the `bookings()` view (the API)
- `restaurant/templates/book.html`: the booking page script (`fetch`)

# Little Lemon booking system

This is a Django project named `littlelemon` with an app named `restaurant`. It stores bookings in MySQL, and the booking page uses JavaScript.

## Requirements

- **Python 3.12 or newer.** The Pipfile asks for 3.14. The project uses Django 6.1, which needs Python 3.12+. If you have a different 3.12+ version, change `python_version` in `Pipfile`.
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

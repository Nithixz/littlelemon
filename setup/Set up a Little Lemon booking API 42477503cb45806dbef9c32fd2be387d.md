# Set up a Little Lemon booking API

## **Lab assets**

You are provided with a basic setup for the Django environment for the project and app inside the zip file below.

[Set-up-the-Little-Lemon-booking-API.zip](Set-up-the-Little-Lemon-booking-API.zip)

The zip has been extracted to the folder **[C7M5L1-6 Lab Initial](C7M5L1-6%20Lab%20Initial/)**. This is the working directory for the lab (it contains **manage.py**). Run all commands from inside it:

```bash
cd "C7M5L1-6 Lab Initial"
```

The zip file contains an updated template to be used along with supportive files to run the lab.

**Note:** *The virtual environment using **pipenv** is defined by the [Pipfile](C7M5L1-6%20Lab%20Initial/Pipfile) and [Pipfile.lock](C7M5L1-6%20Lab%20Initial/Pipfile.lock) in the extracted folder. The virtual environment itself is not included. You must create and activate it and make sure the dependencies are installed.*

### **Extracted project structure**

```
C7M5L1-6 Lab Initial/
├── manage.py
├── Pipfile                     # django, mysqlclient; python_version = "3.9"
├── Pipfile.lock
├── db.sqlite3                  # leftover from earlier labs, not used (settings point to MySQL)
├── littlelemon/                # project
│   ├── settings.py             # MySQL config, templates and static dirs (starter code)
│   └── urls.py                 # includes restaurant.urls (starter code)
├── restaurant/                 # app
│   ├── models.py               # Booking and Menu models (starter code)
│   ├── views.py                # partially complete: book() commented out, bookings() to add
│   ├── urls.py                 # book/ and bookings/ routes commented out
│   ├── admin.py                # registers Menu
│   ├── migrations/             # 0001–0003 already present
│   ├── forms.py                # ← YOU CREATE THIS (Step 3)
│   ├── static/                 # css/style.css, img/
│   └── templates/
│       ├── base.html
│       ├── index.html          # "Add code here for book" placeholder
│       ├── book.html           # empty <h1> and <script>
│       ├── bookings.html       # empty <h1> and <script>, <pre id="bookings">
│       ├── about.html, menu.html, menu_item.html
│       └── partials/
│           ├── _header.html    # placeholders for Book / Reservations nav links
│           └── _footer.html
└── static/img/menu_items/
```

Your goal is to set up an API endpoint that saves form data to a MySQL database and returns a JSON object to display on the web page.

### **Objectives**

Create a view to process form data entered in a Django template

Convert form data received from a **POST** method into a JSON object and return to a web page

## **Introduction**

In part one of the assessment, you set up a MySQL database, which is far more secure and scalable than the SQLite database used earlier. In part two of the final assessment, you will add the booking feature to the existing website.

## **Initial lab instructions**

This lab will require you to modify the following files:

- [restaurant/views.py](C7M5L1-6%20Lab%20Initial/restaurant/views.py)
- **restaurant/forms.py** (new file)
- [restaurant/urls.py](C7M5L1-6%20Lab%20Initial/restaurant/urls.py) (app-level)
- [restaurant/templates/bookings.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/bookings.html)
- [restaurant/templates/book.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/book.html)
- [restaurant/templates/index.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/index.html)
- [restaurant/templates/partials/_header.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/partials/_header.html)

Starter code has already been added to the following files:

- [littlelemon/settings.py](C7M5L1-6%20Lab%20Initial/littlelemon/settings.py)
- [restaurant/models.py](C7M5L1-6%20Lab%20Initial/restaurant/models.py)
- [restaurant/urls.py](C7M5L1-6%20Lab%20Initial/restaurant/urls.py) (app-level)
- [littlelemon/urls.py](C7M5L1-6%20Lab%20Initial/littlelemon/urls.py) (project-level)
- [restaurant/views.py](C7M5L1-6%20Lab%20Initial/restaurant/views.py) (partially complete)

**Note:** *Once you set up the project, review the contents of all the files and then follow the steps. The zip file may contain some additional stylesheets and formatting, so use the extracted folder as the starter code instead of building on your existing project from part one.*

You have already built the project named **littlelemon** and added an app inside the project called **restaurant**.

Follow the instructions below and check the output after every step.

**Note:** *Make sure you have installed MySQL on your local machine and set up the admin user. To keep this exercise simple, you will start with the root user. By default, the root user's password is either **password** or blank. If the password is blank, press **Enter**. The password does not appear on screen as you type it.*

### **Starter code notes (from the extracted files)**

- **Database settings** in [settings.py](C7M5L1-6%20Lab%20Initial/littlelemon/settings.py) are already set to MySQL:

  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'reservations',
          'HOST' : '127.0.0.1',
          'PORT' : '3306',
          'USER' : 'root',
          'PASSWORD' : 'root@123',
      }
  }
  ```

  The database name is **reservations** and the password is **root@123**, not the default password mentioned above. Either create a `reservations` database and use that root password, or change `NAME`, `USER` and `PASSWORD` to match your local MySQL setup.

- **Booking model** in [models.py](C7M5L1-6%20Lab%20Initial/restaurant/models.py) has these fields: `first_name` (CharField), `reservation_date` (DateField) and `reservation_slot` (SmallIntegerField, default 10). The **Menu** model has `name`, `price` and `menu_item_description`.
- **Migrations** `0001`–`0003` already exist in `restaurant/migrations/`, so `makemigrations` should report no changes unless you edit the models.
- **admin.py** registers only `Menu`. `Booking` is imported but not registered. This is optional, but you can add `admin.site.register(Booking)` to see bookings in the Django admin.
- **Pipfile** asks for Python **3.9**. If you have a different Python version, run `pipenv --python 3` (or edit `python_version`) before you install.

This is part two of the final assessment. It uses the Django framework without any JavaScript. You will first create a model form from the model you created earlier and then update the form with some data. You will then process the form data and send it to the front end as a JSON object.

# **Steps**

**Step 1:**

Run the following command to activate the virtual environment:

**pipenv shell**

**Note:** *Run the command in the main working directory that contains the **manage.py** file, which is `C7M5L1-6 Lab Initial/`.*

**Step 2:**

To make sure you have the necessary dependencies in place, run the following commands inside pipenv:

**pipenv install django**

**pipenv install mysqlclient**

**Note:** *If you have problems with the virtual environment, you can try running the project without **pipenv**. On macOS, `mysqlclient` needs the MySQL client libraries to build (for example, `brew install mysql pkg-config`).*

**Step 3:**

Create a file called **forms.py** in the **restaurant** directory (`C7M5L1-6 Lab Initial/restaurant/forms.py`) and import the following:

- The **Booking** model from the file **models.py**.
- The module **forms** from the package **django**

**Step 4:**

Still inside the **forms.py** file, create a class called **BookingForm** and pass **forms.ModelForm** to it as an argument.

**Step 5:**

Inside the class **BookingForm**, create another class called **Meta** and declare the following attributes inside it:

- **model** with the value **Booking**
- **fields** with the value **"\_\_all\_\_"**

**Note:** *Django requires the attribute names to be lowercase: `model` and `fields` (plural).*

**Step 6:**

In the terminal, run both migration commands:

**python manage.py makemigrations**

**python manage.py migrate**

**Note:** *Before you run the migrations, make sure you have created the correct MySQL user, assigned privileges, and created the database named in settings.py (**reservations**).*

Run the necessary commands to make sure the user is created and can access the database, for example:

```sql
CREATE DATABASE reservations;
```

**Step 7:**

Open the file [views.py](C7M5L1-6%20Lab%20Initial/restaurant/views.py) and look at the code for the different views on the Little Lemon website. This functionality was added using only the Django framework.

Uncomment the **book()** view function.

Also uncomment the import statement below:

**# from .forms import BookingForm**

Look at the **book()** function. It accepts the values entered in the form that is rendered on the **book.html** page.

**Note:** *The necessary imports (`serializers`, `Booking`, `datetime`, `json`) are already in the **views.py** file. Add the new view where the comment `# Add code for the bookings() view` appears.*

**Step 8:**

Create a view function called **bookings()** that takes the request object as an argument. Follow the pseudo code below to add the code that processes the form data.

Inside the **bookings()** view function, add the following pseudo code:

- Create a variable called **date** and assign it the following value:
    - **request.GET.get('date',datetime.today().date())**
- Create a variable called **bookings** and assign it the following value:
    - Call **objects.all()** on the **Booking** class object.
- Create a variable called **booking_json** and assign it the following value:
    - **serialize()** function called over **serializers**. Pass the following arguments to the **serialize()** function
        - **'json'**
        - **bookings**
- Return the **render()** function and pass the following arguments to it:
    - **request**
    - **"bookings.html"** as a string
    - dictionary containing following key-value pairs:
        - Key: **"bookings"**
        - Value: **booking_json**

Save the **views.py** file and make sure the code has no errors.

**Step 9:**

Go to the app-level [urls.py](C7M5L1-6%20Lab%20Initial/restaurant/urls.py) file and uncomment the URL configuration for the view functions below:

- **bookings**: `# path('bookings/', views.bookings, name="bookings"),`
- **book**: `# path('book/', views.book, name="book"),`

**Step 10:**

Open the file [book.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/book.html) in the templates folder and update the code as described below.

Add a heading using the **<h1>** tag that says **Make a reservation**. Replace the `<h1> <!--Add title here --> </h1>` placeholder.

**Step 11:**

Go to the **<script>** tags at the bottom of the file (below `<!-- Add your code inside the script tags below -->`) and add the following pseudo code to update the **book.html** file.

- Call the log() function on **console** and pass the argument **"Hello"** inside it
- Call the function **getElementById()** on the **document** and pass the string **"id_reservation_date"** to it as an argument.
    - Add **.type = "date"** to the end of that call, so the field becomes a date picker.

**Step 12:**

Now open the [bookings.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/bookings.html) file in the templates folder and update the code as described below.

Add a heading using the **<h1>** tag that says **All Reservations**. Replace the `<h1><!--Add title here --></h1>` placeholder.

**Step 13:**

Go to the **<script>** tags and add the following pseudo code to update the **bookings.html** file.

- Create a constant called **bookings** and assign it the value of **parse()** function called over **JSON**. Pass the following argument inside the **parse()** function:
    - **'{{ bookings|safe }}'**
- Call the **log()** function on console and pass **bookings** to it as an argument.
- Create a constant called **pretty_json** and assign it the value of:
    - **stringify()** function called over JSON and pass the following arguments to it:
        - **bookings**
        - **null**
        - **2**

**Note:** *Look at the code in the **bookings.html** file and save it. Make sure there are no errors in your code. The template contains a `<pre id="bookings"></pre>` element. To show the JSON on the page, you can also add `document.getElementById("bookings").innerHTML = pretty_json;` at the end of the script.*

**Step 14:**

Open the file [index.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/index.html) and look for the comment, which is in the **Book a table** article:

- **<!-- Add code here for book -->**

Replace the comment with the following code:

- **<a href="{% url 'book' %}">Book your table now</a>**

**Step 15:**

Open the file [_header.html](C7M5L1-6%20Lab%20Initial/restaurant/templates/partials/_header.html) inside the `templates/partials` folder and look for the comments:

- **<!-- Add code here for the book template -->**
- **<!-- Add code here for the reservations template -->**

Replace the comment with the following code:

- **<li><a href="{% url 'book' %}">Book</a></li>**
- **<li><a href="{% url 'bookings' %}">Reservations</a></li>**

**Step 16:**

Run the server and go to the localhost URL:

**python manage.py runserver**

Then open http://127.0.0.1:8000/

**Step 17:**

Go to the **Book** tab and add three entries in the form.

**Step 18:**

After you add the entries, go to the **Reservations** tab in the menu bar. You will see the new entries there.

**Note:** *This is the same data that was saved in the Little Lemon website's database. You can check it in MySQL (for example, `SELECT * FROM reservations.restaurant_booking;`).*

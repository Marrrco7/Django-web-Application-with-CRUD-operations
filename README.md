

Overview
--------

Dynamic web application developed using Django, allowing for easy addition, update, display, and deletion of game information. The application integrates with a PostgreSQL database to efficiently store and retrieve game data for a videogames company (Threading Labs).

Table of Contents
-----------------
- [Project Structure](#project-structure)
- [Features](#features)
- [Functionalities](#functionalities)
- [Analysis and Design](#analysis-and-design)
- [System Architecture](#system-architecture)
- [Security](#security)
- [Setup and Deployment](#setup-and-deployment)

## Project Structure
-----------------


## Features
--------

- **CRUD operations**: Create, Read, Update, and Delete video game entries.
- **User Authentication**: Secure login and logout functionalities for staff members.
- **Permissions**: Restrict access to certain views only to users with certain permissions.
- **Responsive Design**: Easy to use UI.

### Functionalities

- **Game Adding**: Users can add new video games by filling the form rendered by the views.py
- **Details Editing**: Users can update the existing game information.
- **Game Deletion**: Possibility to easily remove games from the catalog (with permission required).
- **View Game List**: Users can view a list of all games in the catalog.
- **User Authentication**: Users must log in to access the application.
- **Permissions**: Only authorized users can add, edit, or delete games (Admins and managers).
- **Security**: Implement proper authentication and authorization mechanisms (CSRF tokens, loggin authentication).
- **Usability**: UI intuitive and easy to navigate, designed for staff memebers with non technical backround.
  

## Analysis and Design
-------------------

### System Architecture

![image](https://github.com/user-attachments/assets/496a4afa-7767-429f-871e-cc012faa441a)

The architecture is composed by these following:

- **Web Application**: The project is build as a Django webApp that serves as a backend. It handles data validation, business logic
  and HTTP requests by the users thanks to the Django MVT architecturarl pattern:
  
  - **Models**: Define the structure of the database (VideoGame and Genre models).
  - **Views**: Handle the business logic and provides posibility to interact with the models.
  - **Templates**: Render the user interface using HTML and Bootstrap4 for a cooler look.

- **Database**: The project uses a relational postgre SQL database for data storage and efficient retrieving of information. The Django App interacts with this database
  via the ORM (Object Relational Mapping), making it easier to perform CRUD operations. The columns of the database are defined in the Django models.py: Title, Release date, Description,
  and Genre. The database design is composed by two tables: Videogame and Genre, implemented with a one to many relationship (a genre can have multiple games, but each genre's game must be only one).

**Tables**:

1. **Genre**
   - `id`: Primary key (automatically generated by Django)
   - `title`: String (50 chars)

2. **VideoGame**
   - `id`: Primary key 
   - `title`: String (100 chars)
   - `release_date`: Date
   - `description`: String (100 chars)
   - `genre_id`: Foreign key pointing to  the `Genre` table
  
  
- **Local Environment**: The application is running in a local server allowing staff members to access the WebApp by connecting to the IP address of the local machine.

## Security

The Django WebApp project includes several security measures to ensure reliabilty and safey for user control:

- **User Authentication**: Only authenticated users can access the application. Django’s built in authentication system is used to handle login and session management securely.
  
- **Role Based Permissions**: Access to specific functionalities is restricted based on user permissions. Users with certain roles (for example Admin, Manager) have superior access to specific areas, ensuring that only authorized users can for instance delete data.

- **CSRF Protection**: Django’s CSRF "Cross-Site Request Forgery" protection is enabled to prevent unauthorized actions that can come from malicious sources. All forms include CSRF tokens, adding an additional layer of security for the submitted data from the users.

- **Password Hashing by Django**: Django securely hashes user passwords using a strong hashing algorithm, which is PBKDF2 by default, before storing them in the database. This ensures that even if the database is compromised, passwords are not stored as plain text.

- **Environment Variables for Sensitive Data**: Sensitive information, such as database credentials, are stored in environment variables, a (`credentials.env` file) and not hardcoded in the source code to prevent exposure of our credentials. 

All this securuty measures were applied following best practices and ensure user validation, access, and a robust functionality of the web application.



## Setup and Deployment Steps

This section shows the steps to set up, test, and deploy the application into a local server, that in the live demo was my machine.

### 1. Database Migrations
   - After creating or making changes to the Django models, it is necessary make and apply migrations to update the database schema accordingly.
   - Run the following commands to make and apply migrations:
     ```bash
     python manage.py makemigrations
     python manage.py migrate
     ```
     -This will ensure us of having the latest database model in our database.

### 2. Running Tests
   - To guarantee that the application works as expected, run tests defined in `tests.py`. The tests are written to ensure correct setup, catalog visualisation, form submission, videogame creation.
   - To run all the tests, run the command:
     ```bash
     python manage.py test
     ```
   - Check the output to see if all the tests passed.

### 3. Using Waitress for Deployment
   - For production deployment, we are using Waitress, a WSGI server that is recommended for Windows.
   - First, install Waitress if you haven’t already, just run:
     ```bash
     pip install waitress
     ```

### 4. Running the Server with Waitress
   - Then, create a new file `run_waitress_server.py` to be able to run the server. Here is an example of how the code might look, just adjust the host and port accordingly:
     
     ```python
     # run_waitress_server.py
     from waitress import serve
     from my_project.wsgi import application

     if __name__ == "__main__":
         serve(application, host="0.0.0.0", port=8000)
     ```
   - Run `run_waitress_server.py` using:
     ```bash
     python run_waitress_server.py
     ```
   - This will start the application on `http://0.0.0.0:8000`, or the specific IP address of your local machine, making it accessible on the local network.

### 5. Testing the Deployment
   - Access the WebApp at `http://serverIP:8000` from other devices on the network to verify functionality and stability.

### 6. Setting the environmental variables for the database credentials

In order to hash the sensitive data for connecting to our database, environmental variables where used wiht the `django environ` package:

To install just run:

```bash
pip install django-environ
```

After that, create an env. file with the credentials to connect to the database, somehting like this:

`DB_NAME=your_database_name
DB_USER=your_database_user
DB_PASSWORD=your_database_password
DB_HOST=your_database_host
DB_PORT=your_database_port`

then update the settings.py to use the environmental variables, it might look like:
`DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': env('DB_NAME'),
        'USER': env('DB_USER'),
        'PASSWORD': env('DB_PASSWORD'),
        'HOST': env('DB_HOST', default='localhost'),  
        '    
    }
}`

Note: the credentials.env file was added to the gititgnore in this repository for obvious reasons, so the credentials are not visible for everyone.

-After following this proccess we would have enabled a better way of securing our sensitve data without havind hardcoded the credentials.



### User Interface Design
- **Base Template**: Includes navigation and common UI elements.
- **Videogame List**: Displays a table of all video games with options to edit or delete.
- **Videogame Form**: Form for adding or editing a video game.
- **Authentication Pages**: Login and logout pages.

**Design Elements**:

- **Bootstrap 4.3.1**: Used for responsive design and styling (includes the jumbotron component).
- **Font Awesome 6.6.0**: Provides icons for buttons and actions.
- **Jumbotron Component**: Used for the header section in the base template.
- **Base Template**: Includes navigation and most common UI elements.
- **Videogame List**: Extends Base.html, displays a table of all video games with options to edit or delete.
- **Videogame Form**: Extends Base.html, form for adding, deleting or updating videogames

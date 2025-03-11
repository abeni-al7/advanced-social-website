# Advanced Social Website

This project is an advanced social website built with Django, providing features for user authentication, image bookmarking, social interactions, and activity tracking.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Apps](#apps)
- [Settings](#settings)
- [Authentication](#authentication)
- [Social Authentication](#social-authentication)
- [Image Bookmarking](#image-bookmarking)
- [Activity Tracking](#activity-tracking)
- [Asynchronous Tasks (Future)](#asynchronous-tasks-future)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## Features

- **User Authentication:**
  - User registration and login ([`account.forms.UserRegistrationForm`](account/forms.py), [`account.views.register`](account/views.py), [`account.views.user_login`](account/views.py))
  - Password reset functionality (Django's built-in authentication views)
  - Profile editing ([`account.forms.UserEditForm`](account/forms.py), [`account.forms.ProfileEditForm`](account/forms.py), [`account.views.edit`](account/views.py))

- **Social Features:**
  - Following/unfollowing users ([`account.models.Contact`](account/models.py), [`account.views.user_follow`](account/views.py))
  - User activity feed on the dashboard ([`account.views.dashboard`](account/views.py), [`actions.models.Action`](actions/models.py))
  - User list and detail views ([`account.views.user_list`](account/views.py), [`account.views.user_detail`](account/views.py))

- **Image Bookmarking:**
  - Bookmark images from other websites ([`images.forms.ImageCreateForm`](images/forms.py), [`images.views.image_create`](images/views.py))
  - Image liking ([`images.views.image_like`](images/views.py))
  - Displaying bookmarked images in user profiles and lists ([`images.models.Image`](images/models.py), [`images.views.image_list`](images/views.py), [`images.views.image_detail`](images/views.py))
  - Image ranking based on views ([`images.views.image_ranking`](images/views.py))

- **Social Authentication:**
  - Google OAuth2 integration ([`social_django`](bookmarks/settings.py))

- **Activity Tracking:**
  - Tracking user actions (e.g., creating an account, bookmarking an image, liking an image, following a user) ([`actions.models.Action`](actions/models.py), [`actions.utils.create_action`](actions/utils.py))

## Requirements

- Python 3.10+
- Django 5.0+
- Redis
- Other Python packages (see `requirements.txt`)

## Installation

1.  Clone the repository:

    ```bash
    git clone https://github.com/abeni-al7/advanced-social-website.git
    cd advanced-social-website
    ```

2.  Create a virtual environment:

    ```bash
    python -m venv .venv
    ```

3.  Activate the virtual environment:

    *   On Windows:

        ```bash
        .venv\Scripts\activate
        ```

    *   On macOS and Linux:

        ```bash
        source .venv/bin/activate
        ```

4.  Install the dependencies:

    ```bash
    pip install -r requirements.txt
    ```

5.  Apply migrations:

    ```bash
    python manage.py migrate
    ```

## Configuration

1.  Create a `.env` file in the project root directory.

2.  Add the following environment variables to the `.env` file:

    ```
    SECRET_KEY=your_secret_key
    GOOGLE_OAUTH2_KEY=your_google_oauth2_key
    GOOGLE_OAUTH2_SECRET=your_google_oauth2_secret
    ```

    *   Replace `your_secret_key` with a strong, randomly generated secret key.
    *   Obtain Google OAuth2 credentials from the Google Cloud Console and replace `your_google_oauth2_key` and `your_google_oauth2_secret` with your actual credentials.

3. Configure Redis:
    * Ensure Redis server is running.
    * Update Redis settings in `bookmarks/settings.py` if needed.

## Usage

1.  Run the development server:

    ```bash
    python manage.py runserver
    ```

2.  Open your web browser and navigate to `http://localhost:8000/`.

3.  Create a superuser account:

    ```bash
    python manage.py createsuperuser
    ```

    *   Follow the prompts to create an administrative user.

## Project Structure

. ├── account/ # User account app ├── actions/ # Activity tracking app ├── bookmarks/ # Main project directory ├── images/ # Image bookmarking app ├── manage.py # Django management script ├── media/ # Media files (uploaded images) ├── requirements.txt # Project dependencies └── static/ # Static files (CSS, JavaScript)

## Apps

### `account`

*   Manages user accounts, authentication, and profiles.
*   **Models:** [`Profile`](account/models.py), [`Contact`](account/models.py)
*   **Views:** [`dashboard`](account/views.py), [`register`](account/views.py), [`edit`](account/views.py), [`user_list`](account/views.py), [`user_detail`](account/views.py), [`user_follow`](account/views.py)
*   **Forms:** [`LoginForm`](account/forms.py), [`UserRegistrationForm`](account/forms.py), [`UserEditForm`](account/forms.py), [`ProfileEditForm`](account/forms.py)
*   **Authentication:** [`EmailAuthBackend`](account/authentication.py)
*   **Signals:** [`create_user_profile`](account/signals.py)

### `actions`

*   Tracks user activities.
*   **Model:** [`Action`](actions/models.py)
*   **Utility functions:** [`create_action`](actions/utils.py)

### `images`

*   Handles image bookmarking and liking.
*   **Model:** [`Image`](images/models.py)
*   **Views:** [`image_create`](images/views.py), [`image_detail`](images/views.py), [`image_like`](images/views.py), [`image_list`](images/views.py), [`image_ranking`](images/views.py)
*   **Forms:** [`ImageCreateForm`](images/forms.py)
*   **Signals:** [`users_like_changed`](images/signals.py)

### `bookmarks`

*   Main project configuration.
*   **Settings:** [`settings.py`](bookmarks/settings.py)
*   **URLs:** [`urls.py`](bookmarks/urls.py)

## Settings

Key settings defined in [`bookmarks/settings.py`](bookmarks/settings.py):

*   `SECRET_KEY`:  A secret key for the Django installation.
*   `DEBUG`:  A boolean that determines whether Django runs in debug mode.
*   `ALLOWED_HOSTS`:  A list of strings representing the host/domain names that this Django instance can serve.
*   `DATABASES`:  A dictionary containing the settings for the database.
*   `STATIC_URL`:  The URL to use when referring to static files.
*   `MEDIA_URL`:  The URL to use when referring to media files.
*   `MEDIA_ROOT`:  The absolute path to the directory where media files are stored.
*   `LOGIN_REDIRECT_URL`:  The URL to redirect to after a successful login.
*   `AUTHENTICATION_BACKENDS`:  A list of authentication backends to use.
*   `SOCIAL_AUTH_GOOGLE_OAUTH2_KEY`: Google OAuth2 client ID.
*   `SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET`: Google OAuth2 client secret.
*   `ABSOLUTE_URL_OVERRIDES`: Overrides `get_absolute_url` for specific models.
*   `REDIS_HOST`, `REDIS_PORT`, `REDIS_DB`: Redis connection settings.

## Authentication

The project uses Django's built-in authentication system, with the addition of an email authentication backend ([`account.authentication.EmailAuthBackend`](account/authentication.py)).  This allows users to log in using their email address instead of a username.

## Social Authentication

The project integrates with Google OAuth2 for social authentication using the `social-auth-app-django` package.  The `SOCIAL_AUTH_GOOGLE_OAUTH2_KEY` and `SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET` settings are used to configure the Google OAuth2 backend.  The `SOCIAL_AUTH_PIPELINE` setting defines the pipeline of functions that are executed during the social authentication process.

## Image Bookmarking

Users can bookmark images from other websites using a bookmarklet.  The bookmarklet is a small piece of JavaScript code that can be saved as a bookmark in a web browser.  When the bookmarklet is clicked, it opens a popup window that allows the user to select an image from the current page and save it to their account.  The [`images`](images/) app handles the image bookmarking functionality.

## Activity Tracking

The project tracks user activities using the [`actions`](actions/) app.  Whenever a user performs an action (e.g., creating an account, bookmarking an image, liking an image, following a user), an `Action` object is created and saved to the database.  These actions are displayed in the user's activity feed on the dashboard.

## Asynchronous Tasks (Future)

*   Celery integration for handling asynchronous tasks such as:
    *   Sending email notifications
    *   Processing large image uploads
    *   Updating user activity feeds

## Testing

To run the tests, use the following command:

```bash
python manage.py test
```

## Deployment
1. Collect static files:

2. Configure a production web server (e.g., Nginx, Apache) to serve the static files and proxy requests to the Django application.

3. Use a process manager (e.g., Supervisor, systemd) to manage the Django application process.

4. Configure a production database (e.g., PostgreSQL, MySQL).

5. Set the DEBUG setting to False in settings.py.

6. Ensure all sensitive information is stored in environment variables.

## Contributing
Contributions are welcome! Please follow these steps:

1. Fork the repository.

2. Create a new branch for your feature or bug fix.

3. Implement your changes.

4. Write tests for your changes.

5. Run the tests to ensure that your changes are working correctly.

6. Commit your changes.

7. Push your changes to your fork.

8. Submit a pull request.
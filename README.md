# Advanced Social Media Website
This repository contains an advanced social website built with Django. It supports user profiles, following other users, and uploading photos. The project is organized into multiple apps—such as account, actions, bookmarks, and images—to keep functionalities separate and maintain clean architecture.

## Features
- Custom user profile
- Following system
- User registration and authentication
- Activity stream: Tracking user actions and logging them.

## Usage
1. Clone the repo
```bash
git clone https://github.com/abeni-al7/advanced-social-website.git
```
2. Create a virtual environment and activate it
```bash
python3 -m venv .venv
source .venv/bin/activate
```
3. Install dependencies
```bash
pip install -r requirements.txt
```
4. Run database migrations
```bash
python manage.py migrate
```
5. Start the development server
```bash
python manage.py runserver
```
6. Access the site at [http://127.0.0.1:8000]http://127.0.0.1:8000
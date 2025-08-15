# Laravel Project Installation Guide

This guide provides step-by-step instructions for installing and running the Laravel project from a Git repository.

## Prerequisites

Before you begin, ensure you have the following installed on your system:

*   PHP (7.4 or higher recommended)
*   Composer
*   Node.js and npm or yarn
*   Git

## Installation Steps

Follow these steps to get the project up and running:

### 1. Clone the Repository

Open your terminal or command prompt and clone the project repository from Git:

```
bash
git clone <repository_url>
```
Replace `<repository_url>` with the actual URL of your Git repository.

Navigate into the cloned project directory:
```
bash
cd <project_directory>
```
Replace `<project_directory>` with the name of the directory created by cloning the repository.

### 2. Install PHP Dependencies

Install the project's PHP dependencies using Composer:
```
bash
composer install
```
### 3. Create the .env File

Copy the example environment file to create your own `.env` file:

```
bash
cp .env.example .env
```
### 4. Configure the .env File

Open the newly created `.env` file in a text editor. You will need to configure your database credentials and other application settings. At a minimum, update the following lines:
```
dotenv
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_database_user
DB_PASSWORD=your_database_password
```
Replace `your_database_name`, `your_database_user`, and `your_database_password` with your actual database details.

### 5. Generate Application Key

Generate a unique application key. This key is used by Laravel's security features:
```
bash
php artisan key:generate
```
### 6. Run Database Migrations

If your project uses a database, run the migrations to create the necessary tables:
```
bash
php artisan migrate
```
If you also have seeders to populate the database with initial data, you can run them with:
```
bash
php artisan db:seed
```
Or, to run migrations and seeders together:
```
bash
php artisan migrate --seed
```
### 7. Install Frontend Dependencies

Install the project's Node.js dependencies using npm or yarn:
```
bash
npm install
# OR
yarn install
```
### 8. Build Frontend Assets

Compile the project's frontend assets:
```
bash
npm run dev
# OR
yarn dev
```
For production, you can use:
```
bash
npm run build
# OR
yarn build
```
### 9. Start the Local Development Server

Finally, start the Laravel development server:
```
bash
php artisan serve
```
This will typically start the server at `http://127.0.0.1:8000`. You can access your application in a web browser at this address.

Alternatively, you can configure a web server like Apache or Nginx to serve the application. Consult the Laravel documentation for detailed instructions on web server configuration.

## Troubleshooting

*   **Permissions:** Ensure the `storage` and `bootstrap/cache` directories have the correct write permissions for your web server.
*   **Composer Errors:** If you encounter Composer errors, try running `composer clear-cache` and then `composer install` again.
*   **Database Connection:** Double-check your database credentials in the `.env` file.
*   **Missing Dependencies:** If you receive errors about missing classes, ensure you have run `composer install` and `npm install` (or `yarn install`).
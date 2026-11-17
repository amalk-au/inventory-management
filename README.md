# Inventory Management System

A web-based inventory management application built with Node.js, Express and PostgreSQL, designed to manage product information, stock quantities, pricing and customer reviews.

## Table of Contents

- [Overview](#overview)

- [Features](#features)

- [Screenshots](#screenshots)

- [Live Demo](#live-demo)

- [Tech Stack](#tech-stack)

- [Architecture](#architecture)

- [Database Design](#database-design)

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)

  - [Installation](#installation)

  - [Environment Variables](#environment-variables)

  - [Database Setup](#database-setup)

- [Project Structure](#project-structure)

- [Available Scripts](#available-scripts)

- [Development](#development)

- [Testing](#testing)

- [Deployment](#deployment)

- [Troubleshooting](#troubleshooting)

- [Contributing](#contributing)

- [License](#license)

- [Acknowledgements](#acknowledgements)

- [Contact](#contact)

---

## Overview

The Inventory Management System is a web application designed to simplify the management of product information and inventory.

Built using Node.js, Express.js and PostgreSQL, the application provides a relational database structure for organising product details and their corresponding inventory records.

The application separates product information from inventory data, making it easier to manage product categories, manufacturers, descriptions, stock quantities, prices and customer reviews.

The project demonstrates backend development concepts, including RESTful-style CRUD operations, relational database design, SQL queries, server-side rendering and database management.

## Features

- **Product Management:** Create, retrieve, update and delete product information.

- **Inventory Management:** Manage stock quantities, product prices and customer reviews.

- **Relational Database:** Separate product information and inventory into related PostgreSQL tables.

- **Product Categorisation:** Organise products into categories such as watches and phones.

- **Product Information:** Store manufacturer details, model numbers, descriptions, external product links and product images.

- **Database Relationships:** Maintain relationships between products and their inventory records using foreign key constraints.

- **Optimised SQL Queries:** Use parameterised queries through the native PostgreSQL `pg` library.

- **Cascade Deletion:** Automatically remove associated inventory records when a product is deleted.

## Screenshots

### Main View

![main-view](./docs/screenshots/main-view.png)

![main-view](./docs/screenshots/view-product.png)

### Admin View

![admin-view](./docs/screenshots/admin-view.png)

### Inventory Management

#### Update product information

![update-product](./docs/screenshots/update-product.png)

#### Add new product product information

![add-new-product](./docs/screenshots/add-new-product.png)

## Tech Stack

| Category                  | Technology            |
| ------------------------- | --------------------- |
| Runtime                   | Node.js               |
| Backend                   | Express.js            |
| Language                  | JavaScript (CommonJS) |
| Templating Engine         | EJS                   |
| Database                  | PostgreSQL            |
| Database Client           | Native `pg` library   |
| Environment Configuration | dotenv                |
| Package Manager           | pnpm                  |
| Deployment                | Render, Koyeb         |

### Dependencies

| Package | Purpose                            |
| ------- | ---------------------------------- |
| express | Web application framework          |
| pg      | PostgreSQL database connectivity   |
| ejs     | Server-side HTML templating        |
| dotenv  | Environment variable configuration |

## Architecture

The application follows a server-side rendered architecture using Express.js and EJS.

The main components are:

1.  **Express Application:** Initialises the server, configures middleware and registers application routes.

2.  **Routes:** Handle incoming HTTP requests and direct them to the appropriate controllers.

3.  **Controllers:** Implement application logic and interact with the PostgreSQL database.

4.  **Database:** Stores product information and inventory records in relational tables.

5.  **Views:** Use EJS templates to render application pages.

6.  **Public Assets:** Serve static resources such as stylesheets, images and client-side assets.

### Application Flow

```
              Client / Browser
                     |
                     v
               Express Server
                     |
                     v
               Application Routes
                     |
                     v
                 Controllers
                     |
                     v
              PostgreSQL (pg)
                     |
          +----------+----------+
          |                     |
          v                     v
 Product_Information      Product_Inventory
          |                     |
          +----------+----------+
                     |
                     v
                EJS Views
                     |
                     v
              Rendered HTML
                     |
                     v
               Client / Browser
```

## Database Design

The application uses a relational PostgreSQL database consisting of two primary tables.

### 1\. Product_Information

Stores detailed information about individual products.

| Column                 | Data Type     | Description                             |
| ---------------------- | ------------- | --------------------------------------- |
| `id`                   | INTEGER       | Primary key, automatically generated    |
| `Category`             | VARCHAR(300)  | Product category (e.g. Watches, Phones) |
| `Item_Name`            | VARCHAR(1000) | Product name                            |
| `Manufacturer`         | VARCHAR(300)  | Manufacturer's name                     |
| `Model_No`             | VARCHAR(300)  | Product model number                    |
| `Product_Description`  | VARCHAR(2000) | Detailed product description            |
| `External_Product_URL` | VARCHAR(2000) | External product reference URL          |
| `Image_URL`            | VARCHAR(500)  | Product image location                  |

### 2\. Product_Inventory

Stores inventory information associated with individual products.

| Column             | Data Type     | Description                                      |
| ------------------ | ------------- | ------------------------------------------------ |
| `id`               | INTEGER       | Primary key, automatically generated             |
| `product_Id`       | INTEGER       | Foreign key referencing `Product_Information.id` |
| `Quantity`         | INTEGER       | Available stock quantity                         |
| `Price`            | DECIMAL(10,2) | Product price                                    |
| `Customer_Reviews` | VARCHAR(5000) | Customer reviews                                 |

### Database Relationships

The database maintains a one-to-one-style association between product information and inventory records through the `product_Id` foreign key.

```
Product_Information
-------------------
id (Primary Key)
Category
Item_Name
Manufacturer
Model_No
Product_Description
External_Product_URL
Image_URL
        |
        | 1
        |
        | product_Id
        |
        | 1
        v
Product_Inventory
-----------------
id (Primary Key)
product_Id (Foreign Key)
Quantity
Price
Customer_Reviews
```

**Foreign Key Constraint:**

`Product_Inventory.product_Id` references `Product_Information.id`.

The relationship uses `ON DELETE CASCADE`, ensuring that deleting a product also removes its associated inventory record.

Products belonging to the same category are grouped using the `Category` column.

## Getting Started

Follow the instructions below to set up the application locally.

### Prerequisites

Ensure the following software is installed:

- [Node.js](https://nodejs.org/) (LTS recommended)

- [pnpm](https://pnpm.io/)

- [PostgreSQL](https://www.postgresql.org/)

- Git

You should also have access to a PostgreSQL database and its connection credentials.

### Installation

**1\. Clone the repository**

```
git clone https://github.com/amalk-au/inventory-management.git

cd inventory-management
```

**2\. Install dependencies**

```
pnpm install
```

**3\. Configure environment variables**

Create a `.env` file in the project root and configure the required environment variables.

See [Environment Variables](#environment-variables) for details.

**4\. Set up the database**

Create a PostgreSQL database and configure the appropriate connection details.

The application includes a database population script that creates the required tables and inserts sample product and inventory records.

See [Database Setup](#database-setup).

**5\. Start the application**

```
pnpm start
```

The application will start using the port configured through `APP_PORT`.

### Environment Variables

Create a `.env` file in the root directory.

The application uses the following environment variables:

| Variable                      | Description                                         | Required                                                       |
| ----------------------------- | --------------------------------------------------- | -------------------------------------------------------------- |
| `NODE_ENV`                    | Application environment (development or production) | No                                                             |
| `HOST`                        | Database host or PaaS hostname                      | Yes                                                            |
| `DATABASE_NAME`               | PostgreSQL database name                            | Yes                                                            |
| `APP_PORT`                    | Port used by the Express application                | Yes                                                            |
| `DATABASE_PORT`               | PostgreSQL database port (typically 5432)           | No                                                             |
| `USER`                        | PostgreSQL database username                        | Yes                                                            |
| `INVENTORY_DATABASE_PASSWORD` | PostgreSQL database password                        | Yes                                                            |
| `URI_LOCAL_TESTING`           | PostgreSQL connection string for local testing      | Required for the population script when using a local database |
| `URI_PRODUCTION`              | PostgreSQL connection string for production         | Required by the current population script                      |

Example configuration:

```
NODE_ENV=development

HOST=localhost
DATABASE_NAME=inventory
APP_PORT=8000
DATABASE_PORT=5432

USER=amal
INVENTORY_DATABASE_PASSWORD=your_database_password

URI_LOCAL_TESTING=postgresql://USERNAME:PASSWORD@localhost:5432/inventory

URI_PRODUCTION=postgresql://USERNAME:PASSWORD@YOUR_DATABASE_HOST/inventory
```

**Important:**

- Replace the example credentials with your actual database configuration.

- Never commit `.env` files, database passwords or production credentials.

- Use the appropriate database connection string for your environment.

- The current `db/populatedb.js` implementation uses `URI_PRODUCTION` directly. If you intend to populate a local database, update the connection string in that script to use `URI_LOCAL_TESTING` instead.

- The production connection string should be configured through your hosting provider's environment variables.

### Database Setup

The repository includes a database population script:

```
node db/populatedb.js
```

This script:

1.  Connects to the configured PostgreSQL database.

2.  Creates the `Product_Information` table if it does not already exist.

3.  Inserts sample product information.

4.  Creates the `Product_Inventory` table if it does not already exist.

5.  Inserts corresponding inventory records.

6.  Establishes the foreign key relationship between the two tables.

The sample dataset includes products from categories such as watches and phones, with corresponding stock quantities, prices and customer reviews.

**Important:** The current population script inserts sample records each time it runs. It is intended for initial database setup rather than repeated execution against an existing populated database.

For cloud-hosted PostgreSQL databases requiring SSL, the current population script configures an SSL connection with `rejectUnauthorized: false`. Review your database provider's SSL requirements before using this configuration in production.

## Project Structure

```
inventory-management/
│
├── controllers/
│   └── # Application controllers
│
├── db/
│   └── populatedb.js       # Database creation and sample data
│
├── docs/
│   └── screenshots       # Screenshots
│
├── public/
│   ├── images/             # Product images
│   ├── css/                # Stylesheets
│   └── # Other static assets
│
├── routes/
│   └── # Application routes
│
├── views/
│   └── # EJS templates
│
├── .gitignore
├── app.js                  # Express application entry point
├── package.json
├── pnpm-lock.yaml
└── README.md
```

Note: The structure above represents the main application directories. Individual files and subdirectories may vary as the project evolves.

## Available Scripts

The following commands are currently defined in `package.json`:

| Command                 | Description                                        |
| ----------------------- | -------------------------------------------------- |
| `pnpm start`            | Start the Express application                      |
| `pnpm install`          | Install project dependencies                       |
| `node db/populatedb.js` | Create database tables and populate sample records |

The current project does not yet include dedicated development, build, lint or automated testing scripts.

## Development

When contributing to the project:

1.  Create a feature branch.

2.  Implement changes following the existing project conventions.

3.  Keep database queries parameterised.

4.  Test database operations and application functionality.

5.  Verify that existing functionality remains intact.

6.  Commit your changes with descriptive commit messages.

7.  Open a pull request.

Keep changes focused, maintainable and well-documented.

### Development Considerations

- Use parameterised SQL queries to protect against SQL injection.

- Keep database operations organised within the existing application structure.

- Maintain referential integrity between product and inventory records.

- Avoid committing environment files and sensitive credentials.

- Ensure database connection settings are compatible with the deployment environment.

## Testing

Automated testing has not yet been configured in the current repository.

The existing `test` script in `package.json` is a placeholder and does not execute an actual test suite.

Manual testing should cover:

- Creating product records.

- Retrieving product information.

- Updating existing product records.

- Deleting products and verifying cascade deletion.

- Managing inventory quantities and prices.

- Validating database relationships.

- Checking database connection handling.

## Deployment

The application has previously been deployed using Render and Koyeb.

### Deployment Configuration

| Configuration   | Details                                  |
| --------------- | ---------------------------------------- |
| Runtime         | Node.js                                  |
| Package Manager | pnpm                                     |
| Build Command   | Not required for the current application |
| Start Command   | `pnpm start`                             |
| Database        | PostgreSQL                               |
| Port            | Configured through `APP_PORT`            |

### Deployment Steps

1.  Create a PostgreSQL database through your hosting provider or use an existing hosted database.

2.  Configure the required environment variables in the hosting provider's dashboard.

3.  Connect the GitHub repository to your hosting platform.

4.  Configure the application start command:

    ```
    pnpm start
    ```

5.  Ensure the application listens on the port provided by the hosting environment.

6.  Verify database connectivity and application functionality after deployment.

For cloud-hosted PostgreSQL databases, ensure that SSL settings match the database provider's requirements.

## Troubleshooting

| Issue                             | Possible Solution                                                                               |
| --------------------------------- | ----------------------------------------------------------------------------------------------- |
| Dependency installation fails     | Verify Node.js and pnpm versions, then retry `pnpm install`.                                    |
| Database connection fails         | Verify database availability, credentials and connection string.                                |
| Application fails to start        | Check the `.env` configuration and ensure `APP_PORT` is defined.                                |
| Database population fails         | Verify the connection string selected in `db/populatedb.js`.                                    |
| PostgreSQL requires SSL           | Review your database provider's SSL requirements and configure the connection accordingly.      |
| Product records are not displayed | Verify that the database tables contain records and that the application can access PostgreSQL. |
| Port already in use               | Change `APP_PORT` to an available port.                                                         |

## Contributing

Contributions, suggestions and improvements are welcome.

To contribute:

1.  Fork the repository.

2.  Create a feature branch.

3.  Implement and test your changes.

4.  Commit your changes.

5.  Open a pull request describing your contribution.

For significant changes, consider opening an issue first to discuss the proposed implementation.

## License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

You are free to use, modify and distribute this software under the terms of the licence.

See [LICENSE](LICENSE) for the full licence text.

## Acknowledgements

- [Node.js](https://nodejs.org/) — JavaScript runtime.

- [Express.js](https://expressjs.com/) — Web application framework.

- [PostgreSQL](https://www.postgresql.org/) — Relational database management system.

- [node-postgres](https://node-postgres.com/) — PostgreSQL client for Node.js.

- [EJS](https://ejs.co/) — Embedded JavaScript templating engine.

## Contact

**Author:** Amal K.

- **Portfolio:**[https://amalk.au](https://amalk.au)

- **GitHub:**[@amalk-au](https://github.com/amalk-au)

- **Repository:**[inventory-management](https://github.com/amalk-au/inventory-management)

- **Issues:**[GitHub Issues](https://github.com/amalk-au/inventory-management/issues)

---

Developed by [amalk-au](https://github.com/amalk-au).

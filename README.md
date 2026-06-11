# BrewPoint — Online Coffee Ordering System

> A RESTful Web API built with ASP.NET Core (.NET 10) for online coffee ordering, with a Razor Views front-end, JWT authentication, and PostgreSQL database.

**Team:** Nikolina Ilievska · Tijana Avramoska  
**Course:** Service Oriented Architecture — South East European University, 2025/2026  
**Repository:** https://github.com/nanuska-lab/BrewPoint.git
**Live URL:** https://brewpoint-74a6.onrender.com

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | ASP.NET Core Web API (.NET 10) |
| Database | PostgreSQL 18 via Npgsql |
| ORM | Entity Framework Core 10 |
| Auth | ASP.NET Core Identity + JWT Bearer |
| Front-end | Razor Views + Bootstrap 5 |
| Deployment | Render |
| CI/CD      | Render Auto-Deploy (GitHub push trigger) |

---

## Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- [PostgreSQL 18](https://www.postgresql.org/download/)
- [Visual Studio 2022](https://visualstudio.microsoft.com/) or VS Code

---

## Setup & Run Locally

### 1. Clone the repository
```bash
git clone https://github.com/nanuska-lab/BrewPoint.git
cd BrewPoint
```

### 2. Configure the database
Open `BrewPoint/appsettings.json` and update the connection string:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=brewpoint_db;Username=postgres;Password=YOUR_PASSWORD"
  },
  "Jwt": {
    "Key": "BrewPointSuperSecretKey1234567890!@#",
    "Issuer": "BrewPoint",
    "Audience": "BrewPointUsers"
  }
}
```

### 3. Run migrations
```bash
cd BrewPoint
dotnet ef database update
```

### 4. Run the application
```bash
dotnet run
```

The app will be available at `https://localhost:7205`.  
Swagger UI is available at `https://localhost:7205/swagger`.

---

## Default Accounts

| Role | Email | Password |
|---|---|---|
| Admin | admin@brewpoint.com | Admin123! |
| User | Register via `/Auth/Register` | — |

---

## Project Structure

```
BrewPoint/
├── Controllers/
│   ├── CoffeeController.cs       # CRUD for coffees (API)
│   ├── OrderController.cs        # Place/view/cancel orders (API)
│   ├── AdminController.cs        # Admin order management (API)
│   ├── AuthController.cs         # Login/Register (API)
│   ├── IngredientController.cs   # Get ingredients (API)
│   └── *ViewController.cs        # Serve Razor Views (MVC)
├── Models/                       # Domain entities
├── DTOs/
│   ├── Requests/                 # API input models
│   └── Responses/                # API output models
├── Data/
│   └── AppDbContext.cs           # EF Core DbContext
├── Repositories/
│   ├── Interfaces/               # Repository contracts
│   └── Implementations/          # EF Core implementations
├── Services/
│   ├── Interfaces/               # Service contracts
│   └── Implementations/          # Business logic
├── Views/                        # Razor Views (front-end)
│   ├── Auth/                     # Login, Register
│   ├── Coffee/                   # Menu, Detail
│   ├── Order/                    # My Orders
│   ├── Admin/                    # Dashboard, Manage Coffees
│   └── Shared/                   # Layout
└── wwwroot/images/               # Static coffee images
```

---

## API Endpoints

| Method | Route | Auth | Description |
|---|---|---|---|
| GET | `/api/Coffee` | None | Get all coffees |
| GET | `/api/Coffee/{id}` | None | Get coffee by ID |
| POST | `/api/Coffee` | Admin | Create coffee |
| PUT | `/api/Coffee/{id}` | Admin | Update coffee |
| DELETE | `/api/Coffee/{id}` | Admin | Delete coffee |
| POST | `/api/Coffee/upload-image` | Admin | Upload coffee image |
| GET | `/api/Ingredient` | None | Get all ingredients |
| POST | `/api/Auth/register` | None | Register user |
| POST | `/api/Auth/login` | None | Login and get JWT token |
| POST | `/api/Order` | User | Place an order |
| GET | `/api/Order/my-orders` | User | Get own orders |
| DELETE | `/api/Order/{id}/cancel` | User | Cancel pending order |
| GET | `/api/Admin/orders` | Admin | Get all orders |
| PATCH | `/api/Admin/orders/{id}/status` | Admin | Update order status |

---

## Key Features

- **Anonymous browsing** — menu is accessible without login
- **Dynamic pricing** — order total calculated from coffee base price + selected extras
- **Price snapshotting** — ingredient prices locked at time of order
- **Role-based access** — Admin and User roles with protected endpoints
- **Image uploads** — coffee images saved to `wwwroot/images/`
- **Order lifecycle** — Pending → Preparing → Ready → Completed / Cancelled

---

## Architecture

```
Razor Views (JS fetch)
    ↓
API Controllers  →  Service Layer  →  Repository Layer  →  PostgreSQL
                     (business logic)   (data access)
```

Patterns used: **Repository Pattern**, **Dependency Injection**, **DTO Pattern**, **Service Layer**.

---

## Running Tests

```bash
cd BrewPoint.Tests
dotnet test
```

---

## CI/CD

Render is connected to the GitHub repository and automatically redeploys on every push to `master`:

1. Render detects the new commit
2. Builds a new Docker image using the `Dockerfile`
3. Runs `dotnet publish` in Release mode
4. Deploys the new container

Live URL: https://brewpoint-74a6.onrender.com
---



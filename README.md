# ShopTogether Backend API

A RESTful Node.js/Express backend for the ShopTogether e-commerce application.

## Features

- **Products API**: Complete CRUD operations for products
- **Users API**: User registration, authentication, and profile management
- **Orders API**: Order creation, tracking, and management
- **File-based Database**: Simple JSON file storage for development
- **JWT Authentication**: Secure token-based authentication
- **CORS Enabled**: Cross-origin requests support
- **Input Validation**: Request validation and error handling

## Quick Start

### Installation

```bash
cd backend
npm install
```

### Running the Server

```bash
# Development mode with auto-reload
npm run dev

# Production mode
npm start
```

The server will start on `http://localhost:3001`

## API Endpoints

### Health Check
- `GET /api/health` - Server health status

### Products
- `GET /api/products` - Get all products (with pagination, filtering)
- `GET /api/products/:id` - Get product by ID
- `GET /api/products/categories` - Get all product categories
- `GET /api/products/featured` - Get featured products
- `POST /api/products` - Create new product
- `PUT /api/products/:id` - Update product
- `DELETE /api/products/:id` - Delete product

### Users
- `GET /api/users` - Get all users (admin)
- `GET /api/users/:id` - Get user by ID
- `POST /api/users/register` - Register new user
- `POST /api/users/login` - User login
- `PUT /api/users/:id` - Update user profile
- `PUT /api/users/:id/wishlist` - Update user wishlist
- `DELETE /api/users/:id` - Delete user (admin)

### Orders
- `GET /api/orders` - Get all orders (with pagination, filtering)
- `GET /api/orders/:id` - Get order by ID
- `GET /api/orders/stats` - Get order statistics
- `GET /api/orders/user/:userId` - Get orders for specific user
- `POST /api/orders` - Create new order
- `PUT /api/orders/:id` - Update order status
- `DELETE /api/orders/:id` - Delete order (admin)

## Query Parameters

### Products
- `category` - Filter by category
- `featured` - Filter featured products (true/false)
- `search` - Search in title, description, brand, category
- `limit` - Pagination limit
- `offset` - Pagination offset

### Users
- `role` - Filter by role (admin/customer)
- `active` - Filter by active status (true/false)
- `limit` - Pagination limit
- `offset` - Pagination offset

### Orders
- `userId` - Filter by user ID
- `status` - Filter by order status
- `sortBy` - Sort field (default: createdAt)
- `sortOrder` - Sort direction (asc/desc, default: desc)
- `limit` - Pagination limit
- `offset` - Pagination offset

## Response Format

### Success Response
```json
{
  "success": true,
  "data": { ... },
  "message": "Operation completed successfully"
}
```

### Error Response
```json
{
  "success": false,
  "error": "Error type",
  "message": "Detailed error message"
}
```

### Pagination Response
```json
{
  "success": true,
  "data": [ ... ],
  "total": 100,
  "count": 10,
  "pagination": {
    "offset": 0,
    "limit": 10,
    "hasMore": true
  }
}
```

## Authentication

The API uses JWT (JSON Web Tokens) for authentication. After successful login or registration, you'll receive a token that should be included in subsequent requests.

### Login Example
```javascript
// Login request
POST /api/users/login
{
  "username": "john_doe",
  "password": "your_password"
}

// Response
{
  "success": true,
  "data": { user_data },
  "token": "jwt_token_here"
}
```

### Using the Token
```javascript
// Include in headers
headers: {
  'Authorization': 'Bearer jwt_token_here'
}
```

## Data Storage

The backend uses JSON files for data storage located in the `/data` directory:
- `products.json` - Product catalog
- `users.json` - User accounts
- `orders.json` - Order history

## Environment Variables

Create a `.env` file in the backend directory:

```env
PORT=3001
JWT_SECRET=your-secret-key-here
NODE_ENV=development
```

## Error Handling

The API includes comprehensive error handling with appropriate HTTP status codes:
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `404` - Not Found
- `409` - Conflict
- `500` - Internal Server Error

## CORS Configuration

CORS is configured to allow requests from:
- `http://localhost:3000` (Create React App)
- `http://localhost:5173` (Vite)

## Development

### File Structure
```
backend/
├── data/           # JSON database files
├── routes/         # API route handlers
├── utils/          # Utility functions
├── middleware/     # Custom middleware
├── server.js       # Main server file
└── package.json    # Dependencies
```

### Adding New Features
1. Create route handler in `/routes`
2. Add route to main server file
3. Update this documentation

## Testing

You can test the API using tools like:
- Postman
- curl
- Frontend application
- API testing tools

### Example API Calls

```bash
# Get all products
curl http://localhost:3001/api/products

# Get featured products
curl http://localhost:3001/api/products/featured

# Register a new user
curl -X POST http://localhost:3001/api/users/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","email":"test@example.com","password":"password123","firstName":"Test","lastName":"User"}'

# Create an order
curl -X POST http://localhost:3001/api/orders \
  -H "Content-Type: application/json" \
  -d '{"userId":"u1","items":[{"productId":"p1","quantity":1}],"shippingAddress":{"firstName":"John","lastName":"Doe","street":"123 Main St","city":"New York","state":"NY","zipCode":"10001","country":"USA"}}'
```

## Security Notes

- Passwords are hashed using bcrypt
- JWT tokens expire after 24 hours
- Sensitive user data is filtered from responses
- Input validation prevents malicious data

## Future Enhancements

- Database integration (MongoDB, PostgreSQL)
- File upload handling
- Email notifications
- Payment processing integration
- Advanced authentication (OAuth, 2FA)
- Rate limiting
- Caching layer
- Comprehensive logging
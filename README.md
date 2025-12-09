# LLM Customer Service - An Intelligent Customer Service System Built on Large Language Models

A front-end and back-end separated intelligent customer service assistant project built with FastAPI and Vue 3, supporting multiple large language models such as DeepSeek V3, Qwen2.5 series, Llama3 series, etc. It covers mainstream application landing scenarios of Agent and RAG in the field of intelligent customer service.

## Functional Features

### 1. General Q&A Capabilities
- **Supports DeepSeek V3 online API**
- **Supports integrating any dialogue model using Ollama, such as Qwen2.5 series, Llama3 series**
- **Flexible model configuration**

### 2. Deep Thinking Capabilities
- **Supports DeepSeek R1 online API**
- **Supports integrating any Deepseek r1 model series using Ollama**
- **Flexible model configuration**

### 3. Ollama Performance Testing Tool
- Single request performance testing
- Concurrent performance testing
- System resource monitoring
- Automated test reports

## Quick Start

### 1. Install Dependencies

```bash
# Create a virtual environment
python -m venv .venv

# Activate the virtual environment
# Windows
.venv\Scripts\activate
# Linux/Mac
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure Environment Variables

Copy the `env.example` file to `llm_backend/.env` and modify the configuration according to your actual situation:

```env
# LLM Service Configuration
CHAT_SERVICE=OLLAMA  # or DEEPSEEK
REASON_SERVICE=OLLAMA  # or DEEPSEEK

# Ollama Configuration
OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_CHAT_MODEL=deepseek-coder:6.7b
OLLAMA_REASON_MODEL=deepseek-coder:6.7b

# DeepSeek Configuration (if used)
DEEPSEEK_API_KEY=your-api-key
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
DEEPSEEK_MODEL=deepseek-chat
```

### 3. Install MySQL Database and Configure Database Connection Information in the `.env` File

### 4. Start the Service

```bash
# Enter the backend directory
cd llm_backend

# Start the service (default port 9000)
python run.py

# If you need to modify the IP and port, edit the configuration in run.py:
uvicorn.run(
    "main:app",
    host="0.0.0.0",  # Modify the listening address
    port=8000,       # Modify the port number
    access_log=False,
    log_level="error",
    reload=True
)
```

After the service starts, you can access:
- API Documentation: http://localhost:8000/docs
- Frontend Interface: http://localhost:8000

## Technology Stack

- Backend:
  - FastAPI
  - SQLAlchemy
  - MySQL
  - Ollama/DeepSeek

- Frontend:
  - Vue 3
  - Element Plus
  - TypeScript

## Notes

1. For production environment deployment:
   - Modify the `SECRET_KEY` in `.env`
   - Configure correct CORS settings
   - Use HTTPS
   - Turn off `reload=True`

2. For development environment:
   - You can enable `reload=True` for hot reloading
   - You can set `log_level="debug"` to view more logs

## License

MIT

# E-commerce Product Data Service

This project provides a simple Python service for retrieving e-commerce product information from a Neo4j graph database and displaying it to the front end through an API or web interface.

## Database Structure

The project uses a Neo4j graph database to store e-commerce related data, mainly including the following nodes and relationships:

### Nodes

1. **Product** - Product
   - ProductID: Product ID
   - ProductName: Product name
   - UnitPrice: Unit price
   - UnitsInStock: Stock quantity
   - UnitsOnOrder: Order quantity
   - QuantityPerUnit: Quantity per unit
   - Discontinued: Discontinued status (boolean)

2. **Category** - Product Category
   - CategoryID: Category ID
   - CategoryName: Category name
   - Description: Category description

3. **Supplier** - Supplier
   - SupplierID: Supplier ID
   - CompanyName: Company name
   - ContactName: Contact person name
   - Phone: Contact phone number

4. **Customer** - Customer
   - CustomerID: Customer ID
   - CompanyName: Company name
   - ContactName: Contact person name

5. **Order** - Order
   - OrderID: Order ID
   - OrderDate: Order date

6. **Review** - Review
   - ReviewID: Review ID
   - ReviewText: Review content
   - Rating: Rating score
   - ReviewDate: Review date

### Relationships

1. **BELONGS_TO** - Product belongs to category: (Product)-[:BELONGS_TO]->(Category)
2. **SUPPLIED_BY** - Product is supplied by supplier: (Product)-[:SUPPLIED_BY]->(Supplier)
3. **CONTAINS** - Order contains product: (Order)-[:CONTAINS]->(Product)
4. **PLACED** - Customer placed order: (Customer)-[:PLACED]->(Order)
5. **WROTE** - Customer wrote review: (Customer)-[:WROTE]->(Review)
6. **ABOUT** - Review is about product: (Review)-[:ABOUT]->(Product)

## File Description

The project contains two main files:

1. **product_service.py** - Provides a service class for interacting with the Neo4j database, encapsulating various methods for querying product information
2. **frontend_demo.py** - A Flask-based web application that provides web interfaces and API endpoints, using product_service to retrieve data

## Install Dependencies

```bash
pip install neo4j flask
```

## Configure Database Connection

Default connection parameters:
- URI: `bolt://localhost:7687`
- Username: `neo4j`
- Password: `your-password`
- Database: `neo4j`

You can modify them in two ways:

1. Directly modify in the code
2. Set environment variables:
   ```bash
   export NEO4J_URI=bolt://localhost:7687
   export NEO4J_USERNAME=neo4j
   export NEO4J_PASSWORD=your-password
   export NEO4J_DATABASE=neo4j
   ```

## Use ProductService

```python
from product_service import ProductService

# Create a service instance
service = ProductService(
    uri="bolt://localhost:7687",
    username="neo4j",
    password="your-password"
)

# Use the context manager to handle connections automatically
with service:
    # Get categories
    categories = service.get_all_categories()
    print(f"Number of categories: {len(categories)}")
    
    # Get products by category
    products = service.get_products_by_category("Smart Speaker")
    print(f"Number of products in the Smart Speaker category: {len(products)}")
    
    # Get product details
    product_details = service.get_product_details(1)
    print(f"Product details: {product_details}")
    
    # Search products
    search_results = service.search_products("Smart")
    print(f"Number of search results: {len(search_results)}")
```

## Run the Web Application

```bash
python frontend_demo.py
```

Visit http://localhost:5000 to view the web application.

## API Endpoints

The following API endpoints are available:

- `GET /api/categories` - Get all product categories
- `GET /api/products/category/<category_name>` - Get products in the specified category
- `GET /api/products/<product_id>` - Get detailed information of the specified product
- `GET /api/products/search?keyword=<keyword>` - Search products
- `GET /api/products/featured` - Get featured products
- `GET /api/products/popular` - Get popular products
- `GET /api/products/<product_id>/reviews` - Get reviews for the specified product

## Notes

1. Ensure the Neo4j database is running and the relevant e-commerce data has been imported
2. If used in a production environment, ensure appropriate authentication and security mechanisms are added
3. Web templates (HTML files) need to be created manually in the templates directory

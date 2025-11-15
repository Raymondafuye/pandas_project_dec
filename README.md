# Nigeria Health Data Analysis API

A FastAPI-based asynchronous RESTful API for analyzing Nigeria's public health indicators using mortality and health data.

## Features

- **Asynchronous CSV Upload**: Efficient handling of health datasets up to 10MB
- **Schema Inspection**: Comprehensive dataset metadata and sample data
- **Filtered Queries**: Query by sex and year with customizable limits
- **Background Jobs**: Asynchronous data summarization with job tracking
- **Data Validation**: Pydantic models for request/response validation
- **Comprehensive Testing**: Full pytest suite with valid/invalid payload testing

## Project Structure

```
├── src/
│   ├── main.py              # FastAPI application entry point
│   ├── routers/
│   │   └── health_router.py # API route definitions
│   ├── controllers/
│   │   └── health_controller.py # Request handling logic
│   ├── services/
│   │   └── data_service.py  # Data processing with Polars
│   ├── models/
│   │   └── schemas.py       # Pydantic models
│   ├── utils/
│   │   └── logger.py        # Logging configuration
│   └── tests/
│       └── test_api.py      # Pytest test suite
├── requirements.txt         # Python dependencies
├── README.md               # Project documentation
└── .env                    # Environment configuration
```

## Installation

1. **Clone and navigate to project directory**
```bash
cd Pandas_Project
```

2. **Create virtual environment**
```bash
python -m venv venv
venv\Scripts\activate  # Windows
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

## Usage

### Start the API Server

```bash
python -m src.main
```

The API will be available at `http://localhost:8000`

### API Documentation

Interactive API documentation: `http://localhost:8000/docs`

## API Endpoints

### 1. Upload Dataset
```http
POST /api/v1/upload
```
Upload CSV file for analysis. Supports files up to 10MB.

**Request**: Multipart form with CSV file
**Response**: Upload confirmation with dataset dimensions

### 2. Get Schema
```http
GET /api/v1/schema
```
Retrieve dataset schema including columns, data types, and sample data.

**Response**: Schema metadata and sample records

### 3. Query by Sex
```http
GET /api/v1/query/sex?sex=Male&limit=100
```
Filter health data by sex category.

**Parameters**:
- `sex`: Male, Female, or "Both sexes"
- `limit`: Maximum records (1-1000)

### 4. Query by Year
```http
GET /api/v1/query/year?year=2000&limit=100
```
Filter health data by specific year.

**Parameters**:
- `year`: Year to filter by
- `limit`: Maximum records (1-1000)

### 5. Create Summary Job
```http
POST /api/v1/summarize
```
Start background job for data summarization.

**Response**: Job ID for tracking progress

### 6. Get Job Results
```http
GET /api/v1/jobs/{job_id}
```
Retrieve job status and results.

**Response**: Job status, completion time, and analysis results

## Data Summaries

The summarization job generates three key analyses:

1. **Basic Statistics**: Descriptive statistics for numeric columns
2. **Missing Values**: Null count and percentage per column
3. **Distributions**: Value counts for categorical columns

## Testing

Run the complete test suite:

```bash
pytest src/tests/ -v
```

Test categories:
- **Endpoint validation**: Valid/invalid requests
- **Parameter validation**: Boundary testing
- **Error handling**: Missing data scenarios
- **Workflow testing**: Complete job lifecycle

## Example Usage

1. **Upload the mortality dataset**:
```bash
curl -X POST "http://localhost:8000/api/v1/upload" \
     -H "accept: application/json" \
     -H "Content-Type: multipart/form-data" \
     -F "file=@data/mortality-and-global-health-estimates-indicators-for-nigeria-24.csv"
```

2. **Query male mortality data**:
```bash
curl "http://localhost:8000/api/v1/query/sex?sex=Male&limit=50"
```

3. **Create summary job**:
```bash
curl -X POST "http://localhost:8000/api/v1/summarize"
```

4. **Check job results**:
```bash
curl "http://localhost:8000/api/v1/jobs/{job_id}"
```

## Technical Implementation

- **FastAPI**: Async web framework with automatic OpenAPI documentation
- **Polars**: High-performance DataFrame library for data processing
- **Pydantic**: Data validation and serialization
- **Background Jobs**: In-memory job queue with async task execution
- **Logging**: Structured logging for monitoring and debugging

## Performance Considerations

- Async file upload with temporary storage
- Efficient Polars operations for large datasets
- Background job processing to avoid blocking requests
- Memory-efficient data filtering and querying

## Error Handling

- Comprehensive HTTP status codes
- Detailed error messages for debugging
- Input validation with clear feedback
- Graceful handling of missing data scenarios
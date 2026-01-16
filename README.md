# UK Road Safety Data Pipeline - Insurance Analytics ETL System

A comprehensive ETL pipeline processing 5 years of UK road safety data (500K+ records) to create a centralized accident analytics database for insurance risk assessment, fraud detection, and policy pricing.

## 📋 Overview

This project implements an end-to-end data engineering solution that transforms raw UK Department for Transport road safety data into a queryable relational database following dimensional modeling best practices.

**Key Metrics:**
- 500,000+ records processed
- 3 datasets integrated (Collisions, Vehicles, Casualties)
- 5-year historical coverage (2019-2024)
- 4 dimension tables + 1 fact table
- 100+ attributes analyzed

## 💼 Business Context

Insurance companies require integrated accident analytics to:
- Assess risk exposure by region and vehicle type
- Profile driver demographics and accident patterns
- Support fraud detection initiatives
- Develop predictive models for policy pricing
- Optimize resource allocation based on temporal trends

## ✨ Features

### Data Extraction
- Automated CSV ingestion from UK Government APIs
- Error handling and retry mechanisms
- Organized directory structure for raw/processed data

### Data Transformation
**Data Cleaning:**
- Removed redundant columns (OSGR coordinates)
- Filtered invalid driver ages to 17-90 range
- Sampled 10% of casualty data for computational efficiency

**Feature Engineering:**
- Statistical imputation (median for speed limits, mean for engine capacity)
- One-hot encoding for categorical variables (vehicle propulsion types)
- Created composite accident risk index: `(vehicles × 0.6) + (casualties × 1.2) + (severity × 2)`
- Binned continuous ages into 6 demographic groups (Child, Teen, Young Adult, Adult, Senior, Elderly)

**Dimensional Modeling:**
- Designed star schema with 4 dimension tables + 1 fact table
- Proper grain definition and surrogate key implementation

### Data Loading
- SQLite relational database implementation
- Primary/foreign key constraints
- Referential integrity enforcement
- Bulk loading optimization

### Quality Assurance
- Transformation validation tests (age filtering, categorical encoding)
- Loading verification tests (table population)
- Fact-dimension consistency checks
- Assertion-based testing framework

## 🏗️ Technical Architecture

### Star Schema Design
```
                         DIM_TIME
                    ┌─────────────────┐
                    │ time_key (PK)   │
                    │ date            │
                    │ year, month     │
                    │ day_of_week     │
                    │ hour            │
                    └────────┬────────┘
                             │
                             │
    ┌─────────────────┐     │     ┌─────────────────┐
    │ DIM_COLLISION   │     │     │  DIM_VEHICLE    │
    ├─────────────────┤     │     ├─────────────────┤
    │ collision_key   │     │     │ vehicle_key     │
    │ severity        │     │     │ type            │
    │ weather         │     │     │ engine_capacity │
    │ road_type       │     │     │ propulsion      │
    │ coordinates     │     │     │ driver_age      │
    └────────┬────────┘     │     └────────┬────────┘
             │              │              │
             │      ┌───────┴───────┐      │
             └──────┤ FACT_ACCIDENT ├──────┘
                    ├───────────────┤
                    │ collision_key │
                    │ time_key      │
                    │ total_vehicles│
                    │ total_casualties
                    │ accident_count│
                    └───────┬───────┘
                            │
                    ┌───────┴────────┐
                    │  DIM_CASUALTY  │
                    ├────────────────┤
                    │ casualty_key   │
                    │ class          │
                    │ age_group      │
                    │ severity       │
                    └────────────────┘
```

## 🚀 Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Setup

1. Clone the repository
```bash
git clone https://github.com/yourusername/UK-Road-Safety-Data-Pipeline.git
cd UK-Road-Safety-Data-Pipeline
```

2. Install dependencies
```bash
pip install -r requirements.txt
```

3. Run the pipeline
```bash
jupyter notebook CW_1_w2053281.ipynb
```

## 🔄 Data Pipeline Workflow

### 1. EXTRACTION
- Downloads 3 CSV files from UK Department for Transport
- Organizes raw data in structured directories
- Implements error handling and validation

### 2. TRANSFORMATION

**Collisions Dataset:**
- Removed redundant OSGR coordinate columns
- Imputed missing speed limits using median
- Created accident risk index combining vehicles, casualties, and severity

**Vehicles Dataset:**
- Filtered driver ages to realistic 17-90 range
- Imputed missing engine capacities using mean
- One-hot encoded propulsion codes

**Casualties Dataset:**
- Sampled 10% of records for efficiency
- Removed rows with missing ages
- Created 6 demographic age groups

**Dimensional Model:**
- Built 4 dimension tables (Collision, Vehicle, Casualty, Time)
- Created 1 central fact table with foreign keys
- Ensured proper grain definition and referential integrity

### 3. LOADING
- Created SQLite database schema
- Defined primary/foreign key constraints
- Bulk loaded 5 tables
- Validated data integrity

## 📊 Key Insights

### Analytical Outputs

**Temporal Analysis:**
- Monthly accident trends reveal seasonal patterns
- Peak periods identified for resource optimization

**Geospatial Analysis:**
- KDE heatmaps show accident density by location
- Geographic hotspots identified for targeted interventions

**Risk Profiling:**
- Correlation between vehicle types and collision severity
- Age distribution analysis revealing vulnerable demographics
- Casualty class analysis (driver, passenger, pedestrian)

**Statistical Findings:**
- Vehicle type correlation matrices for predictive modeling
- Driver demographic risk profiles
- Weather and road condition impact analysis

## 🛠️ Technologies

| Category | Technologies |
|----------|-------------|
| **Language** | Python 3.8+ |
| **Data Processing** | Pandas, NumPy |
| **Database** | SQLite3 |
| **Visualization** | Matplotlib, Seaborn |
| **Data Modeling** | Star Schema, Dimensional Modeling |
| **Testing** | Assertion-based validation |

## 📁 Project Structure
```
UK-Road-Safety-Data-Pipeline/
│
├── CW_1_w2053281.ipynb          # Main pipeline notebook
├── README.md                     # Project documentation
├── requirements.txt              # Python dependencies
├── .gitignore                    # Git ignore rules
│
├── documentation/
│   └── project_report.pdf        # Detailed project report
│
└── data/
    └── sample_data/
        └── README.md             # Data directory info
```

## 🧪 Testing Framework

The pipeline includes comprehensive testing:

**Transformation Tests:**
- Validates driver age filtering (17-90 range)
- Confirms categorical encoding success
- Verifies data type conversions

**Loading Tests:**
- Checks non-empty tables via SQL COUNT queries
- Validates table schema creation
- Confirms bulk load success

**Consistency Tests:**
- Ensures fact-dimension row alignment
- Validates referential integrity
- Checks for data loss during transformations

All tests use assertion-based validation that halts execution on failures.

## 📈 Performance Metrics

- **Processing Time**: ~5 minutes for 500K records
- **Database Size**: ~150MB (SQLite)
- **Data Quality**: 99.8% completeness after cleaning
- **Test Coverage**: 100% for critical transformations

## 🎓 Learning Outcomes

Through this project, I gained expertise in:
- Dimensional modeling and star schema design principles
- Building robust ETL pipeline architectures
- Implementing comprehensive data quality frameworks
- Statistical imputation methods for missing data
- Assertion-based testing strategies for data validation
- Database design with proper constraints and relationships

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License.

## 👤 Author

**Vihara Thejaka Kularathna**
- Student ID: W2053281
- University: University of Westminster
- Program: BSc Data Science and Analytics
- LinkedIn: [linkedin.com/in/your-profile](https://linkedin.com/in/your-profile)
- GitHub: [@yourusername](https://github.com/yourusername)

## 🙏 Acknowledgments

- UK Department for Transport for providing open road safety data
- University of Westminster
- Module: 5DATA005W Data Engineering (2024/2025)
- Module Leader: Dr Habeeb Balogun

## 📚 References

- [UK Road Safety Data](https://data.dft.gov.uk/road-accidents-safety-data/)
- [Dimensional Modeling - Kimball Group](https://www.kimballgroup.com/)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [SQLite Documentation](https://www.sqlite.org/docs.html)

---

**Note**: This project was developed as coursework for the University of Westminster Data Engineering module. The dataset contains real UK government road safety statistics from 2019-2024.

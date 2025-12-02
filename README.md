

#  Real-Time Intelligence System for Event-Driven Data Analysis

**Built with Microsoft Fabric | Real-Time Hub | Eventhouse | KQL | Streaming Pipelines**

This project implements a full **end-to-end real-time analytics system** using **Microsoft Fabric’s Real-Time Intelligence platform**. It captures streaming data, transforms it in motion, stores it in a scalable Eventhouse environment, and delivers insights through dashboards, alerts, and anomaly detection.

This solution uses a **bicycle-sharing dataset** as the case study and demonstrates the complete workflow from ingestion → transformation → analytics → alerting → visualization → anomaly detection.

---

##  Features

### **🔹 Real-Time Data Infrastructure**

* Eventhouse provisioning with auto-generated KQL database
* Workspace setup and resource organization
* Time-series optimized storage

### **🔹 Streaming Data Pipeline**

* Real-Time Hub ingestion
* Eventstream configuration
* Field mapping & timestamp enrichment
* Eventhouse destination table creation (RawData)
* Continuous, automated data flow

### **🔹 Data Transformation Architecture**

* Medallion architecture (Bronze → Silver → Gold)
* Update policies for automated transformations
* Reusable KQL function for data parsing & enrichment
* Creation of `TransformedData` table

### **🔹 Advanced Analytics**

* Time-series visualization
* Materialized views for aggregated analytics
* T-SQL + KQL cross-language support
* SQL → KQL translation using `explain`

### **🔹 Real-Time Monitoring & Alerts**

* Threshold-based alerting (bike availability < 5)
* Email notification integration
* Stream-level rule activation

### **🔹 Dashboarding & Geospatial Visualization**

* Real-time dashboard with:

  * Column charts
  * Map visualizations (latitude/longitude)
* Auto-refreshing tiles
* Live operational monitoring

### **🔹 Anomaly Detection**

* AI-driven anomaly detection model
* Group-by attribute selection (street-level monitoring)
* Training, validation, and optimization
* Highlighting abnormal empty dock patterns

---

##  Architecture Diagram (Conceptual)


     
                         ┌───────────────────────────┐
                         │   Bicycle Rental Source    │
                         │ (Streaming Telemetry Data) │
                         └───────────────┬────────────┘
                                         │
                                         ▼
                         ┌────────────────────────────────┐
                         │     Real-Time Hub (Eventstream)│
                         │  - Ingestion                   │
                         │  - Field Mapping               │
                         │  - Timestamp Enrichment        │
                         └───────────────┬────────────────┘
                                         │
                                         ▼
                         ┌────────────────────────────────┐
                         │ Eventhouse (KQL Database)       │
                         │  RawData Table (Bronze Layer)   │
                         └───────────────┬────────────────┘
                                         │  Update Policy
                                         ▼
                         ┌────────────────────────────────┐
                         │ TransformedData Table (Silver)  │
                         │ - Parsed IDs                    │
                         │ - Calculated Fields             │
                         │ - Action Recommendations        │
                         └───────────────┬────────────────┘
                                         │
                                         ▼
                   ┌──────────────────────────────┬──────────────────────────┐
                   ▼                              ▼                          ▼
     ┌───────────────────────┐   ┌─────────────────────────┐   ┌─────────────────────────┐
     │ Materialized View     │   │ Real-Time Dashboard      │   │ Anomaly Detection        │
     │ (Gold Layer)          │   │ - Charts                 │   │ - Pattern Detection      │
     │ AggregatedData        │   │ - Maps                   │   │ - Alerts (Future)        │
     └───────────────────────┘   └─────────────────────────┘   └─────────────────────────┘

##  Project Structure

```
/project-root
│
├── README.md   # Project documentation
├── architecture/
│   └── data_flow.png  # (optional diagram)
│
├── kql/
│   ├── raw_schema.alter.kql
│   ├── transformed_table.create.kql
│   ├── transform_function.kql
│   └── update_policy.kql
│
└── dashboards/
    ├── availability_chart.png
    └── geospatial_map.png
```

---

##  Technologies Used

| Component     | Technology                                   |
| ------------- | -------------------------------------------- |
| Streaming     | Microsoft Fabric Real-Time Hub               |
| Storage       | Eventhouse + KQL Database                    |
| Processing    | Eventstream transformations, Update Policies |
| Querying      | Kusto Query Language (KQL), T-SQL            |
| Visualization | Fabric Real-Time Dashboards                  |
| Intelligence  | Fabric Anomaly Detection                     |

---

##  Key KQL Snippets

### **Transformation Function**

```kql
.create-or-alter function TransformRawData() {
    RawData
    | parse BikepointID with * "BikePoints_" BikepointID:int
    | extend BikesToBeFilled = No_Empty_Docks - No_Bikes
    | extend Action = iff(BikesToBeFilled > 0, tostring(BikesToBeFilled), "NA")
}
```

### **Update Policy**

```kql
.alter table TransformedData policy update
[
  {
    "IsEnabled": true,
    "Source": "RawData",
    "Query": "TransformRawData()",
    "IsTransactional": false,
    "PropagateIngestionProperties": false
  }
]
```

### **Materialized View**

```kql
.create-or-alter materialized-view with (folder="Gold") AggregatedData on table TransformedData
{
    TransformedData
    | summarize arg_max(Timestamp, No_Bikes) by BikepointID
}
```

---

##  Results

* Fully operational real-time ingestion pipeline
* Automated transformations with medallion architecture
* Instant alerts for low bike availability
* Interactive dashboards updated continuously
* Anomaly detection identifying unusual patterns
* Cross-language analytics (SQL + KQL)
* Production-ready architecture suitable for enterprise use

---

##  Future Enhancements

* Predictive forecasting model (bike demand prediction)
* Integration with Action Groups / Teams notifications
* Real-Time Data Science (ML in Eventhouse)
* Multi-region station analytics
* Custom geospatial polygon mapping

---

##  Author

**Robert Ssebambulidde**

Real-Time Analytics & Data Engineering Enthusiast


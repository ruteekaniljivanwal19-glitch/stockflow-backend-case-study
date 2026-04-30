# StockFlow - Backend Case Study Solution

## Overview
This repository contains my submission for the Backend Engineering Intern case study for a B2B SaaS Inventory Management System ("StockFlow"). The system focuses on multi-warehouse inventory tracking, product management, and low-stock alerting for small and mid-sized businesses.

---

## Part 1: Code Review & Debugging

### Issues Identified
- Missing input validation for request payload
- No transaction management leading to inconsistent database state
- SKU uniqueness not enforced
- Incorrect data modeling (product tied to single warehouse)
- Floating-point precision issue for price
- No error handling or rollback mechanism
- Multiple database commits instead of single atomic transaction
- Missing validation for warehouse existence
- Unsafe direct dictionary access causing runtime errors

### Fix Summary
- Added proper validation for required fields
- Enforced SKU uniqueness check
- Used database transaction with rollback support
- Fixed product-warehouse relationship using Inventory table
- Used Decimal for price precision
- Added error handling and safe defaults

---

## Part 2: Database Design

### Key Entities
- Companies
- Warehouses
- Products
- Inventory (junction table for multi-warehouse support)
- Inventory Logs (tracking stock changes)
- Suppliers
- Product-Supplier Mapping
- Product Bundles
- Sales

### Design Decisions
- Inventory modeled as a separate table to support many-to-many relationship between products and warehouses
- SKU enforced as globally unique
- Decimal type used for price to avoid floating-point errors
- Inventory logs added for audit trail and tracking stock movements
- Indexes recommended on SKU and foreign keys for performance

### Assumptions / Missing Requirements
- Definition of "recent sales" period (assumed 30 days)
- Supplier-product relationship (single vs multiple suppliers)
- Bundle inventory deduction rules
- Backorder and reserved stock handling not defined

---

## Part 3: Low Stock Alerts API

### Endpoint
GET /api/companies/{company_id}/alerts/low-stock

### Logic Summary
- Fetch inventory per warehouse per product
- Calculate recent sales (assumed last 30 days)
- Compute average daily sales
- Compare current stock against dynamic threshold
- Calculate estimated days until stockout
- Include supplier information for reordering

### Edge Cases Handled
- No sales data (avoid division by zero)
- Missing supplier data
- Multiple warehouses per product
- Zero stock scenarios

---

## Technical Highlights
- RESTful API design
- Transaction-safe database operations
- Scalable schema supporting multi-warehouse inventory
- Business-aware low stock prediction logic
- Proper separation of concerns between models and API layer

---

## Tech Stack (Assumed)
- Python (Flask)
- SQLAlchemy ORM
- PostgreSQL/MySQL

---

## Author
Backend Engineering Case Study Submission

# ServiceNow-Metro-Project
Creating this repository for storing my project

🚇 Metro Ticket Generating System in ServiceNow

A ServiceNow-based digital metro ticket booking and ticket management system that automates metro ticket booking, fare calculation, payment information handling, QR-based ticket generation, and backend ticket record creation.

<img width="320" height="176" alt="image" src="https://github.com/user-attachments/assets/bc34e6dd-c999-45c5-b278-c1a39355fd34" />


---

## 📌 Project Overview

The **Metro Ticket Generating System in ServiceNow** is designed to provide a simple and automated process for booking metro tickets.

A passenger can select the starting station, destination station, journey type, and number of passengers. The system automatically calculates the applicable fare, provides payment-mode options, generates a QR-based digital ticket, and creates a centralized ticket record in ServiceNow.

The project demonstrates the use of:

- ServiceNow Service Catalog
- Service Portal
- Catalog Client Scripts
- Catalog UI Policies
- Service Portal Widgets
- Flow Designer
- Custom Tables
- ACLs
- Auto-Numbering
- Reference Fields
- ServiceNow Update Sets

---

# 🎯 Objectives

The main objectives of this project are:

- Digitize the metro ticket booking process.
- Reduce manual effort involved in ticket booking.
- Automatically calculate metro fares.
- Support single and return journeys.
- Support multiple passengers.
- Provide multiple payment options.
- Generate a QR-based digital ticket.
- Automatically create ticket records.
- Maintain readable RITM tracking.
- Provide centralized ticket information.
- Demonstrate ServiceNow automation and workflow capabilities.

---

# ✨ Key Features

## 1. Metro Station Master Data

A custom ServiceNow table is created to maintain metro station information.

**Table Label:**

`Metro Station's Details`

**Table Name:**

`u_metro_station_details`

### Configured Stations

- Ameerpet
- Madhapur
- Panjagutta
- Uppal Stadium
- Jubilee Hills
- LB Nagar
- Kukatpally

These station records are used as reference values in the ticket-booking form.

---

# 2. Book A Metro Ticket

A Service Catalog item named:

**Book A Metro Ticket**

is created for passenger ticket booking.

### Catalog Variables

| Variable | Type |
|---|---|
| Starting From | Reference |
| Going To | Reference |
| Type of journey | Multiple Choice |
| No of Passengers | Select Box |
| Amount for single journey | Single Line Text |
| Amount including return | Single Line Text |
| Mode of Payment | Multiple Choice |
| Enter Payment Mode | Single Line Text |
| QR Generated | Yes/No |

---

# 3. Automatic Fare Calculation

The system automatically calculates the fare based on:

- Starting station
- Destination station
- Journey type
- Number of passengers

### Single Journey

```text
Total Fare = Base Fare × Number of Passengers
Return Journey
Total Fare = Base Fare × 2 × Number of Passengers

The system also validates the route and prevents the passenger from selecting the same station as both the starting and destination station.

4. Payment Modes

The system supports:

UPI
Card
Others

When the user selects:

Mode of Payment = Others

the following field becomes visible:

Enter Payment Mode

This behavior is implemented using a Catalog UI Policy.

Example:

Mode of Payment → Others
                     ↓
             Enter Payment Mode
                     ↓
                    Cash
5. QR Ticket Generation

The project includes a ServiceNow Service Portal widget:

Metro QR Widget

Widget ID
metro_qr_widget

The widget displays a QR representation of the generated metro ticket.

The QR image is generated from a ticket URL/identifier and displayed through the ServiceNow widget.

The QR ticket provides a convenient digital representation of the booking.

6. Metro Database

A custom ServiceNow table is created to store generated ticket information.

Table Label:

Metro Database

Table Name:

u_metro_database

Important Fields
Ticket Number
RITM Number
Request Item
Starting From
Going To
Type of Journey
No of Passengers
Amount for Single Journey
Amount Including Return
Mode of Payment
Enter Payment Mode
QR Generated
Created
Created By
Updated
Updated By
7. Auto-Generated Ticket Number

The Metro Database uses ServiceNow auto-numbering.

Configuration
Prefix       : MTKT
Number       : 1
Digits       : 5

Example ticket numbers:

MTKT00002
MTKT00003
MTKT00004
MTKT00005
8. RITM Number Tracking

The Metro Database contains a separate readable RITM Number field.

The Flow Designer mapping is:

Trigger
   ↓
Service Catalog
   ↓
Requested Item Record
   ↓
Number
   ↓
RITM Number

Example:

RITM0010006

The original Request Item reference field is retained for relationship tracking.

🏗️ System Architecture
                     ┌──────────────────┐
                     │    Passenger     │
                     └────────┬─────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │ ServiceNow Service Portal│
                 │   Book A Metro Ticket   │
                 └────────────┬────────────┘
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
     ┌───────────────┐ ┌──────────────┐ ┌───────────────┐
     │ Metro Station │ │ Fare Client  │ │ Payment UI    │
     │ Master Table  │ │ Script       │ │ Policy        │
     └───────────────┘ └──────────────┘ └───────────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │  Metro QR Widget │
                    │  QR Generation   │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │  Flow Designer   │
                    │  Metro Project   │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │  Metro Database  │
                    │ MTKT + RITM No.  │
                    └──────────────────┘
🔄 Application Workflow
Open Service Portal
        ↓
Book A Metro Ticket
        ↓
Select Starting Station
        ↓
Select Destination Station
        ↓
Select Journey Type
        ↓
Select Number of Passengers
        ↓
Automatic Fare Calculation
        ↓
Select Payment Mode
        ↓
Enter Additional Payment Mode if required
        ↓
Generate QR Ticket
        ↓
Confirm / Submit Request
        ↓
ServiceNow Request & RITM Created
        ↓
Flow Designer Runs
        ↓
Metro Database Record Created
        ↓
MTKT Ticket Number + RITM Number Stored
🧩 ServiceNow Components
Component	Configuration
Custom Table	u_metro_station_details
Custom Table	u_metro_database
Catalog Item	Book A Metro Ticket
Client Script	Fare calculation and validation
Catalog UI Policy	Others payment field visibility
Service Portal Widget	Metro QR Widget
Flow	Metro Project
ACL	Metro Station table access
Auto Number	MTKT + 5 digits
RITM Tracking	RITM Number field
⚙️ Flow Designer

The main Flow Designer flow is:

Metro Project
Trigger
Service Catalog
Actions
1. Get Catalog Variables
2. Create Record
Record Mapping
Metro Database Field	Source
Starting From	starting_from
Going To	going_to
Amount for Single Journey	amount_for_single_journey
Amount Including Return	amount_including_return
Enter Payment Mode	enter_payment_mode
QR Generated	qr_generated
Mode of Payment	mode_of_payment
Type of Journey	type_of_journey
No of Passengers	no_of_passengers
Request Item	Trigger → Requested Item Record
RITM Number	Trigger → Requested Item Record → Number
Ticket Number	Automatically generated
💰 Fare Configuration

The current project prototype uses a configured station-to-station fare matrix.

From	                To	                        Base Fare
Ameerpet	         Uppal Stadium	                   ₹40
Ameerpet           Jubilee Hills	                   ₹20
Ameerpet	         LB Nagar	                         ₹50
Ameerpet	         Kukatpally	                       ₹30
Madhapur	         Uppal Stadium	                   ₹50
Madhapur	         Jubilee Hills	                   ₹10
Madhapur	         LB Nagar	                         ₹60
Madhapur	         Kukatpally	                       ₹30
Panjagutta	       Jubilee Hills	                   ₹20
Panjagutta	       LB Nagar	                         ₹50
Panjagutta	       Kukatpally	                       ₹30
Uppal Stadium	     Madhapur	                         ₹50
Uppal Stadium	     Panjagutta	                       ₹40
Uppal Stadium	     Kukatpally	                       ₹60
Jubilee Hills	     Madhapur	                         ₹10
Jubilee Hills	     Panjagutta	                       ₹20
Jubilee Hills	     Uppal Stadium	                   ₹50
LB Nagar	         Madhapur	                         ₹60
LB Nagar	         Jubilee Hills	                   ₹50
LB Nagar	         Kukatpally	                       ₹70
Kukatpally	       Ameerpet	                         ₹30
Kukatpally	       Uppal Stadium	                   ₹60
Kukatpally	       LB Nagar	                         ₹70

Note: These fare values are configured for the academic/project prototype and should not be considered actual metro authority fares.

🗃️ Data Model
Metro Station's Details
u_metro_station_details
        │
        └── u_station_name
Metro Database
u_metro_database
│
├── Ticket Number
├── RITM Number
├── Request Item
├── Starting From
├── Going To
├── Type of Journey
├── No of Passengers
├── Amount for Single Journey
├── Amount Including Return
├── Mode of Payment
├── Enter Payment Mode
├── QR Generated
└── System Fields
🔐 Security

The project uses ServiceNow access-control mechanisms.

The custom Metro Station's Details table has ACLs for appropriate table operations and uses the generated ServiceNow role for access.

Security considerations include:

ServiceNow authentication
Role-based authorization
Table ACLs
Controlled access to custom records
No passwords or secrets stored in GitHub
🧪 Testing

The application was tested using the following scenarios:

Test Scenario	Expected Result	Status
Open catalog item	Booking form opens	✅ Passed
Select stations	Station references work	✅ Passed
Same source/destination	Validation message appears	✅ Passed
Single journey	Correct amount displayed	✅ Passed
Return journey	Correct amount displayed	✅ Passed
Multiple passengers	Fare reflects passenger count	✅ Passed
Others payment	Additional field appears	✅ Passed
QR generation	QR widget displays QR	✅ Passed
Submit request	Request/RITM created	✅ Passed
Flow automation	Metro Database record created	✅ Passed
RITM mapping	Readable RITM number stored	✅ Passed
🐛 Known Limitations
The fare matrix is currently maintained in the Client Script.
The QR image generation uses an external QR image service.
A live production payment gateway is not integrated.
Actual metro-gate QR validation is outside the current project scope.
The fare values are prototype/demo values.
Production deployment requires appropriate ServiceNow environments and approvals.
🚀 Future Enhancements

Possible future improvements include:

Real payment gateway integration.
Dynamic fare master table.
Metro zone-based fare calculation.
Email/SMS ticket notifications.
QR validation at metro station gates.
Passenger booking history.
Admin dashboards.
Reports and analytics.
Additional metro stations.
Mobile-optimized ticket presentation.
Automated ticket status tracking.
Production DEV → TEST → PROD deployment.
Integration with actual metro APIs.
📚 Project Documentation Phases

The project documentation is divided into seven phases.

Phase 1 — Ideation Phase

Includes:

Empathy Map Canvas
Brainstorming
Idea Generation
Idea Grouping
Idea Prioritization
Problem Statements
Phase 2 — Requirement Analysis

Includes:

Customer Journey Map
Functional Requirements
Non-functional Requirements
Data Flow
User Stories
Technology Stack
Application Characteristics
Phase 3 — Project Design Phase

Includes:

Problem–Solution Fit
Proposed Solution
Solution Architecture
System Components
Data Architecture
Phase 4 — Project Planning Phase

Includes:

Product Backlog
Sprint Planning
User Stories
Story Points
Sprint Schedule
Velocity
Burndown Planning
Phase 5 — Project Development Phase

Includes:

Functional Testing
Performance Testing
User Acceptance Testing
Bug Tracking
Test Results
Phase 6 — Project Documentation

Includes:

Functional Specification
Configuration Documentation
Setup Instructions
Architecture
Testing Documentation
Screenshots
Known Issues
Future Enhancements
Final Project Report
Appendix
Phase 7 — Project Demonstration

The final demonstration covers:

Metro Station's Details
Book A Metro Ticket
Station Selection
Journey Type
Passenger Selection
Automatic Fare Calculation
Payment Mode
QR Ticket
Order Confirmation
Request/RITM
Metro Database
Ticket Number and RITM Number
🎬 Demonstration Flow

The recommended project demonstration sequence is:

1. Show Metro Station's Details
                ↓
2. Show Book A Metro Ticket
                ↓
3. Select Source & Destination
                ↓
4. Select Journey Type
                ↓
5. Select Number of Passengers
                ↓
6. Demonstrate Fare Calculation
                ↓
7. Demonstrate Payment Mode
                ↓
8. Demonstrate QR Ticket
                ↓
9. Show Order Confirmation
                ↓
10. Show Request / RITM
                ↓
11. Show Metro Database
                ↓
12. Show MTKT + RITM Number
📊 Sample Ticket Record

Example project test record:

Ticket Number           : MTKT00003
RITM Number             : RITM0010006
Starting From           : Ameerpet
Going To                : LB Nagar
Type of Journey         : Return
No of Passengers        : 2
Amount Including Return : ₹200
Mode of Payment         : Others
Enter Payment Mode      : Cash
QR Generated            : Yes

This is sample project/demo data.

🛠️ Technologies Used
Platform
ServiceNow
ServiceNow Technologies
Service Catalog
Service Portal
Catalog Client Scripts
Catalog UI Policies
Flow Designer
Custom Tables
Reference Fields
ACLs
Auto-Numbering
Service Portal Widgets
Programming / Scripting
JavaScript
HTML
AngularJS-based Service Portal Widget configuration
External Service
QR image generation service
Documentation / Version Control
Microsoft Word
GitHub
ServiceNow Update Sets
📁 Recommended GitHub Repository Structure
metro-ticket-generating-servicenow/
│
├── README.md
│
├── documentation/
│   │
│   ├── 01-Ideation-Phase.docx
│   ├── 02-Requirement-Analysis.docx
│   ├── 03-Project-Design-Phase.docx
│   ├── 04-Project-Planning-Phase.docx
│   ├── 05-Project-Development-Phase.docx
│   ├── 06-Project-Documentation.docx
│   └── 07-Project-Demonstration.docx
│
├── servicenow/
│   │
│   ├── update-set/
│   │   └── Metro-Ticket-Generating-System-V1.0.xml
│   │
│   ├── client-scripts/
│   │   └── fare-client-script.js
│   │
│   ├── widgets/
│   │   └── metro-qr-widget.md
│   │
│   ├── flow/
│   │   └── metro-project-flow.md
│   │
│   └── configuration/
│       └── configuration-reference.md
│
├── screenshots/
│   ├── station-table.png
│   ├── catalog-item.png
│   ├── ticket-form.png
│   ├── qr-ticket.png
│   ├── request-summary.png
│   └── metro-database.png
│
└── LICENSE
📦 Deployment

The project can be transferred between ServiceNow instances using an Update Set.

Recommended deployment process:

Development Instance
        ↓
Create / Select Update Set
        ↓
Capture Project Changes
        ↓
Complete Update Set
        ↓
Export XML
        ↓
Target Instance
        ↓
Retrieved Update Sets
        ↓
Import XML
        ↓
Preview
        ↓
Resolve Errors
        ↓
Commit
        ↓
Post-Deployment Testing
Project Update Set
Metro Ticket Generating System - V1.0
👥 Stakeholders

The main stakeholders are:

Metro Passengers
Station Managers
Metro Operations Team
IT Administrators
🌱 Benefits

The project aims to provide:

Faster ticket booking
Reduced manual effort
Automated fare calculation
Digital payment options
QR-based digital ticket
Centralized ticket records
Better RITM tracking
ServiceNow workflow automation
Reduced dependency on paper-based ticket information
📌 Project Status

Project Name: Metro Ticket Generating System in ServiceNow

Version: V1.0

Platform: ServiceNow

Project Type: Academic / Prototype

Main Workflow:

Service Catalog
      ↓
Fare Calculation
      ↓
Payment
      ↓
QR Ticket
      ↓
Flow Designer
      ↓
Metro Database
👨‍💻 Author

Name: MURALA AYYAPPA

Program: B.Tech — AI & ML

College: Seshadri Rao Gudlavalleru Engineering College

Project: Metro Ticket Generating System in ServiceNow

📄 License

This project is developed for academic and demonstration purposes.

⭐ Acknowledgement

This project was developed as a ServiceNow academic project to demonstrate digital service automation, workflow management, data management, ticket booking, and QR-based digital ticket generation.

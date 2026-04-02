# Vacation-Tracking-System
A system that allows employees to manage their vacation and leave requests.

## (1) Vision
The Vacation Tracking System (VTS) aims to simplify and improve the process of managing employee vacation time.

### Goals
- Enable employees to manage their own vacation, sick leave, and personal time off easily without deep knowledge of company policies.
- Help managers stay aware of employee vacation activities.
- Reduce the workload on the HR department.
- Improve the efficiency of internal business processes, especially in handling vacation requests.

##(2) Functional Requirements

- The system validates and verifies leave requests using a rules-based mechanism.
- The system allows managers to approve or reject vacation requests.
- The system provides access to past requests (previous year) and allows future requests (up to 18 months).
- The system sends email notifications for approvals and status updates.
- The system keeps logs for all transactions.
- The system allows HR and system administrators to override rules with logging.
- The system allows managers to grant personal leave within defined limits.
- The system provides an interface for other internal systems to access employee vacation summaries.
- The system integrates with HR legacy systems to retrieve employee data.

##(3) Non-Functional Requirements

- The system must be easy to use.
- The system should reduce processing time.
- The system should be efficient.
- The system should be secure.
- The system should be reliable.

- ##(4) Constraints

- The system must use existing hardware and middleware.
- The system must be implemented as an extension to the existing intranet portal.
- The system must use single sign-on (SSO) for authentication.

- ##(5) Domain
  
-The system operates within an organization where vacation requests were previously handled manually, causing delays in approval and adding extra workload for the HR department.
-Managers also had limited visibility into employees' vacation activities.
-To address these issues, the company decided to implement a Vacation Tracking System that automates the request process and integrates with existing internal systems.

##(6) Actors

- Employee
- Manager
- HR Staff
- System Administrator

- ##(7) Entities (Data Model)

- Employee
  - employeeId
  - name
  - email

- Manager
  - managerId
  - name

- VacationRequest
  - requestId
  - startDate
  - endDate
  - status
  - description

- LeaveBalance
  - totalDays
  - remainingDays
 
  - ##(8) flow chart
 
  - <img width="749" height="1961" alt="flowChart drawio" src="https://github.com/user-attachments/assets/70ed3451-0fc5-4216-ac0d-1b4dac63d805" />


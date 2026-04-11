# Vacation-Tracking-System
A system that allows employees to manage their vacation and leave requests.

## (1) Vision
The Vacation Tracking System (VTS) aims to simplify and improve the process of managing employee vacation time.

### Goals
- Enable employees to manage their own vacation, sick leave, and personal time off easily without deep knowledge of company policies.
- Help managers stay aware of employee vacation activities.
- Reduce the workload on the HR department.
- Improve the efficiency of internal business processes, especially in handling vacation requests.

## (2) Functional Requirements

- The system validates and verifies leave requests using a rules-based mechanism.
- The system allows managers to approve or reject vacation requests.
- The system provides access to past requests (previous year) and allows future requests (up to 18 months).
- The system sends email notifications for approvals and status updates.
- The system keeps logs for all transactions.
- The system allows HR and system administrators to override rules with logging.
- The system allows managers to grant personal leave within defined limits.
- The system provides an interface for other internal systems to access employee vacation summaries.
- The system integrates with HR legacy systems to retrieve employee data.

## (3) Non-Functional Requirements

- The system must be easy to use.
- The system should reduce processing time.
- The system should be efficient.
- The system should be secure.
- The system should be reliable.

## (4) Constraints

- The system must use existing hardware and middleware.
- The system must be implemented as an extension to the existing intranet portal.
- The system must use single sign-on (SSO) for authentication.

## (5) Domain
  
-The system operates within an organization where vacation requests were previously handled manually, causing delays in approval and adding extra workload for the HR department.

-Managers also had limited visibility into employees' vacation activities.

-To address these issues, the company decided to implement a Vacation Tracking System that automates the request process and integrates with existing internal systems.

## (6) Actors

- Employee
- Manager
- HR Staff
- System Administrator

## (7) Use-Case :-Manage Time

- ### (7.1) Entities (Data Model)

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
 
   ### (7.2) Flowchart Diagram 
 
    <img width="749" height="1961" alt="flowChart drawio" src="https://github.com/user-attachments/assets/70ed3451-0fc5-4216-ac0d-1b4dac63d805" />

   ### (7.3) Sequence Diagram 

<img width="792" height="921" alt="sequance  drawio" src="https://github.com/user-attachments/assets/e15a0622-c0c4-4207-8290-1abb28ce09a9" />

  ### (7.4) Pseudocode(Main flow) 

```
START

Employee opens VTS

System displays requests and balance

Employee selects leave type

Employee enters dates and details

IF data is invalid THEN
    SHOW ERROR
    GO BACK to input
ELSE
    Submit request
    Set status = Pending
END IF

IF request DOES NOT require manager approval THEN
    Set status = Approved
    Send email to employee with approval
ELSE
    Send email to manager
    Manager reviews request

    IF manager approves THEN
        Set status = Approved
        Send email to employee with approval
    ELSE
        Set status = Rejected
        Enter reason
        Send email to employee with rejection
    END IF
END IF

END

```

## (8) Use-Case :-Withdraw Request

 ### (8.1) Flowchart Diagram 

 <img width="586" height="1449" alt="Withdraw Request flow chart" src="https://github.com/user-attachments/assets/b42f8cff-8d0b-441a-864d-226c7848a3b7" />


 ### (8.2) Sequence Diagram

 <img width="991" height="1432" alt="Withdraw Request Sequence digram" src="https://github.com/user-attachments/assets/7b75f0fb-82c9-438c-a44a-297174012ad9" />

 ### (8.3) Pseudocode

```
function withdrawRequest(requestId, employeeId):

    request = getRequest(requestId)

    if request == null:
        return "Request not found"

    if request.employeeId != employeeId:
        return "Unauthorized action"

    if request.status != "PENDING":
        showError("Cannot withdraw non-pending request")
        return

    userConfirmation = promptConfirmation("Are you sure you want to withdraw?")

    if userConfirmation == false:
        cancelOperation()
        return

    request.status = "WITHDRAWN"
    updateRequest(request)

    removeFromManagerList(requestId)

    sendNotificationAsync(to=manager, message="Request withdrawn")

    return "Withdraw successful"
```

## (9) Use-Case :-Cancel Approved Request

### (9.1) Flowchart Diagram 

<img width="791" height="1671" alt="Cancel Approved Request flow chart drawio" src="https://github.com/user-attachments/assets/540f11b9-23bf-44ee-bbd7-2e8f182076f9" />

### (9.2) Sequence Diagram

<img width="971" height="2031" alt="Cancel Approved Request sequence digram drawio (1)" src="https://github.com/user-attachments/assets/b1ad123c-c0d3-4c88-a8eb-6bc0f5e415a0" />


 ### (9.3) Pseudocode

```
function cancelApprovedRequest(requestId, employeeId):

    request = getRequest(requestId)

    if request == null:
        return "Request not found"

    if request.employeeId != employeeId:
        return "Unauthorized action"

    if request.status != "APPROVED":
        showError("Only approved requests can be cancelled")
        return

    if isFuture(request.date):
        confirmation = prompt("Confirm cancellation?")

    else if isRecentPast(request.date):
        confirmation = prompt("Confirm cancellation and provide explanation?")
        explanation = getExplanation()

        if explanation is empty:
            showError("Explanation is required")
            return

    else:
        showError("Cannot cancel this request")
        return

    if confirmation == false:
        cancelOperation()
        return

    request.status = "CANCELLED"
    updateRequest(request)

    restoreVacationBalance(employeeId, request.days)

    sendNotificationAsync(manager, "Request cancelled")

    return "Cancellation successful"
```

## (10) Use-Case :- Edit Pending Request

### (10.1) Flowchart Diagram 

<img width="1291" height="1796" alt="flow chart33 drawio" src="https://github.com/user-attachments/assets/57ff0708-bc13-4e48-b25c-21131d69e616" />

### (10.2) Sequence Diagram

<img width="791" height="2031" alt="333333333333333333333333 drawio" src="https://github.com/user-attachments/assets/eef74b33-1148-4c82-8ab6-aa25547a506d" />


### (10.3) Pseudocode

```
function editPendingRequest(requestId, employeeId):

    request = getRequest(requestId)

    if request == null:
        return "Request not found"

    if request.employeeId != employeeId:
        return "Unauthorized action"

    if request.status != "PENDING":
        showError("Only pending requests can be edited")
        return

    displayEditableForm(request)

    updatedData = getUpdatedData()

    action = getUserAction()

    if action == "withdraw":

        confirmation = prompt("Confirm withdrawal?")

        if confirmation == false:
            cancelOperation()
            return

        request.status = "WITHDRAWN"
        updateRequest(request)

        returnToHomePage()
        return "Withdraw successful"

    else if action == "edit":

        isValid = validateData(updatedData)

        if isValid == false:
            showErrors()
            return

        updateRequestData(requestId, updatedData)

        returnToHomePage()
        return "Update successful"
```

## (11) Imagine the UI of Requests displayed to the Manager or Employee :- 

### (11.1) Employee UI :- 

<img width="1536" height="1024" alt="employee ui" src="https://github.com/user-attachments/assets/880ee5df-9328-43d8-87fd-f9cac9816b7e" />

### (11.2) Manager UI :-

<img width="1536" height="1024" alt="manager ui" src="https://github.com/user-attachments/assets/51263cd7-d111-4fc0-8032-c5abbcb773a3" />

## (12) the state machine of the request :- 

<img width="752" height="800" alt="state machine digaram drawio" src="https://github.com/user-attachments/assets/540e2a6f-a227-4cc3-ad2a-66dda06c01a0" />




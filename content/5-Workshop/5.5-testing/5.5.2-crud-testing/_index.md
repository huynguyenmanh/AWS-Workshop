---
title: "Testing Task & Note Management (CRUD & DynamoDB)"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

In this section, we conduct end-to-end testing for all CRUD (Create, Read, Update, Delete) operations on tasks and notes directly through the Web UI, and verify real-time data persistence inside the Amazon DynamoDB database.

---

#### 1. Testing Web UI CRUD Operations & Kanban Board Drag-and-Drop

* **Execution Steps:**
  1. Click the **Create New Task** button on the web interface to initialize a new task/note.
  2. Enter the Title, Description, Category, Tags, and Due Date fields.
  3. Click **Save Task** to persist the record.
  4. Perform drag-and-drop state transitions by dragging a task card from *In Progress* to *Completed* on the **Kanban Board**.
  5. Edit an existing task's description and test the **Delete** action on a target note.

* **Expected Results:**
  * The UI updates smoothly, with task cards transitioning between status columns instantly.
  * Backend API Gateway calls (`POST /tasks`, `PUT /tasks/{id}`, `DELETE /tasks/{id}`) respond with HTTP Status Code **`200 OK`** or **`201 Created`**.

![Web UI CRUD Operations](../../../images/5-Workshop/5.5-testing/03-crud-kanban.jpg)

---

#### 2. Amazon DynamoDB Data State Verification

* **Execution Steps:**
  1. Navigate to the **Amazon DynamoDB Console** -> Select **Explore items**.
  2. Select the **`TodoNotesTable`** table.
  3. Inspect the persisted items list.

* **Expected Results:**
  * The DynamoDB table stores all task attributes correctly: `userId` (Partition Key), `taskId` (Sort Key), `title`, `description`, `status`, `category`, `createdAt`, and `updatedAt`.
  * Multi-tenancy access control isolates records strictly belonging to the authenticated Cognito user's `userId`.

![DynamoDB Item Verification](../../../images/5-Workshop/5.5-testing/04-dynamodb.jpg)
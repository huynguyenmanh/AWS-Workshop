---
title: "Testing Task & Note Management (CRUD Operations)"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

In this section, we conduct end-to-end testing for all CRUD (Create, Read, Update, Delete) operations on tasks and notes, including attachment uploads via S3 Presigned URLs and real-time Kanban board state updates.

---

#### 1. Testing Task Creation & Image Upload (Create & Attach Image)

* **Execution Steps:**
  1. Click the **Create New Task** button on the web interface.
  2. Fill in the Title, Description, Category, Tags, and Due Date fields.
  3. Attach an image file (`.png` / `.jpg`). The frontend requests an S3 Presigned URL and uploads the file directly to the **S3 Attachments Bucket**.
  4. Click **Save Task**.

* **Expected Results:**
  * The `POST /tasks` HTTP request completes with status **`201 Created`**.
  * A new item is persisted in the **Amazon DynamoDB (`TodoNotesTable`)** table containing the user's `userId` (Partition Key) and `taskId` (Sort Key).
  * The image thumbnail renders correctly on the task card.

![Create Task & Upload Image](/images/5-Workshop/5.5-testing/create-task.png)

---

#### 2. Testing Task Listing, Filtering & Searching (Read & Query)

* **Execution Steps:**
  1. Navigate to the main dashboard.
  2. Switch between different layout views: **List View**, **Kanban Board**, and **Calendar View**.
  3. Enter keywords in the **Search Bar** and filter items by Tag or Status.

* **Expected Results:**
  * The `GET /tasks` API fetches and returns only the data records belonging to the authenticated user.
  * Real-time search and filter functionalities respond accurately.

![Task List View & Kanban Filters](/images/5-Workshop/5.5-testing/read-tasks-kanban.png)

---

#### 3. Testing Task Updates & Kanban Drag-and-Drop (Update)

* **Execution Steps:**
  1. Edit an existing task's title or description, then click **Update**.
  2. On the **Kanban Board**, drag and drop a task card from the *In Progress* column to the *Completed* column.

* **Expected Results:**
  * The `PUT /tasks/{id}` API responds with HTTP **`200 OK`**.
  * The `status` attribute in DynamoDB updates seamlessly to `COMPLETED`.

![Updating Task Status via Drag-and-Drop](/images/5-Workshop/5.5-testing/update-task-kanban.png)

---

#### 4. Testing Task Deletion (Delete)

* **Execution Steps:**
  1. Click the **Delete** button on a target task card.
  2. Confirm the deletion action in the popup modal.

* **Expected Results:**
  * The `DELETE /tasks/{id}` API responds with HTTP **`200 OK`**.
  * The record is permanently removed from the DynamoDB table.

![Task Deletion Success](/images/5-Workshop/5.5-testing/delete-task.png)
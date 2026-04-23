# DelhiMeetup_UiPathIXP-25Apr2026
This repository will contain the artifacts and guides along with the solutions showcased in the UiPath Delhi Community Meetup held on 25 April 2026

# 🧾 UiPath IXP — Employee Expense Reimbursement Automation
### Hands-On Session Guide

This guide walks you through building an end-to-end **Intelligent Document Processing** solution using **UiPath IXP (Intelligent Xtraction & Processing)**. You will configure an IXP project to classify and extract data from employee expense documents, then wire it up to an RPA flow in Studio Web.

---

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Section 1 — Enabling IXP](#section-1--enabling-ixp)
3. [Section 2 — Building the IXP Project](#section-2--building-the-ixp-project)
   - [Step 1: Launch IXP & Create a Project](#step-1-launch-ixp--create-a-project)
   - [Step 2: Download the Dataset](#step-2-download-the-dataset)
   - [Step 3: Generate Taxonomy via Autopilot](#step-3-generate-taxonomy-via-autopilot)
   - [Step 4: Configure the Model](#step-4-configure-the-model)
   - [Step 5: Validate & Annotate Predictions](#step-5-validate--annotate-predictions)
   - [Step 6: Publish the IXP Project](#step-6-publish-the-ixp-project)
4. [Section 3 — Running the RPA Flow](#section-3--running-the-rpa-flow)
   - [Step 1: Import the RPA Flow into Studio Web](#step-1-import-the-rpa-flow-into-studio-web)
   - [Step 2: Create Storage Buckets in Orchestrator](#step-2-create-storage-buckets-in-orchestrator)
   - [Step 3: Upload Input Files & Run the Flow](#step-3-upload-input-files--run-the-flow)

---

## Prerequisites

Before getting started, make sure you have:

- A **UiPath Automation Cloud** account (Enterprise Trial or licensed)
- Access to the **GitHub repository** shared during the session (contains the dataset and `.uis` RPA flow file)
- A modern web browser (Chrome, Firefox, Edge, or Safari — latest version)

---

## Section 1 — Enabling IXP

> Skip this section if IXP is already available as a service in your tenant.

1. Sign up for an **Enterprise Trial license** at [cloud.uipath.com](https://cloud.uipath.com) if you do not already have one.
2. Once logged in, navigate to:
   **Automation Cloud → Admin → Tenant → Services**
3. Click **Add Service** and select **IXP** from the list.
4. IXP will now appear in your tenant's service menu.

> 💡 **Tip:** If Autopilot taxonomy generation or other IXP features appear unavailable later in the session, try removing IXP from the services list and re-adding it. This refreshes the service registration.

---

## Section 2 — Building the IXP Project

### Step 1: Launch IXP & Create a Project

1. From **Automation Cloud**, click on the **IXP** service to launch it.
2. In the IXP home screen, select **Unstructured & Complex Documents**.
3. Click **Create Project** and enter a meaningful project name (e.g., `Employee Expense Reimbursement`).

---

### Step 2: Download the Dataset

You will need sample expense documents to upload into the project before generating the taxonomy.

1. Open the **GitHub repository** using the shared link.
2. Click the green **Code** button, then click **Download ZIP** from the dropdown.
3. Extract the ZIP file on your local machine.
4. Inside the extracted folder, locate the **dataset** directory — it contains five sub-folders, one per document category:
   - `Hotel Receipt`
   - `Dining / Restaurant Bill`
   - `Flight Ticket`
   - `Cab Receipt`
   - `WiFi / Internet Bill`

---

### Step 3: Generate Taxonomy via Autopilot

1. Inside your newly created IXP project, look for the **Autopilot taxonomy generation** option.
2. **Before generating**, upload **one sample document from each category folder** in the dataset (5 documents total — one per expense type). This gives Autopilot the context it needs to generate an accurate taxonomy.
3. Paste the following prompt into the Autopilot input field:

---

```
I am building an Employee Expense Reimbursement automation using UiPath IXP.
The system will process scanned/photographed expense documents submitted by employees.

I want to first classify the document type based on inferred text, then extract key
details in each document type.

Following are the 5 document types that will be provided. Attached are the sample
documents for your reference:

1. Hotel Receipt
2. Dining / Restaurant Bill
3. Flight Ticket (e-ticket or printed)
4. Cab Receipt (Ola / Uber / printed slip)
5. WiFi / Internet Bill

Note: There should be a main field group as "Expense Document" and various different
sub field groups based on the document type (inferred text), with sub-fields based on
the key values to extract in each document.
```

---

4. Click **Generate with Autopilot** to generate the taxonomy.
5. Once generation is complete, **review the output**:
   - Confirm that **Expense Document** appears as the **top-level field group**.
   - Ensure the five document-type sub-groups and their respective fields are correctly listed beneath it.
6. If the structure looks correct, click **Confirm** to accept the taxonomy.
7. Click **Continue** to proceed to the document validation and annotation view.

---

### Step 4: Configure the Model

1. Inside the IXP project, navigate to **Model Configuration**.
2. Set the following options:
   | Setting | Value |
   |---|---|
   | **Intelligent Pre-processing** | Table Model Mini |
   | **Extraction Model** | GPT-4o |
3. Click **Save** to apply the configuration.
4. **Wait** for the model to finish loading before proceeding — you will see a status indicator while it initialises.

---

### Step 5: Validate & Annotate Predictions

1. Once the model is ready, go to the **Validate Predictions** tab.
2. IXP will display its predicted extractions for each uploaded document.
3. For each document:
   - **Review** every predicted field value carefully.
   - **Correct** any incorrect predictions by editing the field value directly.
   - If a field instruction is consistently producing wrong results, update the field's instruction in **Manage Taxonomy** to better describe what should be extracted, then re-run predictions.
   - Click the **tick boxes** to confirm individual predictions.
   - Click **Submit Annotations** once all fields in the document are reviewed.
4. Repeat this process for **all uploaded documents**.

> 📌 **Important:** Annotating predictions in IXP **refines field instructions** and improves model scoring — it does **not** train the model in the traditional ML sense. The more accurately you annotate, the more meaningful your project score will be.

5. Once all documents are annotated, check your **project score** displayed in the Build tab. A score labelled **Good** or **Excellent** indicates a healthy model ready for publishing.

---

### Step 6: Publish the IXP Project

1. Navigate to the **Publish** tab.
2. Click **Publish** to save a snapshot of the current model state.
3. The published version will now be discoverable by Studio automation developers within your tenant.

> 💡 You can add a description to the published version and tag it as **Staging** or **Production** using the ellipsis (⋯) menu next to the version.

---

## Section 3 — Running the RPA Flow

### Step 1: Import the RPA Flow into Studio Web

1. From the GitHub repository (downloaded in Section 2, Step 2), locate the **`.uis` RPA flow file**.
2. Open **UiPath Studio Web** from Automation Cloud.
3. Import the `.uis` file into Studio Web.
4. Inside the flow, locate the **Extract Document Data** activity.
5. **Link your published IXP project** to this activity by selecting it from the project picker.

---

### Step 2: Create Storage Buckets in Orchestrator

The RPA flow uses two Orchestrator Storage Buckets — one for input documents and one for extraction results.

1. Navigate to **Orchestrator → your Personal Workspace**.
2. Go to **Storage Buckets** and create the following two buckets (names are case-sensitive):

   | Bucket Name | Purpose |
   |---|---|
   | `IXPDocs` | Holds the input expense documents to be processed |
   | `IXPDocExtract` | Stores the extraction output results |

---

### Step 3: Upload Input Files & Run the Flow

1. Open the **`IXPDocs`** storage bucket in Orchestrator.
2. Upload the **input expense document files** from the dataset folder (the same documents used during IXP project configuration).
3. Return to **Studio Web** and run the RPA flow.
4. Once the run completes, navigate to the **`IXPDocExtract`** storage bucket in Orchestrator.
5. Open the output file(s) to verify the extracted data — field values should match what IXP predicted and what you validated during annotation.

---

## 🎉 You're Done!

You have successfully:
- ✅ Enabled IXP on your Automation Cloud tenant
- ✅ Built and configured an IXP project with Autopilot taxonomy generation
- ✅ Validated and annotated model predictions
- ✅ Published the IXP project
- ✅ Integrated the IXP project into an RPA flow in Studio Web
- ✅ Run an end-to-end expense document extraction pipeline

---

> For questions or issues during the session, reach out to the session facilitator or reach out to [me](https://www.linkedin.com/in/enits1712/).

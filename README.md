HR Workflow Designer – Prototype (React + React Flow)

A modular, scalable HR Workflow Designer that enables HR admins to visually create and test internal workflows such as employee onboarding, leave approval, and document verification processes.

This prototype demonstrates strong React architecture, React Flow proficiency, dynamic form handling, mock API integration, and workflow simulation.

📌 Features
1. Workflow Canvas (React Flow)

Drag-and-drop canvas to build HR workflows

Custom node types:

Start Node

Task Node

Approval Node

Automated Step Node

End Node

Connect/disconnect nodes

Delete nodes and edges

Basic workflow validation

Auto-selection → opens configuration form panel

2. Node Configuration Panel

Every node supports editable dynamic forms using controlled components.

Start Node

Title

Metadata (key-value pairs)

Task Node

Title (required)

Description

Assignee

Due date

Custom fields (dynamic key-value pairs)

Approval Node

Title

Approver Role (Manager, HRBP, Director, etc.)

Auto-approve threshold (number)

Automated Step Node

Title

Select automation action (from mock API)

Dynamic parameter fields based on action definition
(e.g., email → to, subject)

End Node

Completion message

Show-summary flag (toggle)

Forms are modular and extensible—entire node types can be added without modifying the core canvas logic.

3. Mock API Layer

API Layer built using MSW / local mocks.

GET /automations

Returns:

[
  { "id": "send_email", "label": "Send Email", "params": ["to", "subject"] },
  { "id": "generate_doc", "label": "Generate Document", "params": ["template", "recipient"] }
]

POST /simulate

Accepts full workflow graph JSON → returns mocked execution logs.

4. Workflow Simulation Panel

Serializes the workflow (nodes + edges)

Sends to /simulate

Shows execution steps sequentially

Validates:

Missing connections

Cycles

Start/End structure

Helps test flow correctness

🧱 Architecture Overview
src/
│── api/
│   ├── automations.ts
│   ├── simulate.ts
│   └── mockServer.ts
│
│── components/
│   ├── Canvas/
│   │   ├── WorkflowCanvas.tsx
│   │   ├── NodeTypes/
│   │   │   ├── StartNode.tsx
│   │   │   ├── TaskNode.tsx
│   │   │   ├── ApprovalNode.tsx
│   │   │   ├── AutomatedNode.tsx
│   │   │   └── EndNode.tsx
│   └── Forms/
│       ├── StartForm.tsx
│       ├── TaskForm.tsx
│       ├── ApprovalForm.tsx
│       ├── AutomatedForm.tsx
│       └── EndForm.tsx
│
│── context/
│   ├── WorkflowContext.tsx
│
│── hooks/
│   ├── useNodeConfigurator.ts
│   ├── useWorkflowValidator.ts
│   └── useAutomationActions.ts
│
│── utils/
│   ├── graphUtils.ts
│   ├── validationUtils.ts
│   └── workflowSerializer.ts
│
│── pages/App.tsx
│── main.tsx

Why this architecture?

✔ Clear separation of concerns
✔ Canvas logic isolated from form logic
✔ Extensible node system — easy to add new node types
✔ Abstraction layers for API, workflow, and validation
✔ Reusable hooks and components
✔ Strong typing ensures UI reliability

🛠️ Tech Stack

React (Vite)

React Flow for graph canvas

TypeScript

MSW / JSON mocks for API

Context + Hooks based global store

CSS Modules / Tailwind (depending on setup)

▶️ How to Run the Project
git clone <your-repo-url>
cd hr-workflow-designer
npm install
npm run dev


Mock API starts automatically via MSW.

📡 API Endpoints
GET /automations

Returns list of automated workflow actions.

POST /simulate

Request body:

{
  "nodes": [...],
  "edges": [...]
}


Response:

{
  "logs": [
    "Start → Employee Onboarding",
    "Task: Collect documents",
    "Approval: Manager approval",
    "Automated: send_email executed",
    "Workflow Completed"
  ]
}

🧪 Testing Workflow

Create nodes by dragging from the left sidebar

Connect nodes in valid order

Select a node → configure it

Open Simulation Panel

Click Run Workflow

View execution logs

⚙️ Design Decisions
Why React Flow?

Perfect for:

Custom nodes

Edge logic

Real-time flow editing

Scalable graph structure

Why MSW over JSON server?

Runs entirely in-browser

Zero backend required

Easy to expand for real API integration later

Why Form-by-Node architecture?

Each node is encapsulated and easily replaceable.
All forms follow a predictable interface → scalable for enterprise workflows.

🔮 Future Enhancements (If more time permitted)

Export / Import workflow JSON

Undo/Redo

Version history per node

Auto-layout / intelligent line routing

Role-based validation

Visual error badges on invalid nodes

Reusable node templates

Multi-branch approval logic

# Job Portal System – Backend API Testing (Postman)

Manual API testing of the **Admin module** of a *Web-Based Recruitment & Application Management Platform (Job Portal System)*. The backend is built with Node.js/Express and MongoDB, and was tested with Postman.

**66 test cases · 4 admin modules · 66 passed · RBAC, authentication, validation and error-handling coverage**

---

## Project Overview

The Job Portal lets employers post jobs and job seekers apply, while administrators manage users, employers, jobs and reports. I acted as the QA engineer for the admin backend: I designed the test cases, executed them against the running API, verified data changes in the database, and documented the results in formal QA reports.

## Modules Tested

| Module | Route file | Test cases | Passed | Report |
|---|---|:---:|:---:|---|
| Admin User Management | `adminUser.routes.js` | 20 | 20 | [PDF](Reports/QA_Report_AdminUser_Route.pdf) |
| Admin Employer Management | `adminEmployer.routes.js` | 16 | 16 | [PDF](Reports/QA_Report_AdminEmployer_Route.pdf) |
| Admin Job Management | `adminJob.routes.js` | 15 | 15 | [PDF](Reports/QA_Report_AdminJob_Route.pdf) |
| Admin Report Management | `adminReport.routes.js` | 15 | 15 | [PDF](Reports/QA_Report_AdminReport_Route.pdf) |
| **Total** | | **66** | **66** | |

## What I Tested

- **Functional testing**: listing with pagination, search and filtering, statistics endpoints, single-record retrieval, create and update, and status or moderation workflows
- **Authentication**: valid token vs. missing token (`401 Unauthorized`)
- **Role-Based Access Control (RBAC)**: Admin/Superadmin allowed; Employer and Job Seeker rejected (`403 Forbidden`)
- **Input validation**: empty request bodies, missing required fields, invalid enum/status values, invalid query parameters (`400 Bad Request`)
- **Negative and edge cases**: malformed IDs (`400`), non-existent IDs (`404`), duplicate email on user creation
- **Data verification**: confirmed that PATCH operations actually changed the data in MongoDB Atlas

## Sample Test Cases

From the Admin Report module:

| ID | Scenario | Endpoint | Expected | Actual | Status |
|---|---|---|---|---|---|
| TC001 | Get all reports (valid Admin token) | `GET /api/v1/admin/reports` | 200 OK | 200 OK | PASS |
| TC002 | Get all reports, no token | `GET /api/v1/admin/reports` | 401 | 401 | PASS |
| TC003 | Get all reports, Employer role | `GET /api/v1/admin/reports` | 403 | 403 | PASS |
| TC009 | Get report, non-existent ID | `GET /api/v1/admin/reports/:id` | 404 | 404 | PASS |
| TC012 | Review report, invalid status value | `PATCH /api/v1/admin/reports/:id/review` | 400 | 400 | PASS |

Every report contains the full test case table (ID, endpoint, input, expected vs. actual result), Postman screenshots, and a bugs/conclusion section.

## Notable Finding

While testing the Employer listing filter (TC005/TC006), the filter appeared to be ignored. Investigating the validation schema showed that the correct query parameter is `status`, not `verificationStatus`. After retesting with the right parameter, filtering and validation worked correctly. This was a testing-parameter issue, not a functional defect. It also showed that the API **silently ignores unknown query parameters** instead of rejecting them, which is worth considering as a design improvement.

## Tools & Technologies

| Tool | Purpose |
|---|---|
| Postman | API request testing, Bearer token authentication |
| MongoDB Atlas | Verifying data changes after write operations |
| VS Code | Reading route definitions and validation logic |
| Node.js / Express (local server) | Running the backend and checking server logs |
| Git & GitHub | Version control and publishing |

## Skills Demonstrated

API testing · Postman · REST / HTTP status codes · JWT/Bearer authentication · RBAC testing · positive and negative testing · input validation testing · test case design · QA documentation · database verification · root-cause analysis

## Repository Structure

```
jobportal-api-testing-postman/
├── README.md
└── Reports/
    ├── QA_Report_AdminUser_Route.pdf
    ├── QA_Report_AdminEmployer_Route.pdf
    ├── QA_Report_AdminJob_Route.pdf
    └── QA_Report_AdminReport_Route.pdf
```

## Note on Test Data

All testing was done on a local development environment (`localhost:5000`) with test data. Authentication tokens in the screenshots are masked.

## Related Projects

This is the first of my QA projects. See my other work:
- [QA Testing Portfolio](https://github.com/Kawya45/QA-Testing-Portfolio)
- *(Automation testing projects: links coming soon)*

## Author

**Kawya Jinasoma**, Computer Science student, aspiring QA Engineer
GitHub: [@Kawya45](https://github.com/Kawya45) ·

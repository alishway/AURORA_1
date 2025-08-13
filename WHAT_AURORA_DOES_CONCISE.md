### What your application does (concise)

- **Frontend**: React + TypeScript + shadcn-ui; routes in `src/App.tsx` to modules like `DataExplorer`, `Relations`, `Cases`, `Monitoring`, `Geolocation`, `Admin`.
- **Auth/RBAC**: Supabase auth + `profiles` table with `user_role` and `organization_id`. `useAuth` enforces login, active user/org checks via RPCs, and exposes role helpers for UI gating.
- **Admin**:
  - **AURORA Admin** manages organizations and all users (`OrganizationManagement`).
  - **Organization Admin** manages users within their org (`UserManagement`).
  - Uses edge functions for creating/deleting users and organization cascades.
- **Data Explorer**:
  - AI search via `ai-search` edge function with keyword/entity heuristics + RLS-aware queries across public tables and secure uploads.
  - Secure uploads via `process-uploaded-file` with classification and storage mode; stores preview rows; full audit logging to `data_access_audit`.
  - Web scraping placeholder.
- **Secure data handling**:
  - `secure_uploaded_data` with enums `data_classification` and `storage_mode`, RLS policies (per docs), and audit trail in `data_access_audit`.
  - Phase 1 emergency fix: `ai-search` now uses anon key with user Authorization header; adds audit logging and runtime access checks.
- **Monitoring/Security**:
  - Security Implementation Guide details alerts and triggers; types include `security_alerts` and monitoring views.
- **Domain goals (docs)**: Entity resolution & relations, case management, fugitive tracking via LLM, address tokenizer/geolocation, real-time monitoring, explainable AI, compliance.

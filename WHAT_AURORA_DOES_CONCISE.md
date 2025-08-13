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

### Notable strengths

- **Clean modular UI** with role-based visibility in `Index.tsx` and clear routing in `src/App.tsx`.
- **Auth guard and RBAC**: login required, inactive user/org checks via RPCs, helpers expose `isAuroraAdmin`, `isOrgAdmin`, `canManageUsers`, etc.
- **Secure uploads design** with classification, storage mode, retention, preview data, and comprehensive audit logging.
- **AI search** enforces user context, respects RLS, applies runtime access checks, and logs audits.
- **Error tracking** system initialized globally with contextual reports and diagnostics.

### Potential issues or clarifications

- **Role field mismatch in ai-search**: references `userProfile.role` but `profiles` uses `user_role`. Align usage and response payload.
- **Fallback tables in DataExplorer**: references non-existent tables (e.g., `social_media_data`, `residential_addresses`). Align with actual tables (`twitter_users`, `facebook_users`, `instagram_users`, `linkedin_users`, `residential_address`).
- **Supabase env defaults**: client includes default URL/anon key; confirm production uses env-only and remove hardcoded fallbacks.
- **Encryption at rest**: `encrypted_data` is currently raw bytea; Phase 1B to add real encryption. Confirm timeline/KMS approach.
- **RLS policy snippets**: migration guide contains typos/partials; verify deployed policies match intended access rules.
- **Relevance scoring**: ai-search heuristic may need tuning and table weighting for noisy queries.
- **LLM features**: fugitive tracking/Explainable AI specified in docs but not yet wired to a provider.

### Targeted questions to align and proceed

- **Roles/RBAC**
  - Should `ai-search` use `user_role` everywhere (and in the JSON response)? Any custom roles beyond the enum planned?
  - Confirm storage-mode access rules: org admins access `case_evidence` org-wide, regular users only own + shared (`intelligence_vault`/`collaborative_workspace`)?
- **Secure uploads**
  - Is encryption at rest (KMS/envelope) planned in Phase 1B for `encrypted_data`? Which key management approach?
  - Any org-level retention overrides or legal holds beyond current defaults?
- **AI search**
  - Keep current keyword/entity heuristic for MVP or integrate a provider (OpenAI/Azure/open-source LLM + RAG) soon?
  - Prioritize weighting of tables/sources (e.g., `interpol_data` > social)?
  - Add fielded queries (name:"…", city:"…"), fuzzy search, and date range filters in first iteration?
- **Data Explorer**
  - Confirm correct fallback search tables/fields so I can align `performBasicSearch`.
  - Any UI constraints to show file-origin and access reason metadata for compliance?
- **Entity relations**
  - Start with client-side graph (e.g., Cytoscape.js) fed by joins + `secure_uploaded_data.preview_data`? Target datasets and minimal matching rules?
- **Case management**
  - MVP scope: create/view cases, attach records/queries; any near-term reporting/export requirements?
  - Should `case_evidence` uploads auto-link to a selected `case_id`?
- **Monitoring/alerts**
  - Are `security_alerts` triggers deployed in prod? Initial alert thresholds?
  - Build in-app alert center first or also email/SMS integration?
- **Compliance**
  - Immediate GDPR/PIPEDA features to implement (access/export/delete, minimization, consent)?
  - Should audit logs (IP, user-agent) be visible in UI to admins or backend-only?
- **Deployment/secrets**
  - How are prod envs managed (Vite env, Supabase secrets, CI/CD)? Switch client to require env-only values and remove defaults?
- **Scale/performance**
  - Expected volumes for uploads/core tables; target AI search latency; on-prem/air-gapped constraints?

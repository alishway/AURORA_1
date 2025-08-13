Targeted questions to align and proceed
Roles/RBAC
• Should ai-search use user_role everywhere (and in the JSON response)? Any custom roles beyond the enum planned?
• Confirm storage-mode access rules: org admins access case_evidence org-wide, regular users only own + shared (intelligence_vault/collaborative_workspace)?
Secure uploads
• Is encryption at rest (KMS/envelope) planned in Phase 1B for encrypted_data? Which key management approach?
• Retention policies: any org-level overrides or legal holds beyond the current defaults?
AI search
• Do you want to keep the current keyword/entity approach for MVP or integrate a provider (OpenAI/Azure/Open source LLM + RAG) soon?
• Any additional tables/sources to prioritize in ranking (e.g., weight INTERPOL > social)?
• Should we support fielded queries (e.g., name:”…”, city:”…”), fuzzy, or date range filters in the first iteration?
Data Explorer
• Confirm correct fallback search tables and desired fields per table so I can align performBasicSearch.
• Any UI constraints on showing file-origin and access reason metadata in results for compliance?
Entity relations
• For the relations module, should we start with client-side graph (Cytoscape.js) fed by joins across existing tables and secure_uploaded_data.preview_data?
• Minimum viable entity resolution rules (name/email/phone blocking + similarity thresholds) and target datasets for a first demo?
Case management
• Confirm current MVP: create/view cases tied to cases table and attach records/queries. Any near-term reporting/export requirements?
• Should case_evidence storage mode automatically link uploads to a selected case_id?
Monitoring/alerts
• Are security_alerts triggers deployed in prod? Which alert thresholds should we start with?
• Do you want an in-app alert center first or email/SMS integration too?
Compliance
• Any specific policies we must implement (GDPR/PIPEDA) now: right to access/export/delete, data minimization toggles, consent banners?
• Should audit logs expose IP and user-agent to admins in UI, or remain backend-only?
Deployment/secrets
• How are you managing env for production (Vite env vars, Supabase dashboard secrets, CI/CD)? Should we switch the client to require env-only values and remove defaults?
Scale/performance
• Expected data volumes for uploaded previews and core tables? Target response times for AI search?
• Any constraints around air-gapped/on-prem deployments?

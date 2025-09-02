# AURORA Case Management Phase 1 Test Plan
## Entity-Relationship Foundation Testing

### 🎯 Testing Objectives
- Verify core case management functionality
- Validate entity extraction and linking
- Ensure Relations-ready architecture
- Confirm no regression in existing features

---

## 📊 Test Categories

### **A. Database Integration Tests**

#### **A1: Case CRUD Operations**
```typescript
describe('Case Management - Database Integration', () => {
  test('TC-A1-001: Create new case with all required fields', async () => {
    // Given: Valid case data with entities
    const caseData = {
      title: 'Financial Fraud Investigation',
      description: 'Suspected embezzlement at XYZ Corp',
      case_type: 'fraud',
      priority: 'high',
      incident_date: '2024-01-15',
      primary_entities: ['john-doe-suspect', 'xyz-corp-org']
    };
    
    // When: Creating case
    const result = await caseService.createCase(caseData);
    
    // Then: Case created successfully with entities linked
    expect(result.id).toBeDefined();
    expect(result.case_number).toMatch(/^CASE-\d{8}-\d{4}$/);
    expect(result.primary_entities).toHaveLength(2);
    
    // Verify database state
    const dbCase = await supabase.from('cases').select('*').eq('id', result.id);
    expect(dbCase.data[0].title).toBe(caseData.title);
    
    // Verify entity linking
    const linkedEntities = await supabase
      .from('case_entities')
      .select('*')
      .eq('case_id', result.id);
    expect(linkedEntities.data).toHaveLength(2);
  });

  test('TC-A1-002: Update case preserves entity relationships', async () => {
    // Given: Existing case with entities
    const existingCase = await createTestCase();
    
    // When: Updating case details
    const updatedData = { description: 'Updated investigation scope' };
    const result = await caseService.updateCase(existingCase.id, updatedData);
    
    // Then: Case updated, entities preserved
    expect(result.description).toBe(updatedData.description);
    expect(result.primary_entities).toEqual(existingCase.primary_entities);
  });

  test('TC-A1-003: Delete case cleans up entity relationships', async () => {
    // Given: Case with multiple entity relationships
    const testCase = await createTestCaseWithEntities();
    
    // When: Deleting case
    await caseService.deleteCase(testCase.id);
    
    // Then: Case and relationships removed
    const deletedCase = await supabase.from('cases').select('*').eq('id', testCase.id);
    expect(deletedCase.data).toHaveLength(0);
    
    const orphanedLinks = await supabase
      .from('case_entities')
      .select('*')
      .eq('case_id', testCase.id);
    expect(orphanedLinks.data).toHaveLength(0);
  });
});
```

#### **A2: Entity Management Tests**
```typescript
describe('Entity Management', () => {
  test('TC-A2-001: Extract entities from case description', async () => {
    // Given: Case description with identifiable entities
    const description = 'John Smith from ABC Company called about suspicious activity at 123 Main St';
    
    // When: Extracting entities
    const entities = await entityService.extractEntities(description);
    
    // Then: Entities identified correctly
    expect(entities).toContainEqual(
      expect.objectContaining({
        entity_type: 'person',
        primary_identifier: 'John Smith',
        confidence_score: expect.any(Number)
      })
    );
    expect(entities).toContainEqual(
      expect.objectContaining({
        entity_type: 'organization',
        primary_identifier: 'ABC Company'
      })
    );
    expect(entities).toContainEqual(
      expect.objectContaining({
        entity_type: 'location',
        primary_identifier: '123 Main St'
      })
    );
  });

  test('TC-A2-002: Merge duplicate entities with confidence scoring', async () => {
    // Given: Two similar entities
    const entity1 = await createTestEntity({ primary_identifier: 'John Smith' });
    const entity2 = await createTestEntity({ primary_identifier: 'J. Smith' });
    
    // When: Running entity resolution
    const result = await entityService.resolveEntities([entity1.id, entity2.id]);
    
    // Then: Entities merged with confidence score
    expect(result.merged_entity).toBeDefined();
    expect(result.confidence_score).toBeGreaterThan(0.7);
    expect(result.merged_entity.aliases).toContain('J. Smith');
  });
});
```

### **B. Relations Integration Tests**

#### **B1: Relationship Detection**
```typescript
describe('Relationship Detection', () => {
  test('TC-B1-001: Detect relationships from case data', async () => {
    // Given: Case with multiple entities
    const caseData = {
      title: 'Employment Fraud Case',
      description: 'John Smith works for ABC Corp and knows Mary Johnson',
      entities: ['john-smith', 'abc-corp', 'mary-johnson']
    };
    
    // When: Creating case with relationship detection
    const result = await caseService.createCaseWithRelationships(caseData);
    
    // Then: Relationships automatically detected
    const relationships = await relationshipService.getCaseRelationships(result.id);
    expect(relationships).toContainEqual(
      expect.objectContaining({
        source_entity: 'john-smith',
        target_entity: 'abc-corp',
        relationship_type: 'works_for'
      })
    );
    expect(relationships).toContainEqual(
      expect.objectContaining({
        source_entity: 'john-smith',
        target_entity: 'mary-johnson',
        relationship_type: 'knows'
      })
    );
  });

  test('TC-B1-002: Calculate relationship strength over time', async () => {
    // Given: Multiple interactions between entities
    const interactions = [
      { date: '2024-01-01', type: 'phone_call' },
      { date: '2024-01-05', type: 'meeting' },
      { date: '2024-01-10', type: 'email' }
    ];
    
    // When: Calculating relationship strength
    const strength = await relationshipService.calculateStrength(
      'entity-1', 'entity-2', interactions
    );
    
    // Then: Strength increases with interactions
    expect(strength).toBeGreaterThan(0.5);
    expect(strength).toBeLessThanOrEqual(1.0);
  });
});
```

### **C. User Interface Tests**

#### **C1: Case Creation Wizard**
```typescript
describe('Case Creation UI', () => {
  test('TC-C1-001: Enhanced case creation form validation', async () => {
    // Given: Case creation form
    render(<CaseCreationWizard />);
    
    // When: Filling required fields
    fireEvent.change(screen.getByLabelText('Case Title'), {
      target: { value: 'Test Investigation' }
    });
    fireEvent.change(screen.getByLabelText('Description'), {
      target: { value: 'John Smith investigation at ABC Corp' }
    });
    
    // Then: Entity suggestions appear
    await waitFor(() => {
      expect(screen.getByText('Detected Entities:')).toBeInTheDocument();
      expect(screen.getByText('John Smith (Person)')).toBeInTheDocument();
      expect(screen.getByText('ABC Corp (Organization)')).toBeInTheDocument();
    });
  });

  test('TC-C1-002: Entity linking interface', async () => {
    // Given: Case creation with detected entities
    render(<CaseCreationWizard />);
    
    // When: User confirms entity linking
    const entityCheckbox = screen.getByRole('checkbox', { name: /John Smith/ });
    fireEvent.click(entityCheckbox);
    
    // Then: Entity marked for inclusion
    expect(entityCheckbox).toBeChecked();
    expect(screen.getByText('Role in Case:')).toBeInTheDocument();
  });
});
```

### **D. Performance Tests**

#### **D1: Database Performance**
```typescript
describe('Performance Tests', () => {
  test('TC-D1-001: Case creation performance with entities', async () => {
    // Given: Large case data with multiple entities
    const largeCase = generateLargeCaseData(50); // 50 entities
    
    // When: Creating case
    const startTime = Date.now();
    const result = await caseService.createCase(largeCase);
    const duration = Date.now() - startTime;
    
    // Then: Performance within acceptable limits
    expect(duration).toBeLessThan(2000); // Under 2 seconds
    expect(result.primary_entities).toHaveLength(50);
  });

  test('TC-D1-002: Entity resolution performance', async () => {
    // Given: 1000 entities for resolution
    const entities = await createTestEntities(1000);
    
    // When: Running batch entity resolution
    const startTime = Date.now();
    const results = await entityService.batchResolveEntities(entities);
    const duration = Date.now() - startTime;
    
    // Then: Completes within acceptable time
    expect(duration).toBeLessThan(5000); // Under 5 seconds
    expect(results.processed_count).toBe(1000);
  });
});
```

### **E. Security & Compliance Tests**

#### **E1: Data Access Control**
```typescript
describe('Security Tests', () => {
  test('TC-E1-001: Case access restricted by organization', async () => {
    // Given: Cases from different organizations
    const org1Case = await createCaseForOrganization('org-1');
    const org2User = await createUserForOrganization('org-2');
    
    // When: User tries to access other org's case
    const result = await caseService.getCase(org1Case.id, org2User);
    
    // Then: Access denied
    expect(result).toBeNull();
  });

  test('TC-E1-002: Entity data privacy compliance', async () => {
    // Given: Entity with PII data
    const entity = await createEntityWithPII();
    
    // When: Accessing entity data
    const result = await entityService.getEntity(entity.id);
    
    // Then: PII properly masked/encrypted
    expect(result.attributes.ssn).toMatch(/\*\*\*-\*\*-\d{4}/);
    expect(result.has_pii).toBe(true);
  });
});
```

### **F. Integration Tests**

#### **F1: Existing Feature Compatibility**
```typescript
describe('Regression Tests', () => {
  test('TC-F1-001: Data Explorer integration unchanged', async () => {
    // Given: Existing Data Explorer functionality
    const searchQuery = 'John Smith';
    
    // When: Performing search
    const results = await searchService.search(searchQuery);
    
    // Then: Results include case creation option
    expect(results.actions).toContainEqual(
      expect.objectContaining({
        type: 'create_case',
        label: 'Create Case from Result'
      })
    );
  });

  test('TC-F1-002: User management unaffected', async () => {
    // Given: Existing user management
    const userData = { email: 'test@example.com', role: 'investigator' };
    
    // When: Creating user
    const result = await userService.createUser(userData);
    
    // Then: User creation works as before
    expect(result.email).toBe(userData.email);
    expect(result.role).toBe(userData.role);
  });

  test('TC-F1-003: API integrations preserved', async () => {
    // Given: Existing API sources
    const apiSource = await getTestAPISource();
    
    // When: Fetching data from API
    const data = await apiService.fetchData(apiSource.id);
    
    // Then: Data fetching works unchanged
    expect(data.status).toBe('success');
    expect(data.records).toBeDefined();
  });
});
```

---

## 🎯 **Success Criteria**

### **Functional Requirements:**
- ✅ All test cases pass with 95%+ success rate
- ✅ Case creation time < 2 seconds with 50 entities
- ✅ Entity resolution accuracy > 90%
- ✅ Zero regression in existing features

### **Performance Benchmarks:**
- ✅ Database queries < 500ms average response time
- ✅ UI responsiveness < 100ms for user interactions
- ✅ Memory usage < 100MB increase from baseline
- ✅ Support 1000+ concurrent users

### **Security & Compliance:**
- ✅ 100% RLS policy coverage
- ✅ All PII properly encrypted/masked
- ✅ Audit logging for all entity operations
- ✅ CJIS compliance validation

---

## 🔧 **Test Execution Strategy**

### **Automated Testing Pipeline:**
```yaml
# .github/workflows/case-management-tests.yml
name: Case Management Tests
on: [push, pull_request]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run Unit Tests
        run: npm run test:unit
      
  integration-tests:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:13
    steps:
      - name: Run Integration Tests
        run: npm run test:integration
      
  performance-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Run Performance Tests
        run: npm run test:performance
      
  security-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Run Security Tests
        run: npm run test:security
```

### **Manual Testing Checklist:**
- [ ] End-to-end case creation workflow
- [ ] Entity relationship visualization
- [ ] Cross-browser compatibility testing
- [ ] Mobile responsiveness validation
- [ ] Accessibility compliance (WCAG 2.1)
- [ ] Load testing with realistic data volumes

---

## 📊 **Test Data Management**

### **Test Data Sets:**
```typescript
// Test data generators
export const testDataSets = {
  smallCase: () => generateCase({ entities: 5, relationships: 3 }),
  mediumCase: () => generateCase({ entities: 25, relationships: 15 }),
  largeCase: () => generateCase({ entities: 100, relationships: 50 }),
  complexNetwork: () => generateNetworkCase({ 
    entities: 200, 
    relationships: 500,
    depth: 5 
  })
};
```

### **Test Environment Setup:**
```bash
# Setup test database
npm run test:db:setup

# Seed test data
npm run test:data:seed

# Run full test suite
npm run test:all

# Generate test report
npm run test:report
```

---

**Ready to begin Phase 1 development with this comprehensive test plan?** This ensures production-grade quality while building the foundation for advanced Relations integration! 🚀

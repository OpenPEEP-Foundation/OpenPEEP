---
layout: examples
title: OpenPEEP Examples
description: See real examples of PCFRA, PEEP, Emergency Evacuation Statements, and other OpenPEEP documents in both human-readable and technical formats. Interactive examples with toggle views.
keywords: PEEP examples, fire safety document examples, evacuation plan templates, PCFRA examples, emergency evacuation statement examples, fire compliance examples
---



<div class="examples-container">
    <div class="examples-sidebar">
        <h3>Document Types</h3>
        <nav class="examples-nav">
            <a href="#introduction" class="nav-item active">Introduction</a>
            <a href="#pcfra-examples" class="nav-item">PCFRA Examples</a>
            <a href="#peep-examples" class="nav-item">PEEP Examples</a>
            <a href="#ees-examples" class="nav-item">Emergency Evacuation Statement Examples</a>
            <a href="#consent-examples" class="nav-item">Consent Examples</a>
            <a href="#review-examples" class="nav-item">Review Examples</a>
        </nav>
    </div>

    <div class="examples-content">
        <div class="content-padding">
            <h1>OpenPEEP Examples</h1>
            
            
                <p><strong>Real documents, real scenarios, real compliance.</strong><br>
                Toggle between human-readable and technical views to see exactly what OpenPEEP produces.</p>
            
            
            <hr>
            
            <section id="introduction">
            <h2>Human-Readable Document Examples</h2>
            
            <p>OpenPEEP produces real documents that real people use in real emergencies. These examples show you exactly what compliance looks like in practice.</p>
            
            <p>Each example includes:</p>
            <ul>
                <li><strong>Real-world scenarios</strong> — Actual people with actual needs</li>
                <li><strong>Complete documents</strong> — Everything required for compliance</li>
                <li><strong>Validation status</strong> — Built-in compliance checking</li>
                <li><strong>Technical structure</strong> — The underlying data format</li>
            </ul>
            
            <div class="toggle-instructions">
                <p><strong>💡 Toggle between views:</strong> Use the "Human View" / "Technical View" buttons to see the same document in different formats.</p>
            </div>
        </section>

        <section id="pcfra-examples">
            <h2>Person-Centred Fire Risk Assessment (PCFRA) Examples</h2>
            
            <p>The PCFRA is the foundation of everything that follows. It assesses both the person and their environment to identify evacuation needs and risks.</p>

            <div class="example-card">
                <div class="example-header">
                    <h3>Example 1: Sarah Johnson - Wheelchair User</h3>
                    <div class="example-controls">
                        <button class="toggle-btn active" data-target="sarah-human">Human View</button>
                        <button class="toggle-btn" data-target="sarah-technical">Technical View</button>
                    </div>
                </div>

                <div id="sarah-human" class="example-content active">
                    <div class="document-preview">
                        <div class="document-header">
                            <h4>Person-Centred Fire Risk Assessment</h4>
                            <div class="document-status valid">✓ Validated & Compliant</div>
                        </div>
                        
                        <div class="document-section">
                            <h5>Resident Information</h5>
                            <div class="document-field">
                                <label>Name:</label>
                                <span>Sarah Johnson</span>
                            </div>
                            <div class="document-field">
                                <label>Date of Birth:</label>
                                <span>15 March 1985</span>
                            </div>
                            <div class="document-field">
                                <label>Room/Flat:</label>
                                <span>Room 42, Sunrise Gardens</span>
                            </div>
                            <div class="document-field">
                                <label>Mobility Status:</label>
                                <span>Wheelchair user (manual)</span>
                            </div>
                            <div class="document-field">
                                <label>Support Requirements:</label>
                                <span>Requires assistance for evacuation, cannot use stairs independently</span>
                            </div>
                        </div>

                        <div class="document-section">
                            <h5>Environmental Assessment</h5>
                            <div class="document-field">
                                <label>Building Type:</label>
                                <span>Residential care home</span>
                            </div>
                            <div class="document-field">
                                <label>Floor Level:</label>
                                <span>Ground floor (accessible)</span>
                            </div>
                            <div class="document-field">
                                <label>Evacuation Routes:</label>
                                <span>Primary: Direct exit to garden, Secondary: Through reception</span>
                            </div>
                            <div class="document-field">
                                <label>Fire Safety Equipment:</label>
                                <span>Evacuation chair available, Fire doors fitted</span>
                            </div>
                        </div>

                        <div class="document-section">
                            <h5>Risk Assessment</h5>
                            <div class="document-field">
                                <label>Primary Risk:</label>
                                <span>Limited mobility in emergency situation</span>
                            </div>
                            <div class="document-field">
                                <label>Mitigation Measures:</label>
                                <span>Assigned evacuation buddy, Evacuation chair training for staff, Regular equipment checks</span>
                            </div>
                            <div class="document-field">
                                <label>Emergency Contacts:</label>
                                <span>Next of kin: David Johnson (brother) - 07123 456789</span>
                            </div>
                        </div>

                        <div class="document-footer">
                            <div class="document-field">
                                <label>Assessment Date:</label>
                                <span>10 October 2025</span>
                            </div>
                            <div class="document-field">
                                <label>Assessor:</label>
                                <span>Jennifer Smith, Fire Safety Officer</span>
                            </div>
                            <div class="document-field">
                                <label>Review Due:</label>
                                <span>10 April 2026</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="sarah-technical" class="example-content">
                    <div class="code-preview">
                        <pre><code>{
  "$schema": "https://open-peep.org/schemas/pcfra/v1.0.0",
  "id": "pcfra-sarah-johnson-2025-10-10",
  "version": "1.0.0",
  "createdAt": "2025-10-10T14:30:00Z",
  "createdBy": {
    "name": "Jennifer Smith",
    "role": "Fire Safety Officer",
    "organisation": "Sunrise Gardens Care Home"
  },
  "resident": {
    "personalDetails": {
      "firstName": "Sarah",
      "lastName": "Johnson",
      "dateOfBirth": "1985-03-15",
      "preferredName": "Sarah"
    },
    "contact": {
      "room": "Room 42",
      "building": "Sunrise Gardens",
      "address": {
        "street": "123 Care Lane",
        "city": "Manchester",
        "postcode": "M1 2AB"
      }
    },
    "capabilities": {
      "mobility": {
        "status": "wheelchair_user",
        "type": "manual",
        "description": "Requires assistance for evacuation, cannot use stairs independently"
      },
      "communication": {
        "hearing": "good",
        "sight": "good",
        "speech": "clear"
      },
      "cognition": {
        "alertness": "alert",
        "orientation": "oriented",
        "memory": "good"
      }
    }
  },
  "environment": {
    "building": {
      "type": "residential_care_home",
      "floors": 2,
      "residentFloor": 0,
      "accessibility": "accessible"
    },
    "evacuationRoutes": [
      {
        "type": "primary",
        "description": "Direct exit to garden",
        "accessibility": "wheelchair_accessible"
      },
      {
        "type": "secondary",
        "description": "Through reception",
        "accessibility": "wheelchair_accessible"
      }
    ],
    "fireSafetyEquipment": [
      "evacuation_chair",
      "fire_doors",
      "fire_alarm",
      "emergency_lighting"
    ]
  },
  "riskAssessment": {
    "identifiedRisks": [
      {
        "type": "mobility_limitation",
        "severity": "medium",
        "description": "Limited mobility in emergency situation"
      }
    ],
    "mitigationMeasures": [
      "assigned_evacuation_buddy",
      "evacuation_chair_training",
      "regular_equipment_checks"
    ],
    "emergencyContacts": [
      {
        "name": "David Johnson",
        "relationship": "brother",
        "phone": "07123456789",
        "priority": "primary"
      }
    ]
  },
  "compliance": {
    "regulations": [
      "Fire Safety (Residential Evacuation Plans) (England) Regulations 2025",
      "Equality Act 2010",
      "Regulatory Reform (Fire Safety) Order 2005"
    ],
    "validationStatus": "valid",
    "lastValidated": "2025-10-10T14:30:00Z"
  },
  "review": {
    "assessmentDate": "2025-10-10",
    "nextReviewDue": "2026-04-10",
    "reviewFrequency": "6_months"
  }
}</code></pre>
                    </div>
                </div>
            </div>

            <div class="example-card">
                <div class="example-header">
                    <h3>Example 2: Robert Chen - Visual Impairment</h3>
                    <div class="example-controls">
                        <button class="toggle-btn active" data-target="robert-human">Human View</button>
                        <button class="toggle-btn" data-target="robert-technical">Technical View</button>
                    </div>
                </div>

                <div id="robert-human" class="example-content active">
                    <div class="document-preview">
                        <div class="document-header">
                            <h4>Person-Centred Fire Risk Assessment</h4>
                            <div class="document-status valid">✓ Validated & Compliant</div>
                        </div>
                        
                        <div class="document-section">
                            <h5>Resident Information</h5>
                            <div class="document-field">
                                <label>Name:</label>
                                <span>Robert Chen</span>
                            </div>
                            <div class="document-field">
                                <label>Room/Flat:</label>
                                <span>Flat 15B, Riverside Court</span>
                            </div>
                            <div class="document-field">
                                <label>Visual Status:</label>
                                <span>Partially sighted, uses cane for navigation</span>
                            </div>
                            <div class="document-field">
                                <label>Support Requirements:</label>
                                <span>Audio fire alarm, Tactile evacuation guidance, Assistance with unfamiliar routes</span>
                            </div>
                        </div>

                        <div class="document-section">
                            <h5>Environmental Assessment</h5>
                            <div class="document-field">
                                <label>Building Type:</label>
                                <span>Purpose-built sheltered housing</span>
                            </div>
                            <div class="document-field">
                                <label>Floor Level:</label>
                                <span>Second floor</span>
                            </div>
                            <div class="document-field">
                                <label>Evacuation Routes:</label>
                                <span>Primary: Stairwell A, Secondary: Lift (if safe)</span>
                            </div>
                            <div class="document-field">
                                <label>Special Provisions:</label>
                                <span>Audio fire alarm, High contrast signage, Tactile handrails</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="robert-technical" class="example-content">
                    <div class="code-preview">
                        <pre><code>{
  "$schema": "https://open-peep.org/schemas/pcfra/v1.0.0",
  "id": "pcfra-robert-chen-2025-10-10",
  "resident": {
    "capabilities": {
      "mobility": {
        "status": "independent_walking",
        "description": "Uses cane for navigation"
      },
      "communication": {
        "hearing": "good",
        "sight": "partially_sighted",
        "speech": "clear"
      }
    }
  },
  "environment": {
    "building": {
      "type": "sheltered_housing",
      "residentFloor": 2
    },
    "specialProvisions": [
      "audio_fire_alarm",
      "high_contrast_signage",
      "tactile_handrails"
    ]
  }
}</code></pre>
                    </div>
                </div>
            </div>
        </section>

        <section id="peep-examples">
            <h2>Personal Emergency Evacuation Plan (PEEP) Examples</h2>
            
            <p>The PEEP is the actual evacuation plan created from PCFRA data. It contains the specific procedures that will be followed in an emergency.</p>

            <div class="example-card">
                <div class="example-header">
                    <h3>Example: Sarah Johnson's PEEP</h3>
                    <div class="example-controls">
                        <button class="toggle-btn active" data-target="sarah-peep-human">Human View</button>
                        <button class="toggle-btn" data-target="sarah-peep-technical">Technical View</button>
                    </div>
                </div>

                <div id="sarah-peep-human" class="example-content active">
                    <div class="document-preview">
                        <div class="document-header">
                            <h4>Personal Emergency Evacuation Plan</h4>
                            <div class="document-status valid">✓ Validated & Compliant</div>
                        </div>
                        
                        <div class="document-section">
                            <h5>Evacuation Procedure</h5>
                            <div class="document-field">
                                <label>Primary Route:</label>
                                <span>Assisted via direct garden exit</span>
                            </div>
                            <div class="document-field">
                                <label>Support Required:</label>
                                <span>2 staff members + evacuation chair</span>
                            </div>
                            <div class="document-field">
                                <label>Assembly Point:</label>
                                <span>Garden area - designated wheelchair space</span>
                            </div>
                            <div class="document-field">
                                <label>Emergency Contacts:</label>
                                <span>David Johnson (brother): 07123 456789</span>
                            </div>
                        </div>

                        <div class="document-section">
                            <h5>Staff Responsibilities</h5>
                            <div class="document-field">
                                <label>Primary Evacuation Buddy:</label>
                                <span>Maria Rodriguez (Senior Carer)</span>
                            </div>
                            <div class="document-field">
                                <label>Secondary Support:</label>
                                <span>James Wilson (Care Assistant)</span>
                            </div>
                            <div class="document-field">
                                <label>Equipment Check:</label>
                                <span>Weekly evacuation chair inspection</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="sarah-peep-technical" class="example-content">
                    <div class="code-preview">
                        <pre><code>{
  "$schema": "https://open-peep.org/schemas/peep/v1.0.0",
  "id": "peep-sarah-johnson-2025-10-10",
  "version": "1.0.0",
  "pcfraId": "pcfra-sarah-johnson-2025-10-10",
  "resident": {
    "name": "Sarah Johnson",
    "room": "Room 42"
  },
  "evacuationProcedure": {
    "primaryRoute": {
      "description": "Assisted via direct garden exit",
      "accessibility": "wheelchair_accessible",
      "estimatedTime": "3_minutes"
    },
    "assemblyPoint": {
      "location": "Garden area - designated wheelchair space",
      "accessibility": "wheelchair_accessible"
    },
    "supportRequired": {
      "personnel": [
        {
          "name": "Maria Rodriguez",
          "role": "Senior Carer",
          "responsibility": "primary_buddy"
        },
        {
          "name": "James Wilson", 
          "role": "Care Assistant",
          "responsibility": "secondary_support"
        }
      ],
      "equipment": ["evacuation_chair"],
      "specialInstructions": "Use evacuation chair if lift unavailable"
    }
  },
  "emergencyContacts": [
    {
      "name": "David Johnson",
      "relationship": "brother",
      "phone": "07123456789"
    }
  ],
  "validation": {
    "status": "valid",
    "lastUpdated": "2025-10-10T14:30:00Z",
    "nextReview": "2026-04-10"
  }
}</code></pre>
                    </div>
                </div>
            </div>
        </section>

        <section id="ees-examples">
            <h2>Emergency Evacuation Statement (EES) Examples</h2>
            
            <p>The EES is the resident-facing summary that explains evacuation procedures in clear, accessible language.</p>

            <div class="example-card">
                <div class="example-header">
                    <h3>Example: Sarah Johnson's EES</h3>
                    <div class="example-controls">
                        <button class="toggle-btn active" data-target="sarah-ees-human">Human View</button>
                        <button class="toggle-btn" data-target="sarah-ees-technical">Technical View</button>
                    </div>
                </div>

                <div id="sarah-ees-human" class="example-content active">
                    <div class="document-preview">
                        <div class="document-header">
                            <h4>Your Emergency Evacuation Plan</h4>
                            <div class="document-status valid">✓ Current & Valid</div>
                        </div>
                        
                        <div class="document-section">
                            <h5>In Case of Fire Alarm</h5>
                            <p><strong>What will happen:</strong></p>
                            <ul>
                                <li>Maria or James will come to help you immediately</li>
                                <li>We will use the evacuation chair if needed</li>
                                <li>We will take you to the garden area safely</li>
                                <li>Your brother David will be contacted</li>
                            </ul>
                            
                            <p><strong>What you should do:</strong></p>
                            <ul>
                                <li>Stay calm and wait for help</li>
                                <li>Don't try to leave on your own</li>
                                <li>Your carers know exactly what to do</li>
                            </ul>
                        </div>

                        <div class="document-section">
                            <h5>Your Support Team</h5>
                            <div class="document-field">
                                <label>Main Carer:</label>
                                <span>Maria Rodriguez</span>
                            </div>
                            <div class="document-field">
                                <label>Backup Support:</label>
                                <span>James Wilson</span>
                            </div>
                            <div class="document-field">
                                <label>Emergency Contact:</label>
                                <span>David Johnson (brother) - 07123 456789</span>
                            </div>
                        </div>

                        <div class="document-section">
                            <h5>Assembly Point</h5>
                            <p>In an emergency, you will be taken to the <strong>garden area</strong> where there is a special space for wheelchair users. This is a safe distance from the building.</p>
                        </div>
                    </div>
                </div>

                <div id="sarah-ees-technical" class="example-content">
                    <div class="code-preview">
                        <pre><code>{
  "$schema": "https://open-peep.org/schemas/ees/v1.0.0",
  "id": "ees-sarah-johnson-2025-10-10",
  "version": "1.0.0",
  "peepId": "peep-sarah-johnson-2025-10-10",
  "resident": {
    "name": "Sarah Johnson",
    "room": "Room 42"
  },
  "content": {
    "title": "Your Emergency Evacuation Plan",
    "sections": [
      {
        "title": "In Case of Fire Alarm",
        "whatWillHappen": [
          "Maria or James will come to help you immediately",
          "We will use the evacuation chair if needed",
          "We will take you to the garden area safely",
          "Your brother David will be contacted"
        ],
        "whatYouShouldDo": [
          "Stay calm and wait for help",
          "Don't try to leave on your own",
          "Your carers know exactly what to do"
        ]
      },
      {
        "title": "Your Support Team",
        "team": [
          {
            "name": "Maria Rodriguez",
            "role": "Main Carer"
          },
          {
            "name": "James Wilson",
            "role": "Backup Support"
          }
        ],
        "emergencyContact": {
          "name": "David Johnson",
          "relationship": "brother",
          "phone": "07123456789"
        }
      },
      {
        "title": "Assembly Point",
        "description": "In an emergency, you will be taken to the garden area where there is a special space for wheelchair users. This is a safe distance from the building."
      }
    ]
  },
  "accessibility": {
    "language": "en-GB",
    "readingLevel": "simple",
    "format": "accessible_text"
  },
  "validation": {
    "status": "valid",
    "lastUpdated": "2025-10-10T14:30:00Z"
  }
}</code></pre>
                    </div>
                </div>
            </div>
        </section>

        <section id="consent-examples">
            <h2>Consent Management Examples</h2>
            
            <p>Consent management ensures GDPR compliance and protects both individuals and organisations.</p>

            <div class="example-card">
                <div class="example-header">
                    <h3>Example: Consent Record</h3>
                    <div class="example-controls">
                        <button class="toggle-btn active" data-target="consent-human">Human View</button>
                        <button class="toggle-btn" data-target="consent-technical">Technical View</button>
                    </div>
                </div>

                <div id="consent-human" class="example-content active">
                    <div class="document-preview">
                        <div class="document-header">
                            <h4>Consent for Evacuation Planning</h4>
                            <div class="document-status valid">✓ Consent Given</div>
                        </div>
                        
                        <div class="document-section">
                            <h5>Consent Details</h5>
                            <div class="document-field">
                                <label>Resident:</label>
                                <span>Sarah Johnson</span>
                            </div>
                            <div class="document-field">
                                <label>Consent Given:</label>
                                <span>Yes - 10 October 2025</span>
                            </div>
                            <div class="document-field">
                                <label>Purpose:</label>
                                <span>Emergency evacuation planning and safety</span>
                            </div>
                            <div class="document-field">
                                <label>Data Sharing:</label>
                                <span>Fire & Rescue Services, Emergency Responders</span>
                            </div>
                        </div>

                        <div class="document-section">
                            <h5>Data Protection</h5>
                            <div class="document-field">
                                <label>Retention Period:</label>
                                <span>Until resident leaves + 7 years</span>
                            </div>
                            <div class="document-field">
                                <label>Your Rights:</label>
                                <span>Access, correction, deletion, portability</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="consent-technical" class="example-content">
                    <div class="code-preview">
                        <pre><code>{
  "$schema": "https://open-peep.org/schemas/consent/v1.0.0",
  "id": "consent-sarah-johnson-2025-10-10",
  "version": "1.0.0",
  "resident": {
    "name": "Sarah Johnson",
    "room": "Room 42"
  },
  "consent": {
    "status": "given",
    "dateGiven": "2025-10-10T14:30:00Z",
    "givenBy": "resident",
    "purpose": "emergency_evacuation_planning",
    "dataSharing": [
      "fire_rescue_services",
      "emergency_responders",
      "care_providers"
    ],
    "retentionPeriod": "resident_departure_plus_7_years",
    "rightsExplained": true
  },
  "legalBasis": {
    "primary": "legitimate_interest",
    "secondary": "vital_interests",
    "description": "Protection of life and safety in emergency situations"
  },
  "validation": {
    "status": "valid",
    "lastUpdated": "2025-10-10T14:30:00Z"
  }
}</code></pre>
                    </div>
                </div>
            </div>
        </section>

        <section id="review-examples">
            <h2>Review and Maintenance Examples</h2>
            
            <p>Regular review ensures plans remain current, accurate, and compliant.</p>

            <div class="example-card">
                <div class="example-header">
                    <h3>Example: Review Record</h3>
                    <div class="example-controls">
                        <button class="toggle-btn active" data-target="review-human">Human View</button>
                        <button class="toggle-btn" data-target="review-technical">Technical View</button>
                    </div>
                </div>

                <div id="review-human" class="example-content active">
                    <div class="document-preview">
                        <div class="document-header">
                            <h4>Evacuation Plan Review</h4>
                            <div class="document-status valid">✓ Review Complete</div>
                        </div>
                        
                        <div class="document-section">
                            <h5>Review Details</h5>
                            <div class="document-field">
                                <label>Resident:</label>
                                <span>Sarah Johnson</span>
                            </div>
                            <div class="document-field">
                                <label>Review Date:</label>
                                <span>10 October 2025</span>
                            </div>
                            <div class="document-field">
                                <label>Reviewer:</label>
                                <span>Jennifer Smith, Fire Safety Officer</span>
                            </div>
                            <div class="document-field">
                                <label>Changes Made:</label>
                                <span>Updated evacuation buddy (Maria Rodriguez)</span>
                            </div>
                        </div>

                        <div class="document-section">
                            <h5>Compliance Status</h5>
                            <div class="document-field">
                                <label>Regulations:</label>
                                <span>All current requirements met</span>
                            </div>
                            <div class="document-field">
                                <label>Next Review:</label>
                                <span>10 April 2026</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="review-technical" class="example-content">
                    <div class="code-preview">
                        <pre><code>{
  "$schema": "https://open-peep.org/schemas/review/v1.0.0",
  "id": "review-sarah-johnson-2025-10-10",
  "version": "1.0.0",
  "resident": {
    "name": "Sarah Johnson"
  },
  "review": {
    "date": "2025-10-10T14:30:00Z",
    "reviewer": {
      "name": "Jennifer Smith",
      "role": "Fire Safety Officer"
    },
    "type": "scheduled_review",
    "changes": [
      {
        "type": "personnel_update",
        "description": "Updated evacuation buddy (Maria Rodriguez)",
        "reason": "staff_change"
      }
    ],
    "complianceStatus": "compliant",
    "nextReviewDue": "2026-04-10T00:00:00Z"
  },
  "validation": {
    "status": "valid",
    "lastUpdated": "2025-10-10T14:30:00Z"
  }
}</code></pre>
                    </div>
                </div>
            </div>
        </section>
        </div>
    </div>
</div>

<style>
/* Examples page specific styling */
.examples-container {
    display: grid;
    grid-template-columns: 280px 1fr;
    gap: 2rem;
    margin-top: 2rem;
}

.content-padding {
    padding: 2rem;
}

.examples-nav {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

.nav-item {
    padding: 0.75rem 1rem;
    background: #f6f8fa;
    border: 1px solid #d1d9e0;
    border-radius: 6px;
    text-decoration: none;
    color: #656d76;
    font-weight: 500;
    transition: all 0.2s ease;
}

.nav-item:hover, .nav-item.active {
    background: #0969da;
    color: white;
    text-decoration: none;
}

.example-card {
    background: white;
    border: 1px solid #d1d9e0;
    border-radius: 8px;
    margin-bottom: 2rem;
    overflow: hidden;
}

.example-header {
    background: #f6f8fa;
    padding: 1rem 1.5rem;
    border-bottom: 1px solid #d1d9e0;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.example-header h3 {
    margin: 0;
    font-size: 1.25rem;
}

.example-controls {
    display: flex;
    gap: 0.5rem;
}

.toggle-btn {
    padding: 0.5rem 1rem;
    background: white;
    border: 1px solid #d1d9e0;
    border-radius: 4px;
    cursor: pointer;
    font-size: 0.875rem;
    transition: all 0.2s ease;
}

.toggle-btn:hover {
    background: #f6f8fa;
}

.toggle-btn.active {
    background: #0969da;
    color: white;
    border-color: #0969da;
}

.example-content {
    display: none;
    padding: 1.5rem;
    width: 100%;
    overflow-x: auto;
}

.example-content.active {
    display: block;
}

.document-preview {
    background: white;
    border: none;
    border-radius: 0;
    padding: 2rem;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

.document-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
    padding-bottom: 1rem;
    border-bottom: 2px solid #0969da;
}

.document-header h4 {
    margin: 0;
    color: #0969da;
    font-size: 1.5rem;
}

.document-status {
    padding: 0.25rem 0.75rem;
    border-radius: 12px;
    font-size: 0.75rem;
    font-weight: 600;
    text-transform: uppercase;
}

.document-status.valid {
    background: #d1f7c4;
    color: #0f5132;
}

.document-section {
    margin-bottom: 1.5rem;
}

.document-section h5 {
    color: #24292f;
    font-size: 1.125rem;
    margin-bottom: 0.75rem;
    padding-bottom: 0.25rem;
    border-bottom: 1px solid #e1e7ef;
}

.document-field {
    display: flex;
    margin-bottom: 0.5rem;
    gap: 1rem;
}

.document-field label {
    font-weight: 600;
    color: #656d76;
    min-width: 140px;
    flex-shrink: 0;
}

.document-field span {
    color: #24292f;
}

.document-footer {
    margin-top: 1.5rem;
    padding-top: 1rem;
    border-top: 1px solid #e1e7ef;
    background: #f6f8fa;
    padding: 1rem;
    border-radius: 4px;
}

.toggle-instructions {
    background: #fff8e1;
    border: 1px solid #ffc107;
    border-radius: 6px;
    padding: 1rem;
    margin: 1.5rem 0;
}

@media (max-width: 768px) {
    .examples-container {
        grid-template-columns: 1fr;
        gap: 1rem;
    }
    
    .examples-nav {
        flex-direction: row;
        overflow-x: auto;
        padding-bottom: 0.5rem;
    }
    
    .nav-item {
        white-space: nowrap;
        flex-shrink: 0;
    }
    
    .example-header {
        flex-direction: column;
        align-items: flex-start;
        gap: 1rem;
    }
    
    .document-field {
        flex-direction: column;
        gap: 0.25rem;
    }
    
    .document-field label {
        min-width: auto;
    }
    
    .document-preview {
        padding: 1rem;
    }
}
</style>

<script>
// Toggle functionality and scroll-based navigation
document.addEventListener('DOMContentLoaded', function() {
    // Handle toggle buttons
    document.querySelectorAll('.toggle-btn').forEach(button => {
        button.addEventListener('click', function() {
            const targetId = this.getAttribute('data-target');
            const card = this.closest('.example-card');
            
            // Remove active class from all content in this card
            card.querySelectorAll('.example-content').forEach(content => {
                content.classList.remove('active');
            });
            
            // Remove active class from all buttons in this card
            card.querySelectorAll('.toggle-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Add active class to clicked button and target content
            this.classList.add('active');
            document.getElementById(targetId).classList.add('active');
        });
    });
    
    // Handle navigation clicks
    document.querySelectorAll('.nav-item').forEach(item => {
        item.addEventListener('click', function(e) {
            e.preventDefault();
            
            // Remove active class from all nav items
            document.querySelectorAll('.nav-item').forEach(nav => {
                nav.classList.remove('active');
            });
            
            // Add active class to clicked item
            this.classList.add('active');
            
            // Scroll to target section
            const targetId = this.getAttribute('href').substring(1);
            const targetSection = document.getElementById(targetId);
            if (targetSection) {
                targetSection.scrollIntoView({ behavior: 'smooth' });
            }
        });
    });
    
    // Scroll-based navigation highlighting
    function updateActiveNavItem() {
        const sections = document.querySelectorAll('section[id]');
        const navItems = document.querySelectorAll('.nav-item');
        
        // Get current scroll position
        const scrollPosition = window.scrollY + 100; // Offset for better UX
        
        // Find the section currently in view
        let currentSection = '';
        
        sections.forEach(section => {
            const sectionTop = section.offsetTop;
            const sectionHeight = section.offsetHeight;
            
            if (scrollPosition >= sectionTop && scrollPosition < sectionTop + sectionHeight) {
                currentSection = section.getAttribute('id');
            }
        });
        
        // Update navigation highlighting
        navItems.forEach(item => {
            const href = item.getAttribute('href');
            const sectionId = href ? href.substring(1) : '';
            
            if (sectionId === currentSection) {
                item.classList.add('active');
            } else {
                item.classList.remove('active');
            }
        });
        
        // Special handling for the top of the page
        if (scrollPosition < 200) {
            document.querySelector('.nav-item[href="#introduction"]')?.classList.add('active');
            navItems.forEach(item => {
                if (item.getAttribute('href') !== '#introduction') {
                    item.classList.remove('active');
                }
            });
        }
    }
    
    // Throttle scroll events for better performance
    let scrollTimeout;
    window.addEventListener('scroll', function() {
        if (scrollTimeout) {
            clearTimeout(scrollTimeout);
        }
        scrollTimeout = setTimeout(updateActiveNavItem, 10);
    });
    
    // Initial call to set the correct active item
    updateActiveNavItem();
});
</script>

---

## Get Involved

- [**Get Started**](/get-started/) — Learn how to implement OpenPEEP
- [**Read Documentation**](/documentation/) — Complete technical reference  
- [**Join the Community**](/support/) — Collaborate, contribute, and stay informed
- [**GitHub Repository**](https://github.com/OpenPEEP-Foundation/OpenPEEP) — Access schemas, examples, and code

---

## Ready for Implementation?

**Now that you've seen what OpenPEEP produces, it's time to plan your implementation.**

The examples show the end result, but successful implementation requires strategic planning, compliance understanding, and technical guidance.

<div class="next-steps">
    <a href="/documentation/" class="btn btn-primary">Implementation Guide →</a>
    <a href="/get-started/" class="btn btn-secondary">Get Started Planning →</a>
</div>



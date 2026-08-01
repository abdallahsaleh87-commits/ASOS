# Market Intelligence

## 1. Object Purpose

### Business Purpose

The Market Intelligence object represents a verified, time-bound observation or analytical insight about the automotive market that may affect dealership sales, pricing, inventory, demand, competitive positioning, or Customer engagement.

It enables the dealership to monitor and understand:

- Vehicle demand and supply conditions.
- Competitor pricing and promotional activity.
- Market price movements.
- Inventory availability and listing volume.
- Manufacturer incentives and rebate programs.
- Customer-search and inquiry trends.
- Regional vehicle preferences.
- Finance-rate and affordability trends.
- Used-vehicle valuation movements.
- Model launches, discontinuations, and specification changes.
- Seasonal and economic factors affecting vehicle sales.
- Competitor inventory and market positioning.
- Emerging opportunities and commercial risks.

Market Intelligence provides decision support. It does not automatically authorize:

- Vehicle-price changes.
- Customer discounts.
- Inventory acquisition.
- Marketing expenditure.
- Finance-rate commitments.
- Trade-In valuation approval.
- Deal or Quotation modifications.

Any binding commercial action derived from Market Intelligence must pass the applicable pricing, approval, finance, inventory, and compliance workflows.

### System Purpose

The Market Intelligence object is the canonical, evidence-backed market-observation and insight aggregate within the ASOS domain.

It connects:

- Dealership
- Vehicle
- Vehicle Model
- Inventory
- Competitor
- Geographic Market
- Market Segment
- Pricing Rule
- Incentive Program
- Campaign
- Lead
- Opportunity
- Quotation
- Deal
- Trade-In
- External Data Source
- AI Agent
- Human Analyst

The object preserves:

- The original observation.
- Its source and provenance.
- Supporting evidence.
- Geographic and commercial scope.
- Effective period.
- Confidence and data-quality assessments.
- Derived analysis.
- Recommended commercial actions.
- Human-review and publication decisions.
- Version and supersession history.

It provides governed intelligence to:

- Pricing and discount workflows.
- Inventory planning.
- Vehicle sourcing.
- Sales Playbook selection.
- Customer and Vehicle matching.
- Campaign planning.
- Competitor monitoring.
- Forecasting.
- Trade-In valuation support.
- Executive dashboards.
- AI recommendation systems.

Every Market Intelligence record must remain traceable to one or more identifiable sources and must clearly distinguish:

- Observed facts.
- Derived calculations.
- Analyst interpretations.
- AI-generated inferences.
- Recommended actions.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `market_intelligence_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `source_id` (UUIDv4 — required)
  - `vehicle_id` (UUIDv4 — optional)
  - `vehicle_model_id` (UUIDv4 — optional)
  - `competitor_id` (UUIDv4 — optional)
  - `market_region_id` (UUIDv4 — optional)
  - `market_segment_id` (UUIDv4 — optional)
  - `incentive_program_id` (UUIDv4 — optional)
  - `campaign_id` (UUIDv4 — optional)
  - `created_by` (UUIDv4 — required)
  - `reviewed_by` (UUIDv4 — optional)
  - `approved_by` (UUIDv4 — optional)
  - `supersedes_intelligence_id` (UUIDv4 — optional)

### Intelligence Classification

- `intelligence_type`
- `status`
- `scope`
- `source_type`
- `priority`
- `visibility`
- `time_horizon`
- `impact_level`
- `confidence_band`
- `data_quality_status`

### Observation Fields

- `title`
- `observation_summary`
- `observation_details`
- `observed_at`
- `effective_from`
- `effective_until`
- `published_at`
- `expires_at`
- `observation_method`
- `observation_unit`
- `observation_value`
- `previous_observation_value`
- `change_amount`
- `change_percentage`

### Vehicle and Product Context

- `vehicle_id`
- `vehicle_model_id`
- `make`
- `model`
- `trim`
- `model_year`
- `vehicle_condition`
- `body_type`
- `fuel_type`
- `transmission`
- `product_category`
- `market_segment_id`

### Pricing Intelligence

- `currency_code`
- `market_price_min_amount`
- `market_price_average_amount`
- `market_price_median_amount`
- `market_price_max_amount`
- `competitor_price_amount`
- `dealership_price_amount`
- `price_gap_amount`
- `price_gap_percentage`
- `discount_average_amount`
- `incentive_amount`
- `estimated_transaction_price_amount`
- `price_index`
- `price_trend`

### Supply and Inventory Intelligence

- `market_listing_count`
- `competitor_listing_count`
- `dealership_inventory_count`
- `estimated_market_supply_days`
- `average_days_on_market`
- `inventory_turn_rate`
- `availability_status`
- `supply_trend`
- `scarcity_score`
- `stock_pressure_score`

### Demand Intelligence

- `search_volume`
- `inquiry_count`
- `lead_count`
- `qualified_lead_count`
- `opportunity_count`
- `quotation_count`
- `deal_count`
- `conversion_rate`
- `demand_index`
- `demand_trend`
- `customer_interest_score`
- `regional_preference_score`

### Competitor Intelligence

- `competitor_id`
- `competitor_name`
- `competitor_location`
- `competitor_offer_summary`
- `competitor_discount_amount`
- `competitor_incentive_summary`
- `competitor_inventory_summary`
- `competitor_finance_summary`
- `competitor_advantage`
- `competitor_weakness`
- `competitive_threat_score`

### Geographic Context

- `country_code`
- `region`
- `city`
- `postal_code`
- `latitude`
- `longitude`
- `radius_km`
- `market_region_id`
- `geographic_scope_description`

### Source and Evidence

- `source_id`
- `source_type`
- `source_name`
- `source_reference`
- `source_url`
- `source_published_at`
- `source_retrieved_at`
- `source_reliability_score`
- `evidence_count`
- `evidence_references`
- `evidence_hash`
- `raw_data_reference`
- `collection_method`
- `collection_job_id`

### Analysis Fields

- `trend_direction`
- `trend_strength`
- `commercial_impact`
- `inventory_impact`
- `pricing_impact`
- `sales_impact`
- `customer_impact`
- `risk_summary`
- `opportunity_summary`
- `analyst_interpretation`
- `ai_interpretation`
- `recommended_action`
- `recommended_action_type`
- `action_urgency`
- `action_owner_role`
- `review_required`

### AI and Model Fields

- `ai_generated`
- `ai_processing_status`
- `ai_model_reference`
- `ai_prompt_version`
- `ai_confidence_score`
- `feature_snapshot`
- `inference_explanation`
- `alternative_hypotheses`
- `human_review_required`
- `human_review_reason`

### Review and Publication

- `review_status`
- `review_outcome`
- `review_notes`
- `reviewed_by`
- `reviewed_at`
- `approved_by`
- `approved_at`
- `publication_channel`
- `published_at`
- `rejection_reason`

### Computed Fields

- `age_hours`
- `age_days`
- `days_until_expiry`
- `is_current`
- `is_stale`
- `freshness_score`
- `composite_confidence_score`
- `market_opportunity_score`
- `market_risk_score`
- `recommended_review_at`
- `supporting_source_count`
- `contradicting_source_count`
- `evidence_consistency_score`

### Governance and Lifecycle

- **Observation Snapshot:** `observation_snapshot` (JSONB)
- **Evidence Snapshot:** `evidence_snapshot` (JSONB)
- **Source Snapshot:** `source_snapshot` (JSONB)
- **Analysis Snapshot:** `analysis_snapshot` (JSONB)
- **AI Analysis Snapshot:** `ai_analysis_snapshot` (JSONB)
- **Review Snapshot:** `review_snapshot` (JSONB)
- **Publication Snapshot:** `publication_snapshot` (JSONB)

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `reviewed_by`
  - `approved_by`
  - `published_by`
  - `archived_by`
  - `last_processed_by_agent`

- **Versioning:**
  - `intelligence_version`
  - `supersedes_intelligence_id`
  - `is_current_version`
  - `record_version`

- **Soft Delete:**
  - `is_deleted`
  - `deleted_at`

- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `observed_at`
  - `source_published_at`
  - `source_retrieved_at`
  - `effective_from`
  - `effective_until`
  - `reviewed_at`
  - `approved_at`
  - `published_at`
  - `expires_at`
  - `superseded_at`
  - `archived_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| market_intelligence_id | UUID | Unique canonical identifier for the Market Intelligence record. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns or subscribes to the intelligence. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| source_id | UUID | Canonical source from which the observation was obtained. | Yes | N/A | Must reference an active approved source | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| vehicle_id | UUID | Specific dealership Vehicle affected by the intelligence. | No | Null | Must belong to the same dealership when populated | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| vehicle_model_id | UUID | Vehicle model or product family affected by the observation. | No | Null | Must reference the approved vehicle catalogue | 666e7777-e88b-99d0-a111-426614174000 | System-controlled |
| competitor_id | UUID | Competitor associated with the intelligence. | No | Null | Must reference a known or approved competitor record | 888e9999-e00b-11d2-a222-426614174000 | System-controlled |
| intelligence_type | Enum | Primary category of the observation or analysis. | Yes | MARKET_TREND | Must match MarketIntelligenceType ENUM | COMPETITOR_PRICING | At least 0.95 |
| status | Enum | Current lifecycle state of the intelligence. | Yes | DRAFT | Must match MarketIntelligenceStatus ENUM | VALIDATED | At least 0.99 |
| scope | Enum | Commercial and organizational scope of the intelligence. | Yes | DEALERSHIP | Must match MarketIntelligenceScope ENUM | REGIONAL | At least 0.95 |
| source_type | Enum | Classification of the evidence source. | Yes | INTERNAL_DATA | Must match MarketIntelligenceSourceType ENUM | MARKETPLACE | At least 0.99 |
| priority | Enum | Operational importance of the intelligence. | Yes | STANDARD | Must match MarketIntelligencePriority ENUM | HIGH | At least 0.90 |
| visibility | Enum | Access classification for the record. | Yes | INTERNAL | Must match MarketIntelligenceVisibility ENUM | MANAGEMENT_ONLY | System-controlled |
| title | String | Short descriptive title. | Yes | N/A | Between 5 and 250 characters | Compact SUV prices increased in Cairo | Human or AI |
| observation_summary | Text | Concise factual summary of what was observed. | Yes | N/A | Must distinguish facts from interpretation | Average listed prices increased by 4.8% over 30 days. | Human or AI |
| observation_details | Text | Detailed explanation of the observation and evidence. | No | Null | Must not contain unsupported claims | Based on 426 active listings from three approved sources. | Human or AI |
| observed_at | Timestamp | Time at which the market condition was observed. | Yes | N/A | Cannot be materially later than ingestion time | 2026-08-01T10:00:00Z | Trusted source |
| effective_from | Timestamp | Start of the period for which the intelligence is considered applicable. | Yes | observed_at | Must not be later than effective_until | 2026-08-01T00:00:00Z | System or analyst |
| effective_until | Timestamp | End of the expected applicability period. | No | Null | Must be later than effective_from | 2026-08-15T23:59:59Z | System or analyst |
| expires_at | Timestamp | Time after which the intelligence must be refreshed or marked stale. | Yes | Policy-defined | Must be later than observed_at | 2026-08-08T10:00:00Z | System-calculated |
| source_name | String | Human-readable name of the source. | Yes | From source record | Maximum 250 characters | Approved Marketplace Feed | Authoritative source |
| source_reference | String | External or internal source identifier. | Yes | N/A | Must be traceable and unique within the source context | feed-batch-20260801-04 | Trusted source |
| source_url | String | Authorized reference URL to the original evidence. | No | Null | Must use an approved protocol and source domain | https://example.com/market-report | Trusted source |
| source_reliability_score | Decimal | Reliability score assigned to the source. | Yes | 0.00 | Must remain between 0.00 and 1.00 | 0.92 | System or analyst |
| evidence_count | Integer | Number of supporting evidence items. | Yes | 0 | Must be zero or greater | 426 | System-computed |
| evidence_hash | String | Cryptographic hash of the normalized evidence package. | Conditional | Generated | Required before validation | sha256:4a71... | System-generated |
| currency_code | String | ISO 4217 currency code used for financial values. | Conditional | Dealership default | Exactly three uppercase characters | EGP | At least 0.99 |
| market_price_average_amount | Decimal | Average observed market price. | No | Null | Must be zero or greater | 2150000.00 | System-computed |
| market_price_median_amount | Decimal | Median observed market price. | No | Null | Must be zero or greater | 2110000.00 | System-computed |
| competitor_price_amount | Decimal | Observed competitor price for the scoped product. | No | Null | Must preserve source and observation time | 2080000.00 | Trusted evidence |
| dealership_price_amount | Decimal | Current dealership price used for comparison. | No | Null | Must come from an authorized internal pricing source | 2140000.00 | Pricing system |
| price_gap_amount | Decimal | Difference between dealership and benchmark price. | No | Null | Must be system-computed | 60000.00 | System-computed |
| price_gap_percentage | Decimal | Price difference expressed as a percentage. | No | Null | Must be system-computed | 2.88 | System-computed |
| market_listing_count | Integer | Number of observed relevant market listings. | No | Null | Must be zero or greater | 426 | Trusted source |
| average_days_on_market | Decimal | Average number of days relevant vehicles remain listed. | No | Null | Must be zero or greater | 23.4 | System-computed |
| demand_index | Decimal | Normalized indicator of relative market demand. | No | Null | Must remain within the configured scale | 78.5 | System-computed |
| scarcity_score | Decimal | Normalized measure of relative supply scarcity. | No | Null | Must remain between 0.00 and 1.00 | 0.74 | System-computed |
| competitive_threat_score | Decimal | Estimated competitive threat level. | No | Null | Must remain between 0.00 and 1.00 | 0.68 | System-computed |
| trend_direction | Enum | Direction of the observed movement. | Yes | STABLE | Must match MarketTrendDirection ENUM | INCREASING | At least 0.90 |
| impact_level | Enum | Estimated significance of the intelligence. | Yes | MODERATE | Must match MarketImpactLevel ENUM | HIGH | Human or AI |
| confidence_band | Enum | Human-readable confidence classification. | Yes | MEDIUM | Must match MarketConfidenceBand ENUM | HIGH | System-computed |
| ai_generated | Boolean | Indicates whether AI generated the analysis or observation. | Yes | false | Source facts must remain distinguishable from AI inference | true | System-controlled |
| ai_confidence_score | Decimal | Confidence assigned to the AI analysis. | No | Null | Must remain between 0.00 and 1.00 | 0.87 | System-computed |
| recommended_action | Text | Non-binding commercial or operational recommendation. | No | Null | Must identify assumptions and approval requirements | Review pricing and inventory allocation within 48 hours. | AI or analyst |
| review_status | Enum | Current human-review state. | Yes | NOT_REQUIRED | Must match MarketIntelligenceReviewStatus ENUM | APPROVED | System-controlled |
| reviewed_by | UUID | User who reviewed the intelligence. | No | Null | Required when review is completed | 321e6547-e89b-12d3-a456-426614174000 | System-controlled |
| intelligence_version | Integer | Sequential version number for the intelligence series. | Yes | 1 | Must be one or greater and increase sequentially | 2 | System-controlled |
| supersedes_intelligence_id | UUID | Previous intelligence record replaced by this version. | No | Null | Must reference an earlier record in the same scope and series | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| is_current_version | Boolean | Indicates whether this is the active version. | Yes | true | Only one current version may exist in the same series | true | System-controlled |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increase after every successful update | 5 | System-controlled |

## 4. Enumerations

### MarketIntelligenceStatus

- **DRAFT:** The record is being prepared and may still be edited.
- **COLLECTED:** Source data and evidence were ingested.
- **VALIDATION_PENDING:** Evidence quality, provenance, and consistency are being reviewed.
- **VALIDATED:** The underlying observation passed required validation.
- **ANALYZED:** Interpretation, impact, and recommended actions were added.
- **PUBLISHED:** The intelligence is approved and available to authorized consumers.
- **STALE:** The record is no longer sufficiently current for unrestricted decision support.
- **REJECTED:** Validation or review determined that the intelligence is unreliable or unsuitable.
- **SUPERSEDED:** A newer intelligence version replaced this record.
- **ARCHIVED:** The record was moved to historical retention.

### MarketIntelligenceType

- MARKET_TREND
- DEMAND_TREND
- SUPPLY_TREND
- PRICING_TREND
- COMPETITOR_PRICING
- COMPETITOR_PROMOTION
- COMPETITOR_INVENTORY
- CUSTOMER_PREFERENCE
- SEARCH_TREND
- LEAD_TREND
- CONVERSION_TREND
- VEHICLE_AVAILABILITY
- INVENTORY_PRESSURE
- INCENTIVE_PROGRAM
- FINANCE_RATE_TREND
- TRADE_IN_VALUE_TREND
- VEHICLE_LAUNCH
- VEHICLE_DISCONTINUATION
- REGULATORY_CHANGE
- ECONOMIC_INDICATOR
- SEASONAL_PATTERN
- MARKET_RISK
- MARKET_OPPORTUNITY
- OTHER

### MarketIntelligenceSourceType

- INTERNAL_DATA
- DEALERSHIP_NETWORK
- OEM
- GOVERNMENT
- REGULATOR
- MARKETPLACE
- COMPETITOR_WEBSITE
- PRICING_PROVIDER
- VALUATION_PROVIDER
- CREDIT_PROVIDER
- INDUSTRY_REPORT
- NEWS_SOURCE
- SOCIAL_MEDIA
- CUSTOMER_RESEARCH
- SURVEY
- MANUAL_OBSERVATION
- API_INTEGRATION
- AI_GENERATED
- OTHER

### MarketIntelligenceScope

- VEHICLE
- VEHICLE_MODEL
- PRODUCT_CATEGORY
- MARKET_SEGMENT
- DEALERSHIP
- DEALER_GROUP
- CITY
- REGIONAL
- NATIONAL
- INTERNATIONAL

### MarketIntelligencePriority

- LOW
- STANDARD
- HIGH
- URGENT
- CRITICAL

### MarketIntelligenceVisibility

- INTERNAL
- SALES_TEAM
- PRICING_TEAM
- INVENTORY_TEAM
- MANAGEMENT_ONLY
- RESTRICTED
- AI_AUTHORIZED
- PARTNER_SHARED

### MarketTimeHorizon

- IMMEDIATE
- SHORT_TERM
- MEDIUM_TERM
- LONG_TERM
- HISTORICAL

### MarketImpactLevel

- NONE
- LOW
- MODERATE
- HIGH
- CRITICAL

### MarketConfidenceBand

- VERY_LOW
- LOW
- MEDIUM
- HIGH
- VERY_HIGH

### MarketTrendDirection

- STRONGLY_DECREASING
- DECREASING
- STABLE
- INCREASING
- STRONGLY_INCREASING
- VOLATILE
- UNKNOWN

### MarketDataQualityStatus

- NOT_ASSESSED
- INCOMPLETE
- LOW_QUALITY
- ACCEPTABLE
- HIGH_QUALITY
- CONFLICTING
- INVALID
- EXPIRED

### MarketIntelligenceReviewStatus

- NOT_REQUIRED
- PENDING
- IN_REVIEW
- APPROVED
- APPROVED_WITH_LIMITATIONS
- REJECTED
- REVISION_REQUIRED

### MarketIntelligenceReviewOutcome

- VALIDATED
- PARTIALLY_VALIDATED
- INSUFFICIENT_EVIDENCE
- CONFLICTING_EVIDENCE
- OUTDATED
- DUPLICATE
- UNSUPPORTED_INFERENCE
- POLICY_RESTRICTED
- OTHER

### RecommendedActionType

- NO_ACTION
- MONITOR
- REVIEW_PRICING
- REVIEW_DISCOUNT_POLICY
- INCREASE_INVENTORY
- REDUCE_INVENTORY
- SOURCE_VEHICLES
- REALLOCATE_INVENTORY
- LAUNCH_CAMPAIGN
- CHANGE_CAMPAIGN
- UPDATE_SALES_PLAYBOOK
- CONTACT_ACTIVE_CUSTOMERS
- REVIEW_TRADE_IN_VALUES
- REVIEW_FINANCE_PROGRAMS
- ESCALATE_TO_MANAGEMENT
- HUMAN_ANALYSIS_REQUIRED
- OTHER

## 5. Validation Rules

### Business Rules

- Every Market Intelligence record must identify at least one traceable source.
- Every published record must contain supporting evidence.
- Observed facts must remain separate from analyst or AI interpretation.
- AI-generated conclusions must identify the model, input scope, confidence, and assumptions.
- A single unsupported observation must not be presented as a confirmed market-wide trend.
- Market-wide conclusions must meet the configured minimum sample-size and source-diversity requirements.
- Pricing comparisons must use equivalent Vehicle specifications, condition, geography, time period, and currency.
- Used and new Vehicle data must not be combined without explicit normalization.
- Customer inquiry volume must not be treated as completed demand without qualification or conversion context.
- Competitor advertised prices must not be treated as final transaction prices unless authoritative evidence exists.
- Expired incentives must not be treated as active.
- Stale Market Intelligence must not be used for automated recommendations unless explicitly revalidated.
- Restricted competitor, Customer, financial, or personal data must not be exposed to unauthorized Users.
- A recommendation does not authorize a binding commercial change.
- Vehicle pricing, discount, acquisition, and marketing-spend decisions require their applicable approval workflows.
- Records derived from unlawful, prohibited, or unauthorized data collection must be rejected.
- Duplicate observations should be merged, linked, or superseded according to provenance rules.
- Contradictory evidence must be preserved and disclosed rather than silently removed.

### Technical Rules

- Source ingestion must use idempotency controls.
- External source records must preserve:
  - Source identifier
  - Retrieval timestamp
  - Source publication timestamp
  - Collection method
  - Request or batch reference
  - Evidence hash
  - Provider response reference

- `record_version` must increase after every permitted update.
- Published observation, evidence, source, and analysis snapshots must become immutable.
- Material changes require a new intelligence version.
- Every computed metric must preserve its formula version.
- Currency conversions must preserve:
  - Original amount
  - Original currency
  - Conversion rate
  - Rate source
  - Conversion timestamp

- AI analysis must preserve:
  - Model reference
  - Prompt version
  - Feature snapshot
  - Confidence
  - Explanation
  - Alternative hypotheses

- Failed ingestion or analysis must not delete the original evidence.
- Scheduled freshness and expiry Jobs must use authoritative server time.
- Search and vector indexes must exclude rejected, deleted, restricted, and expired content when policy requires it.
- Every lifecycle transition must create immutable history and audit records.

### Data Constraints

- `observed_at` cannot be materially later than `source_retrieved_at`.
- `effective_until` must be later than `effective_from`.
- `expires_at` must be later than `observed_at`.
- `source_reliability_score` must remain between `0.00` and `1.00`.
- `ai_confidence_score` must remain between `0.00` and `1.00`.
- `valuation_confidence_score`, when used, must remain between `0.00` and `1.00`.
- `scarcity_score` must remain between `0.00` and `1.00`.
- `stock_pressure_score` must remain between `0.00` and `1.00`.
- `competitive_threat_score` must remain between `0.00` and `1.00`.
- `evidence_consistency_score` must remain between `0.00` and `1.00`.
- Counts cannot be negative.
- Prices, incentives, discounts, and transaction amounts cannot be negative.
- `market_price_min_amount` cannot exceed `market_price_max_amount`.
- `market_price_average_amount` should remain between the minimum and maximum observed values.
- `market_price_median_amount` should remain between the minimum and maximum observed values.
- `change_percentage` must use the configured comparison formula.
- Geographic coordinates must fall within valid latitude and longitude ranges.
- `radius_km` cannot be negative.
- `evidence_hash` is required before validation.
- `reviewed_by` and `reviewed_at` are required when review is completed.
- `rejection_reason` is required when status becomes `REJECTED`.
- `supersedes_intelligence_id` is required when status becomes `SUPERSEDED`.

### Referential Integrity

- Every linked entity must belong to the permitted tenant or shared-reference scope.
- `vehicle_id` must match `vehicle_model_id` when both are populated.
- `competitor_id` must belong to the applicable geographic or commercial market.
- `market_region_id` must match the stated geographic scope.
- `incentive_program_id` must reference the applicable Vehicle, market, and effective period.
- `campaign_id` must belong to the same dealership.
- `source_id` must reference an approved or explicitly reviewed source.
- `supersedes_intelligence_id` must reference an earlier record in the same intelligence series.
- Circular supersession relationships are prohibited.
- A published Market Intelligence record cannot be hard-deleted while referenced by pricing, inventory, Campaign, Quotation, Deal, Trade-In, or audit records.

### Human Approval Requirements

- Competitor pricing with low-quality or unverified evidence requires human review.
- High-impact pricing or inventory recommendations require authorized commercial review.
- Regulatory, legal, safety, fraud, or reputational intelligence requires specialist review.
- AI Agents cannot approve or publish their own intelligence output.
- AI Agents cannot directly change Vehicle prices, discount limits, inventory orders, Campaign budgets, or Trade-In values.
- AI Agents cannot classify a source as authoritative without an approved source-governance rule.
- Low-confidence, conflicting, or insufficiently supported conclusions must create a Human Review Task.
- Restricted intelligence sharing requires approval from an authorized management or compliance role.
- Manual source-reliability overrides require a documented reason.
- Rejection, redaction, or deletion of published intelligence requires authorized governance approval.

## 6. State Machine

### Allowed States

- DRAFT
- COLLECTED
- VALIDATION_PENDING
- VALIDATED
- ANALYZED
- PUBLISHED
- STALE
- REJECTED
- SUPERSEDED
- ARCHIVED

### Allowed Transitions

- DRAFT → COLLECTED
- DRAFT → REJECTED
- COLLECTED → VALIDATION_PENDING
- COLLECTED → REJECTED
- VALIDATION_PENDING → VALIDATED
- VALIDATION_PENDING → COLLECTED
- VALIDATION_PENDING → REJECTED
- VALIDATED → ANALYZED
- VALIDATED → PUBLISHED
- VALIDATED → REJECTED
- ANALYZED → PUBLISHED
- ANALYZED → VALIDATED
- ANALYZED → REJECTED
- PUBLISHED → STALE
- PUBLISHED → SUPERSEDED
- PUBLISHED → ARCHIVED
- STALE → VALIDATION_PENDING
- STALE → SUPERSEDED
- STALE → ARCHIVED
- REJECTED → ARCHIVED
- SUPERSEDED → ARCHIVED

### Forbidden Transitions

- DRAFT → PUBLISHED
- DRAFT → ANALYZED
- COLLECTED → PUBLISHED
- VALIDATION_PENDING → PUBLISHED
- REJECTED → VALIDATED
- REJECTED → ANALYZED
- REJECTED → PUBLISHED
- SUPERSEDED → PUBLISHED
- ARCHIVED → PUBLISHED
- ARCHIVED → VALIDATED
- STALE → PUBLISHED
- PUBLISHED → DRAFT

### Entry Conditions

- To enter `COLLECTED`:
  - At least one source must be identified.
  - Source reference and retrieval time must be recorded.
  - Minimum observation content must exist.
  - Raw evidence or a secure evidence reference must be preserved.

- To enter `VALIDATION_PENDING`:
  - Evidence normalization must be complete.
  - Duplicate checks must run.
  - Source reliability and data quality must be ready for assessment.
  - `evidence_hash` must be populated.

- To enter `VALIDATED`:
  - Source provenance must be verified.
  - Evidence must satisfy minimum quality and completeness requirements.
  - Conflicts and limitations must be documented.
  - Required human review must be complete.
  - Unsupported or unlawful data must be excluded.

- To enter `ANALYZED`:
  - The underlying observation must be validated.
  - Analysis must clearly distinguish facts, calculations, interpretations, and recommendations.
  - Formulas, model references, assumptions, and confidence must be recorded.
  - Material alternative explanations must be considered.

- To enter `PUBLISHED`:
  - The record must be validated.
  - Required analysis and review must be complete.
  - Visibility and permitted audiences must be defined.
  - Effective and expiry periods must be populated.
  - Publication snapshots must be frozen.
  - Restricted or unsupported claims must not remain.

- To enter `STALE`:
  - `expires_at` must have passed, or a material source, market, pricing, supply, demand, or regulatory change must reduce reliability.
  - The stale reason must be recorded.

- To enter `REJECTED`:
  - Evidence must be invalid, insufficient, prohibited, duplicated, materially conflicting, or unsupported.
  - A standardized rejection reason must be recorded.

- To enter `SUPERSEDED`:
  - A replacement Market Intelligence version must exist.
  - The replacement must reference the previous version.
  - `is_current_version` must become false.
  - `superseded_at` must be populated.

- To enter `ARCHIVED`:
  - The record must no longer be required for active decision support.
  - Required retention, dependency, audit, and legal-hold checks must pass.
  - `archived_at` must be populated.

### Exit Conditions

- A record cannot exit `DRAFT` without minimum source and observation information.
- A record cannot exit `COLLECTED` toward validation without preserved evidence.
- A record cannot exit `VALIDATION_PENDING` without a validation, revision, or rejection decision.
- A record cannot exit `VALIDATED` toward publication while required analysis or review remains incomplete.
- A record cannot exit `ANALYZED` toward publication without approved visibility and effective dates.
- A published record cannot be edited directly; material changes require a new version.
- A stale record cannot return directly to `PUBLISHED`; it must be revalidated or replaced.
- A rejected record cannot return to active use; a corrected record must be created separately.
- A superseded record remains historical and cannot become current again.
- An archived record cannot return to an active state.

### Terminal States

- **REJECTED:** The intelligence was determined to be invalid, unsuitable, or prohibited.
- **SUPERSEDED:** A newer version replaced the record.
- **ARCHIVED:** The intelligence moved to historical retention.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Approved Market Intelligence Source identified by `source_id`.
  - Vehicle catalogue, market taxonomy, and geographic-reference data.
  - Source-governance, data-quality, review, publication, and retention policies.
  - Applicable commercial, pricing, inventory, marketing, and compliance rules.

- **Consumes:**
  - Internal Vehicle inventory, pricing, Lead, Opportunity, Quotation, Deal, and Trade-In data.
  - OEM catalogues, incentive programs, product announcements, and allocation data.
  - Approved marketplace, valuation-provider, industry-report, and competitor data.
  - Customer inquiry, search, preference, and conversion trends.
  - Geographic, seasonal, regulatory, economic, and finance-market indicators.
  - Human Analyst observations.
  - Authorized AI-generated analysis.
  - Historical Market Intelligence versions and comparable observations.

- **Produces:**
  - Validated market observations.
  - Demand, supply, pricing, scarcity, and competitive indicators.
  - Vehicle-model and segment-level intelligence.
  - Market opportunities and risk assessments.
  - Non-binding commercial recommendations.
  - Management alerts.
  - Pricing-review and inventory-review context.
  - Sales Playbook and Campaign decision support.
  - Forecasting and executive-dashboard inputs.

- **Creates:**
  - Validation Tasks.
  - Human Review Tasks.
  - Market-monitoring alerts.
  - Pricing-review requests.
  - Inventory-review requests.
  - Campaign-review requests.
  - Sales Playbook update proposals.
  - Replacement Market Intelligence versions.
  - Supporting analytics snapshots.

- **Triggers:**
  - Evidence normalization and validation.
  - Duplicate and contradiction detection.
  - AI analysis and explanation.
  - Analyst review.
  - Intelligence publication.
  - Expiry and freshness monitoring.
  - Market-risk escalation.
  - Pricing and inventory review workflows.
  - Competitor-watch notifications.
  - Vehicle-demand and scarcity alerts.

- **Owned By:**
  - The Dealership or Dealer Group represented by `dealership_id`.
  - Operational ownership may be assigned to a Market Analyst, Pricing Manager, Inventory Manager, or Management User.
  - Source facts remain attributable to the original source and must not be represented as dealership-authored evidence.

- **Referenced By:**
  - Vehicle
  - Vehicle Model
  - Inventory
  - Pricing Rule
  - Incentive Program
  - Campaign
  - Sales Playbook
  - Lead
  - Qualified Lead
  - Opportunity
  - Quotation
  - Deal
  - Trade-In
  - Forecast
  - Competitor
  - Geographic Market
  - Executive Dashboard
  - AI Agent Run
  - Analytics Event
  - Human Review Task

- **Supports but Does Not Control:**
  - Vehicle pricing.
  - Discount limits.
  - Inventory acquisition.
  - Campaign budgets.
  - Trade-In approval.
  - Finance-product selection.
  - Customer-facing commercial commitments.

- **Supersedes / Replaced By:**
  - Material source, evidence, scope, methodology, or interpretation changes require a new intelligence version.
  - The new version references the previous record through `supersedes_intelligence_id`.
  - Superseded records remain immutable and available for audit, comparison, and model evaluation.

## 8. Domain Events

### Emitted Events

- **MarketIntelligenceDraftCreated**  
  Payload: `market_intelligence_id`, `intelligence_type`, `scope`, `source_id`, `created_at`

- **MarketIntelligenceCollected**  
  Payload: `market_intelligence_id`, `source_reference`, `source_retrieved_at`, `evidence_count`

- **MarketIntelligenceValidationRequested**  
  Payload: `market_intelligence_id`, `data_quality_status`, `review_required`, `requested_at`

- **MarketIntelligenceValidated**  
  Payload: `market_intelligence_id`, `source_reliability_score`, `evidence_consistency_score`, `validated_at`

- **MarketIntelligenceValidationFailed**  
  Payload: `market_intelligence_id`, `validation_errors`, `failed_at`

- **MarketIntelligenceAnalyzed**  
  Payload: `market_intelligence_id`, `trend_direction`, `impact_level`, `composite_confidence_score`, `analyzed_at`

- **MarketIntelligenceReviewRequested**  
  Payload: `market_intelligence_id`, `human_review_reason`, `priority`, `requested_at`

- **MarketIntelligenceReviewCompleted**  
  Payload: `market_intelligence_id`, `review_outcome`, `reviewed_by`, `reviewed_at`

- **MarketIntelligenceApproved**  
  Payload: `market_intelligence_id`, `approved_by`, `approved_at`, `visibility`

- **MarketIntelligencePublished**  
  Payload: `market_intelligence_id`, `publication_channel`, `effective_from`, `expires_at`, `published_at`

- **MarketIntelligenceRejected**  
  Payload: `market_intelligence_id`, `rejection_reason`, `reviewed_by`, `rejected_at`

- **MarketIntelligenceStaleDetected**  
  Payload: `market_intelligence_id`, `stale_reason`, `detected_at`, `recommended_review_at`

- **MarketIntelligenceSuperseded**  
  Payload: `market_intelligence_id`, `replacement_intelligence_id`, `superseded_at`

- **MarketIntelligenceArchived**  
  Payload: `market_intelligence_id`, `archived_by`, `archived_at`

- **MarketPriceMovementDetected**  
  Payload: `market_intelligence_id`, `vehicle_model_id`, `change_amount`, `change_percentage`, `trend_direction`

- **MarketDemandChangeDetected**  
  Payload: `market_intelligence_id`, `market_segment_id`, `demand_index`, `demand_trend`

- **MarketSupplyChangeDetected**  
  Payload: `market_intelligence_id`, `market_segment_id`, `estimated_market_supply_days`, `supply_trend`

- **CompetitorPriceChangeDetected**  
  Payload: `market_intelligence_id`, `competitor_id`, `vehicle_model_id`, `competitor_price_amount`, `observed_at`

- **CompetitorPromotionDetected**  
  Payload: `market_intelligence_id`, `competitor_id`, `competitor_offer_summary`, `effective_until`

- **MarketOpportunityDetected**  
  Payload: `market_intelligence_id`, `market_opportunity_score`, `opportunity_summary`, `recommended_action_type`

- **MarketRiskDetected**  
  Payload: `market_intelligence_id`, `market_risk_score`, `risk_summary`, `impact_level`

- **PricingReviewRecommended**  
  Payload: `market_intelligence_id`, `vehicle_id`, `vehicle_model_id`, `price_gap_percentage`, `action_urgency`

- **InventoryReviewRecommended**  
  Payload: `market_intelligence_id`, `vehicle_model_id`, `demand_index`, `scarcity_score`, `recommended_action_type`

- **MarketIntelligenceHumanReviewRequired**  
  Payload: `market_intelligence_id`, `human_review_reason`, `confidence_band`, `created_at`

### Consumed Events

- **VehicleCreated**  
  Enables Vehicle-specific Market Intelligence linking.

- **VehiclePriceUpdated**  
  Recalculates dealership-versus-market price gaps.

- **VehicleStatusChanged**  
  Updates inventory availability and supply calculations.

- **InventorySnapshotCreated**  
  Supplies dealership inventory count, age, and turn-rate context.

- **LeadCreated**  
  Updates inquiry and demand indicators.

- **QualifiedLeadCreated**  
  Updates qualified-demand indicators.

- **OpportunityCreated**  
  Updates active commercial-demand indicators.

- **QuotationIssued**  
  Updates quotation-volume and pricing-response indicators.

- **DealClosed**  
  Updates transaction, conversion, and realized-demand metrics.

- **TradeInAppraised**  
  Updates used-vehicle valuation and acquisition intelligence.

- **CampaignPerformanceUpdated**  
  Updates Customer interest and channel-performance analysis.

- **OEMIncentiveProgramPublished**  
  Creates or updates incentive intelligence.

- **OEMIncentiveProgramExpired**  
  Marks related intelligence stale or superseded.

- **VehicleModelLaunched**  
  Creates product-launch monitoring intelligence.

- **VehicleModelDiscontinued**  
  Creates supply, demand, and inventory-risk intelligence.

- **CompetitorListingObserved**  
  Creates or updates competitor inventory and pricing observations.

- **ExternalMarketDataReceived**  
  Starts evidence ingestion, normalization, and validation.

- **RegulatoryChangePublished**  
  Creates regulatory-impact Market Intelligence.

- **FinanceRateUpdated**  
  Updates affordability and finance-market intelligence.

- **SourceReliabilityChanged**  
  Recalculates confidence and may require revalidation of affected records.

- **DataCorrectionReceived**  
  Creates a corrected Market Intelligence version when published evidence changes.

- **LegalHoldApplied**  
  Prevents deletion or archival that conflicts with the legal hold.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `title`
- `observation_summary`
- `observation_details`
- `competitor_offer_summary`
- `competitor_inventory_summary`
- `competitor_finance_summary`
- `competitor_advantage`
- `competitor_weakness`
- `geographic_scope_description`
- `risk_summary`
- `opportunity_summary`
- `analyst_interpretation`
- `ai_interpretation`
- `recommended_action`
- `inference_explanation`
- `alternative_hypotheses`
- Permitted summaries of supporting evidence
- Vehicle, segment, demand, supply, pricing, and trend descriptions

### Fields Excluded from Embeddings

- `market_intelligence_id`
- `dealership_id`
- `source_id`
- `created_by`
- `reviewed_by`
- `approved_by`
- `collection_job_id`
- Provider authentication data
- Source credentials
- Private API responses
- Raw restricted competitor data
- Raw Customer-level behavior data
- Customer identifiers
- Exact internal Vehicle cost
- Exact gross-profit information
- Internal pricing floors
- Internal discount-authority limits
- Confidential OEM allocation data
- Contractually restricted third-party datasets
- Raw unpublished regulatory or legal material
- `raw_data_reference`
- `review_snapshot`
- Restricted `source_snapshot`
- Restricted `evidence_snapshot`

> Semantic indexing must use normalized, licensed, non-personal, and permission-approved content only.

### Structured AI Context Fields

Authorized AI Agents may receive:

- `intelligence_type`
- `scope`
- `status`
- `priority`
- `impact_level`
- `confidence_band`
- `data_quality_status`
- `trend_direction`
- `trend_strength`
- `market_price_min_amount`
- `market_price_average_amount`
- `market_price_median_amount`
- `market_price_max_amount`
- `price_gap_amount`
- `price_gap_percentage`
- `market_listing_count`
- `average_days_on_market`
- `demand_index`
- `supply_trend`
- `scarcity_score`
- `competitive_threat_score`
- `market_opportunity_score`
- `market_risk_score`
- `effective_from`
- `effective_until`
- `expires_at`
- `source_reliability_score`
- `supporting_source_count`
- `contradicting_source_count`
- `evidence_consistency_score`

Restricted pricing, inventory, or management Agents may additionally receive:

- Internal dealership pricing.
- Internal inventory count and age.
- Internal realized transaction averages.
- Restricted competitor comparisons.
- Approved recommendation thresholds.
- Management-only intelligence.

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `market_intelligence_id`
- `intelligence_type`
- `status`
- `scope`
- `vehicle_id`
- `vehicle_model_id`
- `competitor_id`
- `market_region_id`
- `market_segment_id`
- `source_type`
- `visibility`
- `priority`
- `impact_level`
- `confidence_band`
- `effective_from`
- `expires_at`
- `is_current_version`

### Confidence Thresholds

- Source classification requires confidence of at least `0.95`.
- Vehicle make, model, trim, and model-year matching requires confidence of at least `0.95`.
- Competitor identity matching requires confidence of at least `0.95`.
- Geographic matching requires confidence of at least `0.95`.
- Price extraction requires confidence of at least `0.99`.
- Incentive and promotion extraction requires confidence of at least `0.95`.
- Date and effective-period extraction requires confidence of at least `0.99`.
- Demand or supply trend classification requires confidence of at least `0.90`.
- Pricing-impact classification requires confidence of at least `0.90`.
- Recommended commercial actions require composite confidence of at least `0.85`.
- Publication without mandatory human review requires composite confidence of at least `0.95` and an approved high-reliability source policy.
- Conflicting sources require human review regardless of AI confidence.
- No AI confidence score may authorize a price, discount, inventory, Campaign, Trade-In, finance, or Deal change.

### Human Approval Thresholds

- AI Agents cannot approve or publish their own generated intelligence.
- AI Agents cannot directly change Vehicle prices or discount thresholds.
- AI Agents cannot order, acquire, transfer, or dispose of inventory.
- AI Agents cannot modify Campaign budgets.
- AI Agents cannot approve Trade-In values.
- AI Agents cannot present inferred competitor transaction prices as verified facts.
- AI Agents cannot classify confidential or restricted sources as shareable.
- Low-quality, single-source, contradictory, legally sensitive, or commercially critical intelligence requires human review.
- Regulatory, safety, fraud, sanctions, legal, or reputational intelligence requires specialist review.
- Recommendations with `impact_level = HIGH` or `CRITICAL` require authorized management approval before action.

### Provenance and Hallucination Controls

- Every factual statement must reference one or more evidence items.
- AI-generated inferences must be labeled explicitly as inference.
- Unsupported claims must not be published.
- Source URLs, references, retrieval times, and evidence hashes must remain available.
- AI summaries must not alter original observed values.
- Calculated values must preserve formula version and input snapshot.
- Contradictory sources must remain visible to reviewers.
- AI must distinguish advertised prices from verified transaction prices.
- AI must distinguish market availability from dealership inventory availability.
- AI must distinguish correlation from causation.
- AI recommendations must list assumptions, limitations, and alternative hypotheses.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/market-intelligence`

### Methods

- `GET` — list or search Market Intelligence records.
- `POST` — create a Draft Market Intelligence record.
- `GET /{id}` — retrieve one Market Intelligence version.
- `PATCH /{id}` — update permitted fields before publication.
- `POST /{id}/collect` — confirm evidence collection and move the record to `COLLECTED`.
- `POST /{id}/validate` — run source, evidence, quality, and consistency validation.
- `POST /{id}/analyze` — generate governed calculations and analysis.
- `POST /{id}/request-review` — create a Human Review Task.
- `POST /{id}/approve` — approve validated intelligence.
- `POST /{id}/reject` — reject unsuitable intelligence.
- `POST /{id}/publish` — publish an approved intelligence snapshot.
- `POST /{id}/mark-stale` — mark intelligence stale.
- `POST /{id}/refresh` — create refreshed evidence and analysis.
- `POST /{id}/supersede` — create a replacement version.
- `POST /{id}/archive` — move an eligible record to historical retention.
- `POST /{id}/create-pricing-review` — create a non-binding pricing-review request.
- `POST /{id}/create-inventory-review` — create a non-binding inventory-review request.
- `POST /{id}/create-campaign-review` — create a non-binding Campaign-review request.
- `DELETE /{id}` — perform a soft delete only when legally and operationally permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateMarketIntelligenceRequest",
  "type": "object",
  "properties": {
    "source_id": {
      "type": "string",
      "format": "uuid"
    },
    "vehicle_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "vehicle_model_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "competitor_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "market_region_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "market_segment_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "intelligence_type": {
      "type": "string",
      "enum": [
        "MARKET_TREND",
        "DEMAND_TREND",
        "SUPPLY_TREND",
        "PRICING_TREND",
        "COMPETITOR_PRICING",
        "COMPETITOR_PROMOTION",
        "COMPETITOR_INVENTORY",
        "CUSTOMER_PREFERENCE",
        "SEARCH_TREND",
        "LEAD_TREND",
        "CONVERSION_TREND",
        "VEHICLE_AVAILABILITY",
        "INVENTORY_PRESSURE",
        "INCENTIVE_PROGRAM",
        "FINANCE_RATE_TREND",
        "TRADE_IN_VALUE_TREND",
        "VEHICLE_LAUNCH",
        "VEHICLE_DISCONTINUATION",
        "REGULATORY_CHANGE",
        "ECONOMIC_INDICATOR",
        "SEASONAL_PATTERN",
        "MARKET_RISK",
        "MARKET_OPPORTUNITY",
        "OTHER"
      ]
    },
    "scope": {
      "type": "string",
      "enum": [
        "VEHICLE",
        "VEHICLE_MODEL",
        "PRODUCT_CATEGORY",
        "MARKET_SEGMENT",
        "DEALERSHIP",
        "DEALER_GROUP",
        "CITY",
        "REGIONAL",
        "NATIONAL",
        "INTERNATIONAL"
      ]
    },
    "source_type": {
      "type": "string",
      "enum": [
        "INTERNAL_DATA",
        "DEALERSHIP_NETWORK",
        "OEM",
        "GOVERNMENT",
        "REGULATOR",
        "MARKETPLACE",
        "COMPETITOR_WEBSITE",
        "PRICING_PROVIDER",
        "VALUATION_PROVIDER",
        "CREDIT_PROVIDER",
        "INDUSTRY_REPORT",
        "NEWS_SOURCE",
        "SOCIAL_MEDIA",
        "CUSTOMER_RESEARCH",
        "SURVEY",
        "MANUAL_OBSERVATION",
        "API_INTEGRATION",
        "AI_GENERATED",
        "OTHER"
      ]
    },
    "priority": {
      "type": "string",
      "enum": [
        "LOW",
        "STANDARD",
        "HIGH",
        "URGENT",
        "CRITICAL"
      ]
    },
    "visibility": {
      "type": "string",
      "enum": [
        "INTERNAL",
        "SALES_TEAM",
        "PRICING_TEAM",
        "INVENTORY_TEAM",
        "MANAGEMENT_ONLY",
        "RESTRICTED",
        "AI_AUTHORIZED",
        "PARTNER_SHARED"
      ]
    },
    "title": {
      "type": "string",
      "minLength": 5,
      "maxLength": 250
    },
    "observation_summary": {
      "type": "string",
      "minLength": 1,
      "maxLength": 5000
    },
    "observation_details": {
      "type": ["string", "null"],
      "maxLength": 50000
    },
    "observed_at": {
      "type": "string",
      "format": "date-time"
    },
    "effective_from": {
      "type": "string",
      "format": "date-time"
    },
    "effective_until": {
      "type": ["string", "null"],
      "format": "date-time"
    },
    "expires_at": {
      "type": "string",
      "format": "date-time"
    },
    "source_name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 250
    },
    "source_reference": {
      "type": "string",
      "minLength": 1,
      "maxLength": 1000
    },
    "source_url": {
      "type": ["string", "null"],
      "format": "uri"
    },
    "source_reliability_score": {
      "type": "number",
      "minimum": 0,
      "maximum": 1
    },
    "currency_code": {
      "type": ["string", "null"],
      "pattern": "^[A-Z]{3}$"
    },
    "market_price_min_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "market_price_average_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "market_price_median_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "market_price_max_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "competitor_price_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "market_listing_count": {
      "type": ["integer", "null"],
      "minimum": 0
    },
    "evidence_references": {
      "type": "array",
      "items": {
        "type": "string",
        "maxLength": 2000
      }
    },
    "ai_generated": {
      "type": "boolean"
    }
  },
  "required": [
    "source_id",
    "intelligence_type",
    "scope",
    "source_type",
    "priority",
    "visibility",
    "title",
    "observation_summary",
    "observed_at",
    "effective_from",
    "expires_at",
    "source_name",
    "source_reference",
    "source_reliability_score",
    "evidence_references",
    "ai_generated"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type MarketIntelligence {
  id: ID!
  dealershipId: ID!
  sourceId: ID!
  vehicleId: ID
  vehicleModelId: ID
  competitorId: ID
  marketRegionId: ID
  marketSegmentId: ID
  incentiveProgramId: ID
  campaignId: ID
  supersedesIntelligenceId: ID
  intelligenceType: MarketIntelligenceType!
  status: MarketIntelligenceStatus!
  scope: MarketIntelligenceScope!
  sourceType: MarketIntelligenceSourceType!
  priority: MarketIntelligencePriority!
  visibility: MarketIntelligenceVisibility!
  timeHorizon: MarketTimeHorizon
  impactLevel: MarketImpactLevel!
  confidenceBand: MarketConfidenceBand!
  dataQualityStatus: MarketDataQualityStatus!
  title: String!
  observationSummary: String!
  observationDetails: String
  observedAt: DateTime!
  effectiveFrom: DateTime!
  effectiveUntil: DateTime
  expiresAt: DateTime!
  make: String
  model: String
  trim: String
  modelYear: Int
  currencyCode: String
  marketPriceMinAmount: Float
  marketPriceAverageAmount: Float
  marketPriceMedianAmount: Float
  marketPriceMaxAmount: Float
  competitorPriceAmount: Float
  dealershipPriceAmount: Float
  priceGapAmount: Float
  priceGapPercentage: Float
  marketListingCount: Int
  competitorListingCount: Int
  dealershipInventoryCount: Int
  estimatedMarketSupplyDays: Float
  averageDaysOnMarket: Float
  inventoryTurnRate: Float
  demandIndex: Float
  demandTrend: MarketTrendDirection
  scarcityScore: Float
  stockPressureScore: Float
  competitiveThreatScore: Float
  trendDirection: MarketTrendDirection!
  trendStrength: Float
  commercialImpact: String
  riskSummary: String
  opportunitySummary: String
  analystInterpretation: String
  aiInterpretation: String
  recommendedAction: String
  recommendedActionType: RecommendedActionType
  sourceName: String!
  sourceReference: String!
  sourceUrl: String
  sourceReliabilityScore: Float!
  evidenceCount: Int!
  supportingSourceCount: Int!
  contradictingSourceCount: Int!
  evidenceConsistencyScore: Float
  aiGenerated: Boolean!
  aiConfidenceScore: Float
  reviewStatus: MarketIntelligenceReviewStatus!
  reviewOutcome: MarketIntelligenceReviewOutcome
  reviewedBy: ID
  reviewedAt: DateTime
  approvedBy: ID
  approvedAt: DateTime
  publishedAt: DateTime
  intelligenceVersion: Int!
  isCurrentVersion: Boolean!
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `market_intelligence`
- **Source Table:** `market_intelligence_sources`
- **Evidence Table:** `market_intelligence_evidence`
- **Observation Table:** `market_intelligence_observations`
- **Metric Table:** `market_intelligence_metrics`
- **Competitor Table:** `market_competitors`
- **Geographic-Market Table:** `market_regions`
- **Segment Table:** `market_segments`
- **Analysis Table:** `market_intelligence_analysis`
- **AI-Analysis Table:** `market_intelligence_ai_analysis`
- **Review Table:** `market_intelligence_reviews`
- **Publication Table:** `market_intelligence_publications`
- **Recommendation Table:** `market_intelligence_recommendations`
- **Version-History Table:** `market_intelligence_versions`
- **Status-History Table:** `market_intelligence_status_history`
- **Audit Table:** `market_intelligence_audit_log`

### Indexes

- `idx_market_intelligence_tenant_status (dealership_id, status)`  
  Used for collection, validation, review, publication, and stale-intelligence queues.

- `idx_market_intelligence_type_time (dealership_id, intelligence_type, observed_at DESC)`  
  Used for intelligence-category timelines.

- `idx_market_intelligence_vehicle (dealership_id, vehicle_id, status)`  
  Used for Vehicle-specific pricing and availability intelligence.

- `idx_market_intelligence_vehicle_model (dealership_id, vehicle_model_id, observed_at DESC)`  
  Used for model-level demand, pricing, and supply trends.

- `idx_market_intelligence_competitor (dealership_id, competitor_id, observed_at DESC)`  
  Used for competitor monitoring.

- `idx_market_intelligence_region (dealership_id, market_region_id, observed_at DESC)`  
  Used for regional intelligence.

- `idx_market_intelligence_segment (dealership_id, market_segment_id, observed_at DESC)`  
  Used for market-segment analysis.

- `idx_market_intelligence_source (dealership_id, source_id, source_reference)`  
  Used for source traceability and duplicate detection.

- `idx_market_intelligence_expiry (dealership_id, expires_at, status)`  
  Used by freshness and expiry Jobs.

- `idx_market_intelligence_priority (dealership_id, priority, impact_level, status)`  
  Used for management and review queues.

- `idx_market_intelligence_review (dealership_id, review_status, priority, created_at)`  
  Used for analyst-review queues.

- `idx_market_intelligence_current_version (dealership_id, is_current_version, status)`  
  Used to retrieve current intelligence versions.

- `idx_market_intelligence_price_gap (dealership_id, vehicle_model_id, price_gap_percentage)`  
  Used for pricing-review analysis.

- `idx_market_intelligence_demand (dealership_id, market_segment_id, demand_index)`  
  Used for demand ranking.

- `idx_market_intelligence_scarcity (dealership_id, vehicle_model_id, scarcity_score)`  
  Used for scarcity and sourcing analysis.

- `idx_market_intelligence_geo`  
  Use a PostGIS or equivalent spatial index for latitude, longitude, and geographic scope.

- `idx_market_intelligence_evidence_hash (evidence_hash)`  
  Used for duplicate-evidence and tampering detection.

### Unique Constraints

- `UQ_market_intelligence_source_reference (dealership_id, source_id, source_reference, observed_at)`

- `UQ_market_intelligence_series_version (dealership_id, intelligence_type, scope, intelligence_version, vehicle_model_id, competitor_id, market_region_id)`

- `UQ_market_intelligence_current_version`  
  Applies to the configured intelligence-series key when `is_current_version = true`.

- `UQ_market_intelligence_evidence_hash (evidence_hash)`  
  Applies when the normalized evidence package must be globally unique.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `source_id` → `market_intelligence_sources(id)`
- `vehicle_id` → `vehicles(id)` — nullable
- `vehicle_model_id` → `vehicle_models(id)` — nullable
- `competitor_id` → `market_competitors(id)` — nullable
- `market_region_id` → `market_regions(id)` — nullable
- `market_segment_id` → `market_segments(id)` — nullable
- `incentive_program_id` → `incentive_programs(id)` — nullable
- `campaign_id` → `campaigns(id)` — nullable
- `created_by` → `users(id)`
- `reviewed_by` → `users(id)` — nullable
- `approved_by` → `users(id)` — nullable
- `published_by` → `users(id)` — nullable
- `supersedes_intelligence_id` → `market_intelligence(id)` — nullable

### Database Constraints

- `intelligence_version >= 1`
- `source_reliability_score BETWEEN 0.00 AND 1.00`
- `ai_confidence_score BETWEEN 0.00 AND 1.00`
- `scarcity_score BETWEEN 0.00 AND 1.00`
- `stock_pressure_score BETWEEN 0.00 AND 1.00`
- `competitive_threat_score BETWEEN 0.00 AND 1.00`
- `evidence_consistency_score BETWEEN 0.00 AND 1.00`
- `market_opportunity_score BETWEEN 0.00 AND 1.00`
- `market_risk_score BETWEEN 0.00 AND 1.00`
- `latitude BETWEEN -90.00 AND 90.00`
- `longitude BETWEEN -180.00 AND 180.00`
- `radius_km >= 0`
- Counts must be zero or greater.
- Financial amounts must be zero or greater.
- `market_price_min_amount <= market_price_max_amount`
- `market_price_average_amount` must fall between the observed minimum and maximum when both exist.
- `market_price_median_amount` must fall between the observed minimum and maximum when both exist.
- `effective_until > effective_from` when `effective_until` is populated.
- `expires_at > observed_at`
- `evidence_hash IS NOT NULL` before status becomes `VALIDATED`.
- `reviewed_by IS NOT NULL` and `reviewed_at IS NOT NULL` when review is completed.
- `published_at IS NOT NULL` when status is `PUBLISHED`.
- `rejection_reason IS NOT NULL` when status is `REJECTED`.
- `supersedes_intelligence_id IS NOT NULL` when status is `SUPERSEDED`.
- Published snapshots must be immutable.
- Circular supersession relationships are prohibited.

### Storage Strategy

- Preserve raw evidence in immutable object storage or an approved data lake.
- Store normalized observations and metrics separately from raw source payloads.
- Store cryptographic hashes for raw and normalized evidence.
- Large source datasets must be referenced rather than copied into the primary table.
- Contractually restricted data must remain in access-controlled storage.
- Search and vector indexes must use approved normalized content only.
- Source corrections must not overwrite historical raw evidence.
- Formula versions and feature snapshots must be stored for every derived metric.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition by `observed_at`.
- Evidence, metrics, analysis, reviews, recommendations, history, and audit tables must preserve the same tenant-isolation strategy.
- Shared dealer-group intelligence must use explicit authorized sharing records rather than cross-tenant direct access.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Read access to published Sales Team intelligence relevant to assigned Vehicles, Leads, and Opportunities.
- **Sales Manager:** Read access to dealership sales intelligence and permitted recommendation workflows.
- **Pricing Manager:** Read/Write access to pricing intelligence, review requests, and approved pricing-analysis context.
- **Inventory Manager:** Read/Write access to supply, scarcity, sourcing, and inventory-pressure intelligence.
- **Marketing User:** Access to approved demand, preference, Campaign, and segment intelligence.
- **Trade-In Appraiser:** Access to approved used-vehicle and valuation-trend intelligence.
- **Finance User:** Access to approved finance-rate and affordability intelligence without unrestricted competitor or Customer-level data.
- **Market Analyst:** Create, validate, analyze, and submit intelligence for review.
- **Management User:** Access to Management-only and high-impact intelligence.
- **Compliance User:** Access to source-governance, regulatory, legal, restricted, and data-collection review.
- **AI Market Intelligence Agent:** Service Account access limited to approved collection, extraction, normalization, analysis, explanation, and recommendation requests.
- **External Data Integration Service:** Restricted ingestion access using tenant-scoped and source-scoped credentials.
- **Audit Service:** Read-only access to immutable provenance, review, publication, and change records.

### Data Classification

- **Default Level:** `INTERNAL COMMERCIAL DATA`
- **Possible Elevated Levels:**
  - `MANAGEMENT CONFIDENTIAL`
  - `RESTRICTED THIRD-PARTY DATA`
  - `REGULATORY SENSITIVE`
  - `COMPETITOR SENSITIVE`
  - `CONTRACTUALLY RESTRICTED`

Market Intelligence must not contain unnecessary Customer-level PII.

### Commercially Sensitive Fields

- Internal dealership prices
- Internal realized transaction prices
- Vehicle cost information
- Internal discount limits
- Margin targets
- Inventory acquisition plans
- OEM allocation data
- Competitor-monitoring strategy
- Campaign plans
- Source-reliability assessments
- Internal pricing and inventory recommendations
- Management review notes

### Source and Licensing Security

- Every source must have an approved usage classification.
- Source terms, licensing limits, scraping restrictions, and redistribution rules must be recorded.
- Restricted third-party data must not be exposed to unauthorized Users or external partners.
- Data obtained through prohibited access methods must be rejected.
- Source credentials must never be stored in Market Intelligence records.
- API keys, tokens, and session credentials must be stored in a secrets-management system.
- Partner-shared intelligence must use explicit sharing agreements and field-level controls.
- AI model training must not use restricted source data unless legally and contractually authorized.

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for databases, raw evidence, snapshots, event stores, and backups.
- **Column-Level Protection:** Restricted competitor data, internal pricing, OEM allocation data, source references, analyst notes, and confidential recommendations require encryption, tokenization, or equivalent protection.
- Raw evidence must be stored in encrypted object storage.
- Source credentials and integration secrets must be stored separately from business data.
- Encryption keys must be separated by environment and rotated according to security policy.

### Audit Requirements

- Every source-ingestion operation must preserve:
  - Source ID
  - Source type
  - Source reference
  - Collection method
  - Collection Job ID
  - Retrieval timestamp
  - Authentication result
  - Evidence hash
  - Processing result

- Every validation decision must preserve:
  - Validator
  - Source reliability
  - Data-quality assessment
  - Supporting evidence
  - Contradictions
  - Limitations
  - Decision
  - Timestamp

- Every calculated metric must preserve:
  - Formula version
  - Input snapshot
  - Calculation timestamp
  - Processing service
  - Output value

- Every AI operation must preserve:
  - Model reference
  - Prompt version
  - Authorized feature snapshot
  - Source scope
  - Output
  - Confidence
  - Explanation
  - Alternative hypotheses
  - Human-review status
  - Timestamp

- Every review and publication operation must preserve:
  - Reviewer
  - Approver
  - Review outcome
  - Visibility
  - Publication channel
  - Effective period
  - Publication snapshot
  - Timestamp

- Every commercial recommendation must preserve:
  - Supporting intelligence
  - Assumptions
  - Confidence
  - Recommended action
  - Action owner
  - Human decision
  - Timestamp

- Every supersession or correction must preserve:
  - Original intelligence ID
  - Replacement intelligence ID
  - Reason
  - Changed evidence or methodology
  - Actor
  - Timestamp

- Human overrides of AI recommendations must retain both the original recommendation and the final human decision.
- Access to restricted competitor, pricing, inventory, source, and management data must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Vehicle, pricing, Campaign, Customer, Lead, Opportunity, Quotation, Deal, Trade-In, source, or Market Intelligence linking is prohibited unless an explicit dealer-group sharing agreement exists.
- Shared intelligence must use approved sharing records, permitted fields, and scoped audiences.
- AI Agents, Jobs, integrations, exports, analytics, and semantic retrieval must receive tenant scope through signed execution context.
- Vector retrieval must enforce tenant, visibility, source-license, effective-period, confidence, and status filters.
- External provider records must be mapped to exactly one tenant or approved shared scope before processing.

### Retention and Deletion

- Market Intelligence retention must follow source licensing, commercial, regulatory, contractual, privacy, audit, and legal-hold requirements.
- Published and superseded intelligence snapshots must remain immutable.
- Raw evidence must be retained only for the period permitted by source agreements and law.
- Restricted third-party data must be deleted or anonymized when the source licence expires or requires removal.
- Records referenced by pricing, inventory, Campaign, Quotation, Deal, Trade-In, forecasting, audit, or legal-hold workflows must not be hard-deleted while dependencies remain.
- Legal hold overrides ordinary deletion and archival schedules.
- Soft deletion is the operational default for eligible records.
- Legally or contractually approved deletion must address:
  - Market Intelligence records
  - Raw evidence
  - Source snapshots
  - Derived metrics
  - AI analysis and embeddings
  - Search indexes
  - Review and publication records
  - Recommendations
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy

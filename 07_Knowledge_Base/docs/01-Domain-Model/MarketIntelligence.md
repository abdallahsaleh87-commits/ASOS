# Market Intelligence

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Market Intelligence Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Market Intelligence Object represents one governed, evidence-backed, time-bounded observation, analytical insight, forecast, risk assessment, or non-binding Recommendation concerning an automotive market.

It enables authorized dealership Users and systems to understand:

- Vehicle demand.
- Vehicle supply.
- Market listing activity.
- Advertised pricing.
- Estimated transaction-price movement.
- Competitor pricing.
- Competitor promotions.
- Competitor Inventory observations.
- Manufacturer incentives.
- Rebate programs.
- Finance-rate trends.
- Customer-search behaviour.
- Lead and Opportunity trends.
- Conversion trends.
- Used-Vehicle valuation movements.
- Vehicle launches.
- Vehicle discontinuations.
- Vehicle specification changes.
- Geographic demand differences.
- Seasonal patterns.
- Economic indicators.
- Regulatory developments.
- Inventory pressure.
- Commercial opportunities.
- Commercial risks.

Market Intelligence provides governed decision support.

It does not independently authorize:

- Vehicle-price changes.
- Discount changes.
- Customer-specific offers.
- Inventory acquisition.
- Inventory transfer.
- Vehicle Reservation.
- Vehicle Allocation.
- Marketing expenditure.
- Campaign launch.
- Finance-rate commitments.
- Trade-In appraisal approval.
- Quotation modification.
- Deal modification.
- Contract modification.
- Customer communication.
- External system Commands.

Any binding action derived from Market Intelligence must pass the responsible Domain Service, Business Rules, approval, policy, compliance, and Command Orchestration workflows.

### Market Intelligence Domain Boundary

The Market Intelligence Domain owns:

- Canonical market-observation records.
- Evidence and provenance relationships.
- Normalized market-data projections.
- Comparability context.
- Analytical snapshots.
- Forecast snapshots.
- Risk and opportunity assessments.
- Recommendation records.
- Review and publication workflow.
- Freshness and expiration state.
- Version and supersession history.
- Downstream-consumption records.
- Market-intelligence audit evidence.

It does not own authoritative:

- Vehicle identity.
- Inventory state.
- Dealership selling price.
- Customer identity.
- Lead qualification.
- Opportunity stage.
- Quotation terms.
- Trade-In appraisal.
- Lender Decision.
- Financial Contract.
- Deal outcome.
- Campaign execution.
- Accounting outcome.

Those facts remain owned by their responsible Domain Services or configured authoritative external systems.

### Observation, Analysis, Recommendation, and Action Separation

The following concepts must remain explicit and separate:

```text
Source Evidence
  = original report, feed, listing, API response, document,
    approved manual observation, or internal aggregate dataset

Observed Fact
  = normalized statement directly supported by source evidence

Calculated Metric
  = deterministic output produced from governed inputs and formulas

Analyst Interpretation
  = Human interpretation of observed facts and calculated metrics

AI-Derived Intelligence
  = model-generated inference, forecast, classification, or explanation

Recommendation
  = non-binding proposed commercial or operational response

Authoritative Human Decision
  = approved Decision made by an authorized Human role

Command
  = governed request to an operational or external system

External Confirmation
  = authoritative evidence that the commanded action completed
```

A Recommendation must never be stored as though it were an approved Decision.

A Decision must never be stored as though the related operational action completed.

### Market Observation and Market Intelligence Separation

A Market Observation records evidence-backed information about a market condition.

Examples include:

- One competitor listing.
- One manufacturer incentive announcement.
- One approved marketplace data batch.
- One published finance-rate table.
- One government economic indicator.
- One internal aggregate demand snapshot.

Market Intelligence may analyze one or more Market Observations to produce:

- A validated trend.
- A benchmark.
- A forecast.
- A risk assessment.
- An opportunity assessment.
- A Recommendation.

The source observations must remain traceable and must not be overwritten by the analysis.

### External and Internal Intelligence Separation

Market Intelligence may use:

#### External Sources

- OEM feeds.
- Government publications.
- Regulatory publications.
- Approved marketplaces.
- Approved pricing providers.
- Approved valuation providers.
- Approved finance-rate providers.
- Licensed industry reports.
- Approved public competitor information.
- Approved economic-data providers.
- Approved research sources.

#### Internal Sources

- Aggregated Inventory statistics.
- Aggregated Lead statistics.
- Aggregated Qualified Lead statistics.
- Aggregated Opportunity statistics.
- Aggregated Quotation statistics.
- Aggregated Deal statistics.
- Aggregated Interaction statistics.
- Aggregated Trade-In statistics.
- Aggregated sales-performance data.
- Aggregated Customer-search or browsing data where lawfully permitted.

Internal market intelligence must use governed aggregation.

It must not expose unnecessary individual Customer, Applicant, User, or transaction-level data.

### Competitor Intelligence Boundary

Competitor Intelligence may include lawfully obtained and appropriately licensed:

- Public advertised prices.
- Public promotions.
- Public Inventory listings.
- Public finance-offer descriptions.
- Public business-location information.
- Public Vehicle specifications.
- Public campaign observations.
- Licensed third-party competitor datasets.

Market Intelligence must not contain or encourage:

- Unauthorized access.
- Circumvention of access controls.
- Credential misuse.
- Deceptive collection.
- Prohibited scraping.
- Trade-secret theft.
- Personal-data misuse.
- Collusion.
- Price fixing.
- Market allocation.
- Unlawful competitor coordination.
- Another prohibited collection or commercial practice.

Publicly advertised competitor prices must not be represented as final transaction prices without authoritative evidence.

### Customer Data Boundary

Market Intelligence may use Customer-derived aggregates only when permitted.

Market Intelligence must not become a repository for:

- Customer names.
- Phone numbers.
- Email addresses.
- Home addresses.
- National identifiers.
- Finance documents.
- Credit information.
- Payment information.
- Full Interaction content.
- Sensitive individual behaviour profiles.

Customer-derived analysis must preserve:

- Purpose.
- Legal basis.
- Aggregation method.
- Minimum aggregation requirements.
- Suppression rules.
- Source-data versions.
- Retention.
- Privacy restrictions.

### Market Intelligence and Vehicle Separation

Vehicle Domain Service owns canonical Vehicle identity and specification.

Market Intelligence may reference:

- Specific Vehicle.
- Vehicle Model.
- Vehicle configuration.
- Make.
- Model.
- Trim.
- Model year.
- Body type.
- Fuel type.
- Transmission.
- Product category.
- Market segment.

Observed source descriptions may not exactly match canonical Vehicle terminology.

Matching and normalization must preserve:

- Original source description.
- Canonical candidate.
- Match method.
- Match confidence where meaningful.
- Human Review where required.
- Normalization version.

### Market Intelligence and Inventory Separation

Inventory Domain Service owns authoritative dealership Inventory state.

Market Intelligence may provide decision support concerning:

- Market supply.
- Listing volume.
- Average days on market.
- Scarcity.
- Inventory pressure.
- Acquisition opportunities.
- Transfer opportunities.
- Pricing pressure.
- Inventory ageing risk.

Market Intelligence must not directly:

- Change Inventory availability.
- Reserve a Vehicle.
- Allocate a Vehicle.
- Transfer Inventory.
- Mark a Vehicle sold.
- Mark a Vehicle delivered.
- Create an acquisition order.

### Market Intelligence and Pricing Separation

Market Intelligence may provide:

- Observed advertised-price ranges.
- Price benchmarks.
- Price-position comparisons.
- Price-trend analysis.
- Estimated market values.
- Recommended pricing review.
- Recommended discount-policy review.

It does not own authoritative dealership prices.

A pricing Recommendation must not directly change:

- Inventory price.
- Quotation price.
- Discount.
- Incentive.
- Trade-In allowance.
- Customer-specific offer.

All authoritative monetary calculations and approved changes must use deterministic pricing, tax, finance, and approval services.

### Market Intelligence and Trade-In Separation

Trade-In Domain Service owns:

- Trade-In identity.
- Inspection.
- Condition.
- Appraisal.
- Actual cash value.
- Customer allowance.
- Payoff.
- Equity.
- Acquisition.

Market Intelligence may support:

- Used-Vehicle market benchmarks.
- Wholesale-value trends.
- Retail-value trends.
- Auction observations.
- Depreciation trends.
- Model-specific demand.
- Reconditioning-cost trends.

An intelligence benchmark is not an approved Trade-In appraisal.

### Market Intelligence and Finance Separation

Market Intelligence may observe:

- Published finance rates.
- Lender program changes.
- Average payment trends.
- Affordability trends.
- Macroeconomic interest-rate changes.
- Approved finance-product availability.

It must not:

- Create a Customer-specific finance rate.
- Predict an authoritative Lender Decision.
- Replace a Finance Application.
- Replace a Lender offer.
- Alter Financial Contract terms.
- Present a market finance rate as a guaranteed Customer offer.

### Market Intelligence and Regulatory Information Separation

Market Intelligence may capture evidence of:

- Published regulatory changes.
- Tax changes.
- Registration changes.
- Import restrictions.
- Consumer-protection developments.
- Environmental requirements.
- Finance or advertising requirements.
- Data-protection developments.

Market Intelligence is not legal advice.

A regulatory observation must be reviewed by an authorized legal, compliance, tax, or regulatory role before it changes binding Business Rules or operational policy.

### Market Intelligence Series and Versioning

A Market Intelligence series represents a continuing intelligence topic or monitored market question.

A Market Intelligence version represents one immutable published analytical snapshot.

The model must distinguish:

```text
intelligence_series_id
  = stable identifier for the continuing intelligence topic

market_intelligence_id
  = identifier for one specific intelligence version

intelligence_version
  = sequential version within the series
```

Draft content may be revised under concurrency controls.

After publication:

- Observation snapshots are immutable.
- Evidence snapshots are immutable.
- Calculation snapshots are immutable.
- Analysis snapshots are immutable.
- Review snapshots are immutable.
- Publication snapshots are immutable.

A material change requires a new Market Intelligence version.

### Freshness and Expiration

Every Market Intelligence record must define an applicable freshness model.

Freshness may depend on:

- Intelligence type.
- Source type.
- Market volatility.
- Vehicle category.
- Geographic scope.
- Time horizon.
- Business purpose.
- Policy.
- Source publication frequency.

A stale or expired intelligence record must not be silently used as current decision support.

A historical record remains available for authorized trend analysis and audit.

### Publication and Consumption

Publication means that a validated intelligence version is made available to authorized consumers.

Publication does not authorize a commercial action.

Every significant downstream use should preserve:

- Intelligence identifier.
- Intelligence version.
- Consumer.
- Purpose.
- Consumed fields.
- Consumption time.
- Freshness state.
- Recommendation used.
- Decision made.
- Resulting Command where applicable.
- Resulting External Confirmation where applicable.

### System Purpose

The Market Intelligence Object provides governed intelligence context to:

- Pricing workflows.
- Discount-policy workflows.
- Inventory planning.
- Vehicle sourcing.
- Inventory transfer planning.
- Sales Playbooks.
- Customer-to-Vehicle matching.
- Campaign planning.
- Trade-In support.
- Finance-program review.
- Executive dashboards.
- Forecasting.
- Risk monitoring.
- AI Agents.
- Analytics.
- Audit and compliance workflows.

The Market Intelligence Object may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Original external source content | Approved External Source |
| Internal operational facts | Responsible Domain Service or approved data platform |
| Canonical source registration | Source Registry |
| Canonical Market Observation | Market Intelligence Domain Service |
| Deterministic calculated metric | Approved Calculation or Analytics Service |
| Analyst interpretation | Authorized Human Analyst |
| AI analysis or forecast | Derived Intelligence |
| Review and publication Decision | Authorized Human role or approved publication policy |
| Vehicle identity | Vehicle Domain Service |
| Inventory state | Inventory Domain Service |
| Customer identity and Consent | Customer Domain Service |
| Quotation terms | Quotation Domain Service |
| Trade-In appraisal | Trade-In Domain Service |
| Lender Decision | Lender through Finance Application |
| Financial Contract | Financial Contract Domain Service |
| Deal state | Deal Domain Service |
| Pricing, acquisition, Campaign, or Inventory Decision | Authorized Human role or approved deterministic workflow |
| External action completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `market_intelligence_id` — UUIDv4, required and immutable.
- `intelligence_series_id` — UUIDv4, required and immutable within the series.
- `tenant_id` — UUIDv4, required and immutable.
- `intelligence_version` — Integer, required.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `legal_entity_id`.
- `department_id`.
- `market_intelligence_team_id`.
- `responsible_analyst_user_id`.
- `responsible_manager_user_id`.
- `responsible_data_steward_user_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `vehicle_id`.
- `vehicle_model_reference`.
- `inventory_record_ids`.
- `lead_segment_reference`.
- `qualified_lead_segment_reference`.
- `opportunity_segment_reference`.
- `quotation_segment_reference`.
- `trade_in_segment_reference`.
- `finance_program_reference`.
- `deal_segment_reference`.
- `campaign_reference`.
- `playbook_reference`.
- `pricing_policy_reference`.
- `market_region_id`.
- `market_segment_id`.
- `competitor_ids`.
- `source_ids`.
- `observation_ids`.
- `calculation_ids`.
- `recommendation_ids`.
- `review_decision_ids`.

### Intelligence Identity

- `intelligence_number`.
- `title`.
- `intelligence_type`.
- `intelligence_subtype`.
- `status`.
- `scope`.
- `priority`.
- `visibility`.
- `time_horizon`.
- `impact_level`.
- `workflow_authority_mode`.
- `is_current_version`.
- `supersedes_market_intelligence_id`.
- `superseded_by_market_intelligence_id`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.
- `publication_status`.

### Intelligence Question

- `business_question`.
- `decision_context`.
- `intended_consumers`.
- `intended_use_cases`.
- `excluded_use_cases`.
- `required_evidence_types`.
- `required_source_diversity`.
- `required_comparability_dimensions`.
- `analysis_method_reference`.
- `analysis_method_version`.
- `intelligence_question_snapshot`.
- `intelligence_question_snapshot_hash`.

### Geographic Scope

- `country_codes`.
- `region_codes`.
- `city_codes`.
- `postal_area_references`.
- `market_region_id`.
- `market_region_name`.
- `latitude`.
- `longitude`.
- `radius_km`.
- `geographic_scope_type`.
- `geographic_scope_description`.
- `geographic_normalization_reference`.
- `geographic_snapshot`.
- `geographic_snapshot_hash`.

Precise coordinates must be used only when necessary and lawfully permitted.

### Product and Vehicle Scope

- `vehicle_id`.
- `vehicle_model_reference`.
- `vehicle_catalog_configuration_reference`.
- `make`.
- `model`.
- `trim`.
- `model_year_from`.
- `model_year_to`.
- `vehicle_condition`.
- `body_types`.
- `fuel_types`.
- `transmissions`.
- `drivetrain_types`.
- `product_category`.
- `market_segment_id`.
- `market_segment_name`.
- `vehicle_scope_description`.
- `vehicle_normalization_reference`.
- `vehicle_normalization_version`.
- `vehicle_scope_snapshot`.
- `vehicle_scope_snapshot_hash`.

### Time Scope

- `observation_period_start`.
- `observation_period_end`.
- `comparison_period_start`.
- `comparison_period_end`.
- `forecast_period_start`.
- `forecast_period_end`.
- `effective_from`.
- `effective_until`.
- `expires_at`.
- `recommended_refresh_at`.
- `freshness_policy_id`.
- `freshness_policy_version`.
- `freshness_status`.
- `freshness_evaluated_at`.

### Source Records

- `source_ids`.
- `primary_source_id`.
- `source_count`.
- `source_type_summary`.
- `source_authority_summary`.
- `source_diversity_status`.
- `source_reliability_status`.
- `source_license_status`.
- `source_privacy_status`.
- `source_security_status`.
- `source_snapshot`.
- `source_snapshot_hash`.

Each source record may contain:

- `market_source_id`.
- `source_name`.
- `source_type`.
- `source_authority_type`.
- `source_owner`.
- `source_reference`.
- `source_url_reference`.
- `source_provider_reference`.
- `source_account_reference`.
- `source_published_at`.
- `source_retrieved_at`.
- `source_effective_from`.
- `source_effective_until`.
- `source_license_reference`.
- `source_license_version`.
- `source_usage_restrictions`.
- `source_geographic_scope`.
- `source_product_scope`.
- `source_reliability_assessment`.
- `source_reliability_method`.
- `source_status`.
- `source_security_classification`.
- `source_retention_class`.

### Source Ingestion

- `collection_job_ids`.
- `collection_batch_ids`.
- `ingestion_status`.
- `ingestion_method`.
- `ingestion_provider`.
- `ingestion_requested_at`.
- `ingestion_started_at`.
- `ingestion_completed_at`.
- `source_record_count`.
- `accepted_record_count`.
- `rejected_record_count`.
- `duplicate_record_count`.
- `quarantined_record_count`.
- `ingestion_deduplication_keys`.
- `ingestion_error_references`.
- `ingestion_reconciliation_status`.

Source-ingestion deduplication keys remain separate from:

- ASOS `event_id`.
- Command `idempotency_key`.
- Canonical `market_intelligence_id`.

### Evidence Package

- `evidence_package_id`.
- `evidence_count`.
- `supporting_evidence_count`.
- `contradicting_evidence_count`.
- `neutral_evidence_count`.
- `evidence_references`.
- `raw_data_references`.
- `normalized_data_references`.
- `evidence_hash`.
- `evidence_snapshot`.
- `evidence_snapshot_hash`.
- `evidence_integrity_status`.
- `evidence_completeness_status`.
- `evidence_consistency_status`.
- `evidence_provenance_status`.
- `evidence_security_classification`.

Each evidence item may contain:

- `market_evidence_id`.
- `market_source_id`.
- `source_record_reference`.
- `evidence_type`.
- `evidence_role`.
- `original_value`.
- `normalized_value`.
- `unit`.
- `currency_code`.
- `observed_at`.
- `published_at`.
- `retrieved_at`.
- `product_scope`.
- `geographic_scope`.
- `content_reference`.
- `content_hash`.
- `normalization_reference`.
- `normalization_version`.
- `verification_status`.
- `exclusion_status`.
- `exclusion_reason`.

### Observation Records

- `observation_ids`.
- `primary_observation_id`.
- `observation_count`.
- `observation_type`.
- `observation_summary`.
- `observation_details`.
- `observation_method`.
- `observation_unit`.
- `observation_value`.
- `previous_observation_value`.
- `change_amount`.
- `change_percentage`.
- `observed_at`.
- `observation_status`.
- `observation_snapshot`.
- `observation_snapshot_hash`.

Each observation must preserve the supporting evidence references.

### Comparability Context

- `comparability_status`.
- `comparability_rule_id`.
- `comparability_rule_version`.
- `comparison_dimensions`.
- `vehicle_equivalence_status`.
- `condition_equivalence_status`.
- `geographic_equivalence_status`.
- `time_equivalence_status`.
- `currency_equivalence_status`.
- `tax_equivalence_status`.
- `fee_equivalence_status`.
- `incentive_equivalence_status`.
- `finance_equivalence_status`.
- `data-source-equivalence_status`.
- `normalization_adjustments`.
- `comparability_limitations`.
- `comparability_snapshot`.
- `comparability_snapshot_hash`.

### Sampling and Coverage

- `population_definition`.
- `sample_definition`.
- `sample_size`.
- `unique_source_count`.
- `unique_seller_count`.
- `unique_listing_count`.
- `coverage_percentage`.
- `coverage_status`.
- `sampling_method`.
- `sampling_period`.
- `sampling_bias_assessment`.
- `missing_data_percentage`.
- `outlier_count`.
- `excluded_record_count`.
- `coverage_limitations`.
- `sample_snapshot`.
- `sample_snapshot_hash`.

Required sample and diversity thresholds must remain policy-configurable.

### Currency and Unit Normalization

- `original_currency_codes`.
- `normalized_currency_code`.
- `currency_conversion_required`.
- `currency_rate_source`.
- `currency_rate_reference`.
- `currency_rate_timestamp`.
- `currency_conversion_rule_version`.
- `tax_inclusion_basis`.
- `fee_inclusion_basis`.
- `unit_normalization_reference`.
- `unit_normalization_version`.
- `rounding_rule_reference`.
- `normalization_snapshot`.
- `normalization_snapshot_hash`.

### Pricing Intelligence

- `market_price_min_amount`.
- `market_price_average_amount`.
- `market_price_median_amount`.
- `market_price_max_amount`.
- `market_price_percentiles`.
- `advertised_price_average_amount`.
- `estimated_transaction_price_amount`.
- `competitor_price_min_amount`.
- `competitor_price_average_amount`.
- `competitor_price_median_amount`.
- `competitor_price_max_amount`.
- `internal_price_reference`.
- `internal_price_amount`.
- `price_gap_amount`.
- `price_gap_percentage`.
- `discount_average_amount`.
- `discount_median_amount`.
- `incentive_average_amount`.
- `price_index`.
- `price_trend`.
- `price_volatility`.
- `pricing_observation_count`.
- `pricing_calculation_reference`.
- `pricing_snapshot`.
- `pricing_snapshot_hash`.

Advertised price and estimated transaction price must remain distinguishable.

### Supply and Inventory Intelligence

- `market_listing_count`.
- `active_market_listing_count`.
- `new_listing_count`.
- `removed_listing_count`.
- `competitor_listing_count`.
- `internal_inventory_count_projection`.
- `estimated_market_supply_days`.
- `average_days_on_market`.
- `median_days_on_market`.
- `listing_turn_rate`.
- `inventory_turn_rate_projection`.
- `availability_status`.
- `supply_trend`.
- `scarcity_score`.
- `stock_pressure_score`.
- `supply_volatility_score`.
- `supply_calculation_reference`.
- `supply_snapshot`.
- `supply_snapshot_hash`.

Internal Inventory values remain projections from Inventory authority.

### Demand Intelligence

- `search_volume`.
- `inquiry_count`.
- `lead_count`.
- `qualified_lead_count`.
- `opportunity_count`.
- `quotation_count`.
- `deal_count`.
- `appointment_count`.
- `test_drive_count`.
- `conversion_rate`.
- `lead_to_qualified_rate`.
- `qualified_to_opportunity_rate`.
- `opportunity_to_quotation_rate`.
- `quotation_to_deal_rate`.
- `demand_index`.
- `demand_trend`.
- `customer_interest_score`.
- `regional_preference_score`.
- `demand_volatility_score`.
- `demand_calculation_reference`.
- `demand_snapshot`.
- `demand_snapshot_hash`.

Customer inquiry volume must not be represented as completed demand.

### Competitor Intelligence

- `competitor_ids`.
- `competitor_count`.
- `competitor_name_projections`.
- `competitor_location_projections`.
- `competitor_offer_summaries`.
- `competitor_discount_observations`.
- `competitor_incentive_observations`.
- `competitor_inventory_observations`.
- `competitor_finance_observations`.
- `competitor_campaign_observations`.
- `competitor_strength_summary`.
- `competitor_weakness_summary`.
- `competitive_position_summary`.
- `competitive_threat_score`.
- `competitive_opportunity_score`.
- `competitor_evidence_references`.
- `competitor_intelligence_snapshot`.
- `competitor_intelligence_snapshot_hash`.

Strength and weakness summaries are interpretations, not observed facts.

### Incentive Intelligence

- `manufacturer_incentive_references`.
- `dealer_incentive_references`.
- `government_incentive_references`.
- `incentive_type`.
- `incentive_amount`.
- `incentive_percentage`.
- `incentive_currency_code`.
- `incentive_effective_from`.
- `incentive_effective_until`.
- `incentive_eligibility_summary`.
- `incentive_geographic_scope`.
- `incentive_vehicle_scope`.
- `incentive_status`.
- `incentive_evidence_references`.
- `incentive_snapshot`.
- `incentive_snapshot_hash`.

Observed incentive eligibility must not be applied to a Customer without deterministic eligibility validation.

### Finance-Rate and Affordability Intelligence

- `finance_provider_references`.
- `finance_product_types`.
- `published_rate_min`.
- `published_rate_average`.
- `published_rate_median`.
- `published_rate_max`.
- `published_annual_percentage_rate_min`.
- `published_annual_percentage_rate_max`.
- `typical_term_months`.
- `typical_down_payment_percentage`.
- `estimated_periodic_payment_ranges`.
- `affordability_index`.
- `finance_rate_trend`.
- `finance_product_availability_status`.
- `finance_evidence_references`.
- `finance_intelligence_snapshot`.
- `finance_intelligence_snapshot_hash`.

These values are market observations and must not be represented as Customer-specific Lender terms.

### Trade-In and Used-Vehicle Intelligence

- `used_vehicle_price_index`.
- `wholesale_value_index`.
- `retail_value_index`.
- `auction_value_index`.
- `depreciation_rate`.
- `appreciation_rate`.
- `market_liquidity_index`.
- `condition_adjustment_observations`.
- `mileage_adjustment_observations`.
- `reconditioning_cost_trend`.
- `trade_in_demand_index`.
- `trade_in_supply_index`.
- `trade_in_evidence_references`.
- `trade_in_intelligence_snapshot`.
- `trade_in_intelligence_snapshot_hash`.

### Economic Intelligence

- `economic_indicator_references`.
- `inflation_rate_projection`.
- `interest_rate_projection`.
- `exchange_rate_projection`.
- `consumer_confidence_projection`.
- `fuel_price_projection`.
- `unemployment_rate_projection`.
- `vehicle_import_cost_projection`.
- `economic_trend_summary`.
- `economic_impact_summary`.
- `economic_evidence_references`.
- `economic_snapshot`.
- `economic_snapshot_hash`.

External economic values must preserve source and observation date.

### Regulatory Intelligence

- `regulatory_source_references`.
- `regulatory_change_type`.
- `regulatory_jurisdiction`.
- `regulatory_publication_reference`.
- `regulatory_effective_date`.
- `regulatory_summary`.
- `potential_business_impact`.
- `legal_review_required`.
- `legal_review_status`.
- `compliance_review_required`.
- `compliance_review_status`.
- `regulatory_evidence_references`.
- `regulatory_snapshot`.
- `regulatory_snapshot_hash`.

Regulatory summaries remain informational until reviewed by an authorized role.

### Internal Aggregate Intelligence

- `internal_dataset_references`.
- `internal_metric_definitions`.
- `internal_aggregation_level`.
- `internal_aggregation_period`.
- `internal_population_count`.
- `internal_suppression_applied`.
- `internal_privacy_threshold_status`.
- `internal_data_quality_status`.
- `internal_source_record_versions`.
- `internal_aggregate_snapshot`.
- `internal_aggregate_snapshot_hash`.

### Deterministic Calculations

- `calculation_ids`.
- `calculation_status`.
- `formula_references`.
- `formula_versions`.
- `calculation_input_references`.
- `calculation_input_hash`.
- `calculation_output_snapshot`.
- `calculation_output_hash`.
- `calculated_at`.
- `calculation_expires_at`.
- `calculation_validation_status`.
- `calculation_reconciliation_status`.

Every authoritative metric must preserve its deterministic calculation lineage.

### Trend Analysis

- `trend_direction`.
- `trend_strength`.
- `trend_start_at`.
- `trend_end_at`.
- `trend_duration`.
- `trend_change_points`.
- `trend_volatility`.
- `trend_seasonality`.
- `trend_baseline_reference`.
- `trend_method_reference`.
- `trend_method_version`.
- `trend_limitations`.
- `trend_snapshot`.
- `trend_snapshot_hash`.

### Forecast

- `forecast_status`.
- `forecast_method_type`.
- `forecast_model_reference`.
- `forecast_model_version`.
- `forecast_target`.
- `forecast_horizon`.
- `forecast_values`.
- `forecast_ranges`.
- `forecast_confidence_intervals`.
- `forecast_scenarios`.
- `forecast_assumptions`.
- `forecast_limitations`.
- `forecast_input_snapshot`.
- `forecast_input_snapshot_hash`.
- `forecast_generated_at`.
- `forecast_expires_at`.
- `forecast_review_status`.

A forecast must not be represented as a guaranteed future outcome.

### Analyst Interpretation

- `analyst_interpretation`.
- `analyst_assumptions`.
- `analyst_limitations`.
- `alternative_hypotheses`.
- `contradicting_evidence_summary`.
- `analyst_user_id`.
- `analyst_completed_at`.
- `analyst_snapshot`.
- `analyst_snapshot_hash`.

### AI-Derived Intelligence

- `ai_processing_status`.
- `ai_agent_run_ids`.
- `ai_model_reference`.
- `ai_model_version`.
- `ai_prompt_version`.
- `ai_feature_snapshot`.
- `ai_input_snapshot_hash`.
- `ai_interpretation`.
- `ai_forecast`.
- `ai_risk_summary`.
- `ai_opportunity_summary`.
- `ai_explanation`.
- `ai_alternative_hypotheses`.
- `ai_confidence_score`.
- `ai_limitations`.
- `ai_generated_at`.
- `ai_expires_at`.
- `ai_review_status`.

### Commercial Impact Assessment

- `commercial_impact`.
- `inventory_impact`.
- `pricing_impact`.
- `sales_impact`.
- `customer_impact`.
- `trade_in_impact`.
- `finance_impact`.
- `campaign_impact`.
- `operational_impact`.
- `compliance_impact`.
- `estimated_financial_impact_range`.
- `impact_time_horizon`.
- `impact_assumptions`.
- `impact_snapshot`.
- `impact_snapshot_hash`.

Estimated impact must remain separate from confirmed financial outcome.

### Risk Assessment

- `market_risk_score`.
- `pricing_risk_score`.
- `inventory_risk_score`.
- `demand_risk_score`.
- `supply_risk_score`.
- `competitor_risk_score`.
- `regulatory_risk_score`.
- `data_risk_score`.
- `risk_summary`.
- `risk_factors`.
- `risk_mitigations`.
- `risk_assessment_method_reference`.
- `risk_assessment_method_version`.
- `risk_snapshot`.
- `risk_snapshot_hash`.

### Opportunity Assessment

- `market_opportunity_score`.
- `pricing_opportunity_score`.
- `inventory_opportunity_score`.
- `demand_opportunity_score`.
- `sourcing_opportunity_score`.
- `campaign_opportunity_score`.
- `trade_in_opportunity_score`.
- `opportunity_summary`.
- `opportunity_factors`.
- `opportunity_constraints`.
- `opportunity_assessment_method_reference`.
- `opportunity_assessment_method_version`.
- `opportunity_snapshot`.
- `opportunity_snapshot_hash`.

### Recommendation Records

- `recommendation_ids`.
- `primary_recommendation_id`.
- `recommended_action_type`.
- `recommended_action`.
- `recommended_action_scope`.
- `recommended_action_owner_role`.
- `recommended_action_priority`.
- `recommended_action_due_at`.
- `recommended_monitoring_period`.
- `recommendation_evidence_references`.
- `recommendation_assumptions`.
- `recommendation_limitations`.
- `recommendation_expected_impact`.
- `recommendation_risk`.
- `recommendation_action_class`.
- `recommendation_requires_human_decision`.
- `recommendation_expires_at`.
- `recommendation_status`.
- `recommendation_snapshot`.
- `recommendation_snapshot_hash`.

### Human Decision

- `decision_required`.
- `decision_request_ids`.
- `decision_ids`.
- `decision_status`.
- `decision_type`.
- `decision_scope`.
- `decision_snapshot`.
- `decision_snapshot_hash`.
- `decision_outcome`.
- `decision_reason`.
- `decided_by_actor_ids`.
- `decided_at`.
- `decision_effective_from`.
- `decision_expires_at`.
- `decision_evidence_references`.

The Decision record remains separate from the Market Intelligence Recommendation.

### Downstream Action and Consumption

- `consumption_record_ids`.
- `consumer_types`.
- `consumer_references`.
- `consumption_purposes`.
- `consumed_at`.
- `consumed_intelligence_version`.
- `consumed_freshness_status`.
- `resulting_decision_ids`.
- `resulting_command_ids`.
- `resulting_external_confirmation_ids`.
- `resulting_domain_object_references`.
- `outcome_measurement_status`.
- `measured_outcome_references`.

### Review

- `review_required`.
- `review_status`.
- `review_type`.
- `review_reason_codes`.
- `review_request_ids`.
- `reviewer_role_requirements`.
- `assigned_reviewer_ids`.
- `review_outcome`.
- `review_notes`.
- `review_limitations`.
- `reviewed_at`.
- `review_decision_ids`.
- `review_snapshot`.
- `review_snapshot_hash`.

### Publication

- `publication_status`.
- `publication_channel`.
- `publication_audience`.
- `publication_scope`.
- `publication_requested_at`.
- `publication_decision_id`.
- `published_at`.
- `published_by_actor_id`.
- `publication_expires_at`.
- `publication_artifact_reference`.
- `publication_artifact_hash`.
- `publication_snapshot`.
- `publication_snapshot_hash`.
- `publication_withdrawal_status`.
- `publication_withdrawn_at`.

### Quality and Confidence

- `data_quality_status`.
- `evidence_quality_status`.
- `source_quality_status`.
- `comparability_quality_status`.
- `coverage_quality_status`.
- `calculation_quality_status`.
- `analysis_quality_status`.
- `confidence_band`.
- `confidence_score`.
- `confidence_method_reference`.
- `confidence_method_version`.
- `confidence_limitations`.
- `quality_review_required`.

Confidence thresholds must remain configurable and must not be hard-coded into the Canonical Object.

### Conflict and Contradiction

- `conflict_status`.
- `conflict_types`.
- `contradicting_source_ids`.
- `contradicting_observation_ids`.
- `conflict_summary`.
- `conflict_resolution_status`.
- `conflict_resolution_decision_id`.
- `conflict_resolved_at`.
- `conflict_evidence_references`.

Contradictory evidence must be preserved rather than silently removed.

### Computed Projections

- `age_hours`.
- `age_days`.
- `days_until_expiry`.
- `is_current`.
- `is_stale`.
- `freshness_score`.
- `supporting_source_count`.
- `contradicting_source_count`.
- `evidence_consistency_score`.
- `source_diversity_score`.
- `coverage_score`.
- `composite_confidence_score`.
- `market_opportunity_score`.
- `market_risk_score`.
- `recommended_review_at`.
- `last_consumed_at`.
- `consumption_count`.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `source_updated_at`.
- `last_synced_at`.
- `last_sync_status`.
- `reconciliation_status`.
- `field_authority_map`.
- `created_at`.
- `created_by_actor_type`.
- `created_by_actor_id`.
- `updated_at`.
- `updated_by_actor_type`.
- `updated_by_actor_id`.
- `validated_at`.
- `analyzed_at`.
- `reviewed_at`.
- `approved_at`.
- `published_at`.
- `superseded_at`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `market_intelligence_id` | UUID | Yes | ASOS | Immutable identifier for one intelligence version. |
| `intelligence_series_id` | UUID | Yes | ASOS | Stable identifier for the continuing intelligence topic. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `intelligence_number` | String | Yes | ASOS or configured authority | Human-readable intelligence reference. |
| `intelligence_version` | Integer | Yes | ASOS | Sequential version in the intelligence series. |
| `title` | String | Yes | Analyst or approved workflow | Concise intelligence title. |
| `intelligence_type` | Enum | Yes | Workflow | Primary intelligence category. |
| `status` | Enum | Yes | Market Intelligence workflow | Current lifecycle state. |
| `scope` | Enum | Yes | Workflow | Commercial or geographic scope. |
| `priority` | Enum | Yes | Human or policy | Operational review priority. |
| `visibility` | Enum | Yes | Authorization policy | Permitted audience. |
| `time_horizon` | Enum | Yes | Analysis | Applicable time horizon. |
| `impact_level` | Enum | Yes | Analysis or Human Review | Estimated impact significance. |
| `is_current_version` | Boolean | Yes | ASOS | Indicates the current version in the series. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Evidence and Source Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `primary_source_id` | UUID | Conditional | Source Registry | Primary evidence source. |
| `source_ids` | Array | Yes | Source relationships | All sources supporting or contradicting the intelligence. |
| `source_count` | Integer | Yes | Deterministic projection | Number of distinct governed sources. |
| `evidence_package_id` | UUID | Yes | ASOS | Governed evidence package. |
| `evidence_count` | Integer | Yes | Deterministic projection | Number of included evidence records. |
| `evidence_hash` | String | Yes before validation | ASOS | Integrity hash of the normalized evidence package. |
| `source_license_status` | Enum | Yes | Source governance | Whether use is permitted. |
| `source_reliability_status` | Enum | Yes | Source governance | Current reliability assessment. |
| `evidence_integrity_status` | Enum | Yes | Evidence workflow | Evidence-integrity result. |
| `evidence_consistency_status` | Enum | Yes | Validation workflow | Consistency across evidence. |
| `source_snapshot_hash` | String | Yes before publication | ASOS | Integrity hash of source context. |
| `evidence_snapshot_hash` | String | Yes before publication | ASOS | Integrity hash of publication evidence. |

### Scope and Time Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `country_codes` | Array | Conditional | Scope workflow | Countries included in the analysis. |
| `market_region_id` | UUID | Conditional | Market-region registry | Canonical geographic market. |
| `vehicle_model_reference` | String | Conditional | Vehicle catalogue relationship | Canonical Vehicle Model or configuration scope. |
| `market_segment_id` | UUID | No | Market-segment registry | Commercial segment. |
| `observation_period_start` | Timestamp | Yes | Analysis design | Start of the evidence period. |
| `observation_period_end` | Timestamp | Yes | Analysis design | End of the evidence period. |
| `effective_from` | Timestamp | Yes | Review or policy | Start of permitted applicability. |
| `effective_until` | Timestamp | No | Review or policy | End of expected applicability. |
| `expires_at` | Timestamp | Yes | Freshness policy | Time after which unrestricted current use is prohibited. |
| `freshness_status` | Enum | Yes | Deterministic policy | Current freshness classification. |

### Observation and Calculation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `observation_summary` | Text | Yes | Evidence-backed workflow | Concise statement of observed facts. |
| `observation_details` | Text | No | Evidence-backed workflow | Detailed factual description. |
| `observation_type` | Enum | Yes | Workflow | Category of observed market fact. |
| `observation_value` | Decimal or JSON | No | Source or calculation | Current observed value. |
| `previous_observation_value` | Decimal or JSON | No | Source or calculation | Comparison value. |
| `change_amount` | Decimal | No | Deterministic calculation | Absolute change. |
| `change_percentage` | Decimal | No | Deterministic calculation | Percentage change. |
| `calculation_status` | Enum | Yes | Calculation workflow | Current deterministic calculation state. |
| `formula_versions` | Array | Conditional | Calculation authority | Applied formula versions. |
| `calculation_input_hash` | String | Conditional | ASOS | Integrity hash of calculation inputs. |
| `calculation_output_hash` | String | Conditional | ASOS | Integrity hash of calculation outputs. |

### Pricing and Market Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `normalized_currency_code` | String | Conditional | Normalization workflow | ISO 4217 comparison currency. |
| `market_price_min_amount` | Decimal | No | Deterministic calculation | Minimum comparable observed price. |
| `market_price_average_amount` | Decimal | No | Deterministic calculation | Average comparable observed price. |
| `market_price_median_amount` | Decimal | No | Deterministic calculation | Median comparable observed price. |
| `market_price_max_amount` | Decimal | No | Deterministic calculation | Maximum comparable observed price. |
| `estimated_transaction_price_amount` | Decimal | No | Derived Intelligence or licensed source | Estimated transaction-price benchmark. |
| `market_listing_count` | Integer | No | Source or calculation | Count of relevant governed listings. |
| `average_days_on_market` | Decimal | No | Deterministic calculation | Average listing duration. |
| `demand_index` | Decimal | No | Deterministic calculation | Normalized demand indicator. |
| `scarcity_score` | Decimal | No | Deterministic calculation | Configured scarcity indicator. |
| `competitive_threat_score` | Decimal | No | Derived Intelligence | Non-binding threat assessment. |
| `trend_direction` | Enum | Yes after analysis | Analysis | Direction of the observed trend. |

### Analysis and Recommendation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `analyst_interpretation` | Text | No | Authorized Human | Human interpretation separated from observed facts. |
| `ai_interpretation` | Text | No | Derived Intelligence | AI-generated analysis. |
| `risk_summary` | Text | No | Analysis | Identified market risks. |
| `opportunity_summary` | Text | No | Analysis | Identified market opportunities. |
| `recommended_action_type` | Enum | No | Recommendation | Non-binding action category. |
| `recommended_action` | Text | No | Recommendation | Proposed response. |
| `recommendation_action_class` | Enum | Conditional | Policy | Required governance class. |
| `recommendation_expires_at` | Timestamp | Conditional | Policy or analysis | Time after which the Recommendation must be refreshed. |
| `decision_required` | Boolean | Yes | Policy | Whether an authoritative Human Decision is required. |
| `decision_status` | Enum | Yes | Decision workflow | Current Decision state. |

### Review and Publication Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `review_status` | Enum | Yes | Review workflow | Current Human Review state. |
| `review_outcome` | Enum | No | Authorized Human | Review result. |
| `reviewed_at` | Timestamp | No | Review workflow | Time review completed. |
| `publication_status` | Enum | Yes | Publication workflow | Current publication state. |
| `publication_audience` | Array | No | Authorization policy | Permitted consumer roles or systems. |
| `publication_decision_id` | UUID | Conditional | Human Decision | Decision authorizing publication. |
| `published_at` | Timestamp | No | Publication workflow | Time the intelligence became available. |
| `publication_artifact_hash` | String | Conditional | ASOS | Hash of the published artifact. |

---

## 4. Enumerations

### MarketIntelligenceStatus

- `DRAFT`
- `INGESTION_PENDING`
- `COLLECTED`
- `VALIDATION_PENDING`
- `VALIDATED`
- `ANALYSIS_PENDING`
- `ANALYZED`
- `REVIEW_PENDING`
- `APPROVED`
- `PUBLISHED`
- `STALE`
- `REJECTED`
- `SUPERSEDED`
- `ARCHIVED`

### MarketIntelligenceType

- `MARKET_TREND`
- `DEMAND_TREND`
- `SUPPLY_TREND`
- `PRICING_TREND`
- `COMPETITOR_PRICING`
- `COMPETITOR_PROMOTION`
- `COMPETITOR_INVENTORY`
- `CUSTOMER_PREFERENCE`
- `SEARCH_TREND`
- `LEAD_TREND`
- `QUALIFIED_LEAD_TREND`
- `OPPORTUNITY_TREND`
- `QUOTATION_TREND`
- `CONVERSION_TREND`
- `VEHICLE_AVAILABILITY`
- `INVENTORY_PRESSURE`
- `INCENTIVE_PROGRAM`
- `FINANCE_RATE_TREND`
- `AFFORDABILITY_TREND`
- `TRADE_IN_VALUE_TREND`
- `USED_VEHICLE_TREND`
- `VEHICLE_LAUNCH`
- `VEHICLE_DISCONTINUATION`
- `VEHICLE_SPECIFICATION_CHANGE`
- `REGULATORY_CHANGE`
- `ECONOMIC_INDICATOR`
- `SEASONAL_PATTERN`
- `MARKET_RISK`
- `MARKET_OPPORTUNITY`
- `FORECAST`
- `BENCHMARK`
- `OTHER`

### MarketObservationType

- `SOURCE_FACT`
- `LISTING_OBSERVATION`
- `PRICE_OBSERVATION`
- `SUPPLY_OBSERVATION`
- `DEMAND_OBSERVATION`
- `INCENTIVE_OBSERVATION`
- `FINANCE_RATE_OBSERVATION`
- `COMPETITOR_OBSERVATION`
- `ECONOMIC_OBSERVATION`
- `REGULATORY_OBSERVATION`
- `INTERNAL_AGGREGATE_OBSERVATION`
- `MANUAL_FIELD_OBSERVATION`
- `OTHER`

### MarketIntelligenceSourceType

- `INTERNAL_AGGREGATE_DATA`
- `DEALERSHIP_NETWORK`
- `OEM`
- `GOVERNMENT`
- `REGULATOR`
- `MARKETPLACE`
- `COMPETITOR_PUBLIC_SOURCE`
- `PRICING_PROVIDER`
- `VALUATION_PROVIDER`
- `FINANCE_RATE_PROVIDER`
- `ECONOMIC_DATA_PROVIDER`
- `INDUSTRY_REPORT`
- `RESEARCH_PUBLICATION`
- `NEWS_SOURCE`
- `CUSTOMER_RESEARCH`
- `SURVEY`
- `MANUAL_OBSERVATION`
- `APPROVED_API_INTEGRATION`
- `APPROVED_FILE_IMPORT`
- `OTHER`

`AI_GENERATED` must not be used as the source type for observed external facts.

AI output is Derived Intelligence.

### SourceAuthorityType

- `EXTERNAL_AUTHORITATIVE`
- `EXTERNAL_INFORMATIONAL`
- `INTERNAL_AUTHORITATIVE_AGGREGATE`
- `INTERNAL_OPERATIONAL_PROJECTION`
- `LICENSED_THIRD_PARTY`
- `PUBLIC_INFORMATIONAL`
- `AUTHORIZED_HUMAN_OBSERVATION`
- `UNKNOWN`
- `DISPUTED`

### SourceStatus

- `PROPOSED`
- `REVIEW_PENDING`
- `APPROVED`
- `ACTIVE`
- `SUSPENDED`
- `EXPIRED`
- `REVOKED`
- `REJECTED`
- `DISPUTED`

### SourceLicenseStatus

- `NOT_EVALUATED`
- `NOT_REQUIRED`
- `PERMITTED`
- `PERMITTED_WITH_RESTRICTIONS`
- `EXPIRED`
- `NOT_PERMITTED`
- `DISPUTED`
- `LEGAL_REVIEW_REQUIRED`

### MarketIntelligenceScope

- `VEHICLE`
- `VEHICLE_MODEL`
- `VEHICLE_CONFIGURATION`
- `PRODUCT_CATEGORY`
- `MARKET_SEGMENT`
- `BRANCH`
- `DEALERSHIP`
- `DEALER_GROUP`
- `CITY`
- `REGION`
- `NATIONAL`
- `MULTI_COUNTRY`
- `INTERNATIONAL`
- `CUSTOM`

### GeographicScopeType

- `POINT`
- `RADIUS`
- `CITY`
- `POSTAL_AREA`
- `REGION`
- `COUNTRY`
- `MULTI_REGION`
- `MULTI_COUNTRY`
- `CUSTOM`

### MarketIntelligencePriority

- `LOW`
- `STANDARD`
- `HIGH`
- `URGENT`
- `CRITICAL`

Priority must not override evidence, legal, privacy, approval, or security controls.

### MarketIntelligenceVisibility

- `INTERNAL_GENERAL`
- `SALES_TEAM`
- `PRICING_TEAM`
- `INVENTORY_TEAM`
- `MARKETING_TEAM`
- `FINANCE_TEAM`
- `TRADE_IN_TEAM`
- `MANAGEMENT`
- `EXECUTIVE`
- `RESTRICTED`
- `LEGAL_RESTRICTED`
- `LICENSE_RESTRICTED`
- `PARTNER_SHARED`
- `AI_AUTHORIZED`

### MarketTimeHorizon

- `REAL_TIME`
- `IMMEDIATE`
- `SHORT_TERM`
- `MEDIUM_TERM`
- `LONG_TERM`
- `HISTORICAL`
- `CUSTOM`

### MarketImpactLevel

- `NONE`
- `LOW`
- `MODERATE`
- `HIGH`
- `CRITICAL`
- `UNKNOWN`

### MarketTrendDirection

- `STRONGLY_DECREASING`
- `DECREASING`
- `SLIGHTLY_DECREASING`
- `STABLE`
- `SLIGHTLY_INCREASING`
- `INCREASING`
- `STRONGLY_INCREASING`
- `VOLATILE`
- `MIXED`
- `UNKNOWN`

### MarketDataQualityStatus

- `NOT_ASSESSED`
- `INCOMPLETE`
- `LOW_QUALITY`
- `ACCEPTABLE`
- `HIGH_QUALITY`
- `CONFLICTING`
- `INVALID`
- `STALE`
- `QUARANTINED`

### EvidenceIntegrityStatus

- `NOT_ASSESSED`
- `PENDING`
- `VALID`
- `HASH_MISMATCH`
- `INCOMPLETE`
- `CORRUPTED`
- `ALTERED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### EvidenceConsistencyStatus

- `NOT_ASSESSED`
- `CONSISTENT`
- `MOSTLY_CONSISTENT`
- `MIXED`
- `CONTRADICTORY`
- `INSUFFICIENT_EVIDENCE`
- `REVIEW_REQUIRED`

### ComparabilityStatus

- `NOT_ASSESSED`
- `COMPARABLE`
- `COMPARABLE_WITH_ADJUSTMENTS`
- `PARTIALLY_COMPARABLE`
- `NOT_COMPARABLE`
- `REVIEW_REQUIRED`

### CoverageStatus

- `NOT_ASSESSED`
- `INSUFFICIENT`
- `LIMITED`
- `ACCEPTABLE`
- `STRONG`
- `BIASED`
- `UNKNOWN`

### MarketConfidenceBand

- `VERY_LOW`
- `LOW`
- `MEDIUM`
- `HIGH`
- `VERY_HIGH`
- `NOT_APPLICABLE`

Confidence-band assignment must use a governed configurable method.

### MarketFreshnessStatus

- `NOT_EVALUATED`
- `CURRENT`
- `APPROACHING_EXPIRY`
- `STALE`
- `EXPIRED`
- `REFRESH_PENDING`
- `UNKNOWN`

### CalculationStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `QUEUED`
- `PROCESSING`
- `COMPLETED`
- `VALIDATION_PENDING`
- `VALIDATED`
- `FAILED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### ForecastStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `QUEUED`
- `PROCESSING`
- `GENERATED`
- `REVIEW_PENDING`
- `APPROVED`
- `REJECTED`
- `EXPIRED`
- `SUPERSEDED`
- `FAILED`

### ForecastMethodType

- `DETERMINISTIC_PROJECTION`
- `STATISTICAL_MODEL`
- `TIME_SERIES_MODEL`
- `MACHINE_LEARNING_MODEL`
- `SCENARIO_ANALYSIS`
- `HUMAN_ANALYST_FORECAST`
- `HYBRID`
- `OTHER`

### MarketIntelligenceReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `APPROVED_WITH_LIMITATIONS`
- `REJECTED`
- `REVISION_REQUIRED`
- `ESCALATED`
- `EXPIRED`

### MarketIntelligenceReviewOutcome

- `VALIDATED`
- `PARTIALLY_VALIDATED`
- `INSUFFICIENT_EVIDENCE`
- `CONFLICTING_EVIDENCE`
- `OUTDATED`
- `DUPLICATE`
- `UNSUPPORTED_INFERENCE`
- `COMPARABILITY_FAILURE`
- `LICENSE_RESTRICTED`
- `PRIVACY_RESTRICTED`
- `SECURITY_RESTRICTED`
- `POLICY_RESTRICTED`
- `OTHER`

### PublicationStatus

- `NOT_REQUESTED`
- `NOT_READY`
- `REQUESTED`
- `APPROVAL_PENDING`
- `APPROVED`
- `PUBLISHED`
- `WITHDRAWAL_PENDING`
- `WITHDRAWN`
- `EXPIRED`
- `REJECTED`
- `SUPERSEDED`

### RecommendationStatus

- `DRAFT`
- `REVIEW_PENDING`
- `ACTIVE`
- `ACCEPTED_FOR_DECISION`
- `DECLINED`
- `EXPIRED`
- `SUPERSEDED`
- `WITHDRAWN`

### RecommendedActionType

- `NO_ACTION`
- `MONITOR`
- `REFRESH_DATA`
- `REQUEST_HUMAN_ANALYSIS`
- `REVIEW_PRICING`
- `REVIEW_DISCOUNT_POLICY`
- `REVIEW_INVENTORY_LEVELS`
- `REVIEW_INVENTORY_TRANSFER`
- `REVIEW_VEHICLE_SOURCING`
- `REVIEW_ACQUISITION`
- `REVIEW_CAMPAIGN`
- `REVIEW_SALES_PLAYBOOK`
- `REVIEW_CUSTOMER_MATCHING`
- `REVIEW_TRADE_IN_BENCHMARKS`
- `REVIEW_FINANCE_PROGRAMS`
- `REVIEW_REGULATORY_IMPACT`
- `ESCALATE_TO_MANAGEMENT`
- `ESCALATE_TO_LEGAL`
- `ESCALATE_TO_COMPLIANCE`
- `OTHER`

### RecommendationActionClass

- `ACTION_CLASS_0_INFORMATION_ONLY`
- `ACTION_CLASS_1_INTERNAL_ANALYSIS`
- `ACTION_CLASS_2_CONTROLLED_OPERATION`
- `ACTION_CLASS_3_BINDING_OR_HIGH_IMPACT`

### DecisionStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `APPROVED_WITH_CONDITIONS`
- `REJECTED`
- `EXPIRED`
- `REVOKED`
- `SUPERSEDED`

### IngestionStatus

- `NOT_STARTED`
- `QUEUED`
- `PROCESSING`
- `PARTIALLY_COMPLETED`
- `COMPLETED`
- `FAILED`
- `QUARANTINED`
- `RECONCILIATION_REQUIRED`

### ConflictStatus

- `NONE`
- `POTENTIAL`
- `CONFIRMED`
- `UNDER_REVIEW`
- `RESOLVED`
- `ACCEPTED_AS_UNRESOLVED`

### ReconciliationStatus

- `NOT_REQUIRED`
- `CURRENT`
- `PENDING`
- `IN_PROGRESS`
- `RESOLVED`
- `FAILED`
- `MANUAL_REVIEW_REQUIRED`

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_ANALYTICS_PLATFORM_AUTHORITATIVE`
- `EXTERNAL_DATA_PROVIDER_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- All Tenant-owned relationships must belong to the authorized Tenant.
- Dealership, branch, legal entity, team, User, and consumer scope must be validated.
- Cross-Tenant intelligence access, matching, aggregation, AI retrieval, publication, or export is prohibited unless governed by an approved and auditable mechanism.
- Background Jobs, Event Consumers, integrations, and AI Agents must receive trusted Tenant execution context.
- Shared external reference data must be explicitly classified and must not weaken Tenant isolation.

### Intelligence-Creation Rules

Every Market Intelligence record must contain:

- Intelligence series.
- Tenant context.
- Business question or approved monitoring purpose.
- Intelligence type.
- Scope.
- Time period.
- At least one governed observation or evidence package.
- Source provenance.
- Visibility.
- Freshness policy.
- Responsible actor or workflow.
- Record version.
- Audit evidence.

A Market Intelligence record must not be created solely from an unsupported statement.

### Source-Approval Rules

A source may be used only when:

- Source identity is known.
- Source type is classified.
- Source authority is classified.
- Collection method is permitted.
- License or usage rights are valid.
- Security review is satisfied where required.
- Privacy review is satisfied where required.
- Retention requirements are known.
- Geographic and product scope are known.
- Source reliability is assessed.
- No active suspension or revocation exists.

An unapproved source may be stored in quarantine for review.

It must not support published intelligence.

### Collection-Legality Rules

Collection must not use:

- Unauthorized credentials.
- Circumvention of technical controls.
- Prohibited automated access.
- Deceptive impersonation.
- Unlawful personal-data collection.
- Trade-secret acquisition.
- Contractually prohibited reuse.
- Another prohibited method.

Every collection Job must preserve:

- Source.
- Method.
- Permission basis.
- Batch or request reference.
- Retrieval time.
- Original payload reference.
- Original payload hash.
- Deduplication key.
- Collection software or connector version.
- Errors and exclusions.

### Ingestion and Deduplication Rules

- Source ingestion must use source-specific deduplication controls.
- The same source record must not create duplicate observations.
- Provider or source deduplication keys remain separate from ASOS `event_id`.
- Retryable collection Commands must use `idempotency_key`.
- Event Consumers must deduplicate ASOS Events using `event_id`.
- Duplicate evidence must not inflate sample size.
- Duplicate listing records must be identified before pricing and supply calculations.
- Failed ingestion must not delete successfully retained original evidence.
- Partial ingestion must remain explicit.

### Evidence Rules

Every validated intelligence record must preserve:

- Source identifiers.
- Evidence identifiers.
- Source publication or occurrence time.
- Retrieval time.
- Original references.
- Integrity hashes.
- Product scope.
- Geographic scope.
- Time scope.
- Normalization.
- Exclusions.
- Supporting evidence.
- Contradicting evidence.

Evidence must remain distinguishable from interpretation.

Evidence with failed integrity checks must not support publication.

### Observed-Fact Rules

- Observed facts must be directly traceable to evidence.
- Observation summaries must not add unsupported interpretation.
- Estimated or inferred values must be labelled.
- Unknown values must remain unknown.
- Advertised values must remain labelled as advertised.
- Submitted values must remain labelled as submitted.
- Public claims must not be represented as verified internal outcomes.
- One observation must not be generalized into a market-wide trend without applicable validation.

### Time Rules

- `observation_period_start` must not be later than `observation_period_end`.
- `comparison_period_start` must not be later than `comparison_period_end`.
- `forecast_period_start` must not be later than `forecast_period_end`.
- `effective_until`, when populated, must be later than `effective_from`.
- `expires_at` must be later than the applicable analysis or publication time.
- Observation time must not be materially later than source retrieval time without an explained source model.
- Future-dated source records must be quarantined or reviewed.
- Time zones must be explicit.
- Server-side authoritative time must be used for lifecycle evaluation.

### Vehicle and Product Normalization Rules

- Source Vehicle descriptions must preserve their original form.
- Canonical Vehicle matching must use approved normalization.
- Vehicle condition must remain explicit.
- New, used, demo, and damaged Vehicle data must not be combined without controlled normalization.
- Model years must not be combined when the difference is material.
- Trim, drivetrain, fuel type, transmission, and major specifications must be considered when relevant.
- Ambiguous matching requires Human Review or exclusion.
- AI matching must remain Derived Intelligence until accepted by the governed normalization workflow.

### Geographic Rules

- Geographic scope must match the evidence.
- City, region, country, radius, and custom areas must not be treated as equivalent without approved normalization.
- Geographic comparisons must consider applicable:
  - Taxes.
  - Fees.
  - Import costs.
  - Logistics costs.
  - Incentives.
  - Currency.
  - Registration requirements.
  - Market conditions.
- Precise coordinates must not be retained when unnecessary.
- Invalid latitude, longitude, or radius values must be rejected.

### Comparability Rules

Before comparative pricing, supply, demand, or competitor conclusions:

- Product comparability must be assessed.
- Condition comparability must be assessed.
- Geographic comparability must be assessed.
- Time comparability must be assessed.
- Currency comparability must be assessed.
- Tax and fee basis must be assessed.
- Incentive basis must be assessed.
- Source methodology must be assessed.
- Listing status must be assessed.
- New and used markets must be separated where material.
- Material normalization adjustments must be disclosed.

A comparison marked `NOT_COMPARABLE` must not support an unrestricted Recommendation.

### Sampling and Coverage Rules

- Sample definitions must be explicit.
- Population definitions must be explicit.
- Duplicate observations must not count as independent samples.
- Required sample size must remain policy-configurable.
- Required source diversity must remain policy-configurable.
- Missing-data and outlier handling must be documented.
- Small or biased samples must remain clearly limited.
- Coverage limitations must be disclosed.
- A single marketplace must not automatically represent the whole market.
- A high count from one duplicated or related source does not establish source diversity.

### Pricing Rules

- Monetary values must use fixed decimal precision.
- Currency must use ISO 4217.
- Advertised prices must remain separate from transaction prices.
- Cash prices must remain separate from finance-dependent prices.
- Tax-inclusive and tax-exclusive prices must remain distinguishable.
- Optional fees and mandatory fees must remain distinguishable.
- Incentive-dependent prices must preserve eligibility conditions.
- New and used pricing must remain separate where material.
- Damaged and normal-condition Vehicles must remain separate.
- `market_price_min_amount` must not exceed `market_price_max_amount`.
- Average and median values must be calculated from the governed comparable sample.
- Price-gap calculations must preserve formula version.
- Internal price references must come from an authorized internal source.
- Market Intelligence must not directly update internal price.

### Currency-Conversion Rules

Currency conversion must preserve:

- Original amount.
- Original currency.
- Target currency.
- Conversion rate.
- Rate source.
- Rate timestamp.
- Conversion method.
- Rule version.
- Rounding.
- Applicable fees where included.

Stale conversion rates must not support current cross-currency comparison.

### Supply Rules

- Listing count must not be equated with physical Inventory without qualification.
- Duplicate listings must be removed or identified.
- Removed listings must not automatically be treated as sold.
- Listing duration must use a consistent methodology.
- Market supply-days calculations must preserve formula version.
- Internal Inventory count must come from Inventory authority.
- Competitor Inventory remains an observation unless authoritative data exists.
- Scarcity and stock-pressure scores must preserve scale and formula.

### Demand Rules

- Search volume must not be represented as purchase intent.
- Inquiry count must not be represented as Qualified Lead count.
- Lead count must not be represented as Opportunity count.
- Quotation count must not be represented as Deal count.
- Conversion metrics must use consistent definitions and time windows.
- Cancelled, duplicate, test, and invalid records must be handled according to the metric definition.
- Customer-derived metrics must use approved aggregation.
- Customer-level records must not be exposed through Market Intelligence.
- Demand indices must preserve formula, scale, and source versions.

### Internal Aggregate Rules

Internal aggregates must preserve:

- Dataset identifiers.
- Domain-source versions.
- Metric definitions.
- Inclusion rules.
- Exclusion rules.
- Aggregation level.
- Time period.
- Privacy thresholds.
- Suppression rules.
- Calculation version.
- Refresh time.

Internal aggregates must not expose individual Customer, Applicant, or User records.

Small groups must be suppressed or generalized according to policy.

### Competitor-Intelligence Rules

- Competitor data must come from permitted sources.
- Public prices must remain labelled as advertised prices.
- Promotional conditions must be preserved.
- Expired promotions must not be treated as active.
- Competitor strengths and weaknesses must be labelled as analysis.
- Market Intelligence must not facilitate unlawful coordination.
- Market Intelligence must not recommend matching a competitor price for the purpose of price fixing.
- Restricted competitor information must not be exposed to unauthorized Users.
- Competitor identity must not be confused with a canonical Customer or partner record.

### Incentive Rules

- Incentive source must be identified.
- Effective dates must be preserved.
- Vehicle and geographic eligibility must be preserved.
- Customer eligibility must not be assumed.
- Incentive amount and percentage must not be combined incorrectly.
- Expired or withdrawn incentives must not be treated as active.
- An observed incentive must not be added to a Quotation without deterministic eligibility validation.

### Finance-Rate Rules

- Published rates must remain distinguishable from approved Customer-specific rates.
- Nominal rate, APR, effective rate, flat rate, and equivalent measures must not be mixed without normalization.
- Terms, down payment, fees, and eligibility assumptions must be preserved.
- Market rates must not be represented as guaranteed.
- Finance-rate observations must not replace a Lender Decision.
- Lender and program source restrictions must be respected.

### Trade-In Intelligence Rules

- Market benchmark must not become an appraisal.
- Condition, mileage, model year, specification, and geography must be considered.
- Wholesale, auction, retail, and Trade-In values must remain distinct.
- Reconditioning-cost assumptions must be disclosed.
- Valuation confidence thresholds must remain configurable.
- AI valuation must remain a Recommendation.

### Economic-Indicator Rules

- Economic indicators must preserve authoritative source and date.
- Preliminary and final values must remain distinguishable.
- Forecast and actual values must remain distinguishable.
- Correlation must not be represented as causation.
- Economic indicators must not be used to create unsupported Customer-level conclusions.

### Regulatory-Intelligence Rules

- Regulatory source must be authoritative or clearly informational.
- Publication date and effective date must remain separate.
- Draft regulation must remain distinguishable from enacted regulation.
- Regulatory summary must not be presented as legal advice.
- Binding policy changes require legal or compliance review.
- AI must not independently decide regulatory applicability.

### Deterministic Calculation Rules

Authoritative Market Intelligence metrics must be calculated by approved deterministic services.

AI must not generate authoritative numeric market metrics without controlled calculation.

Every calculation must preserve:

- Formula.
- Formula version.
- Input references.
- Input versions.
- Input hash.
- Exclusions.
- Normalization.
- Currency.
- Units.
- Rounding.
- Output.
- Output hash.
- Timestamp.
- Expiration.
- Validation.

### Forecast Rules

- Forecast target must be explicit.
- Forecast horizon must be explicit.
- Training or input period must be explicit.
- Model or method version must be preserved.
- Confidence intervals or scenario ranges should be provided where meaningful.
- Assumptions and limitations must be preserved.
- Forecasts must expire.
- Forecasts must not be presented as guaranteed outcomes.
- Model drift and performance must be monitored where applicable.
- Material forecasts require Human Review according to policy.

### Confidence Rules

- Confidence values must use a documented method.
- Confidence thresholds must remain configurable.
- Confidence must not replace evidence quality.
- High model confidence must not override poor source quality.
- High source reliability must not remove comparability limitations.
- Confidence methods and versions must be preserved.
- Missing confidence must not be replaced with an invented default.
- Customer-facing or binding actions must not depend solely on a confidence score.

### Contradictory-Evidence Rules

- Contradictory evidence must be preserved.
- Contradicting sources must be identified.
- Material conflicts require review.
- One source must not be silently preferred without authority.
- Conflict-resolution method and Decision must be preserved.
- Unresolved conflicts must limit publication and use.
- AI may summarize conflicts but must not conceal them.

### Versioning and Immutability Rules

- `intelligence_version` must increase sequentially within one series.
- Only one current version may exist in a series.
- Published versions are immutable.
- Material evidence changes require a new version.
- Material calculation changes require a new version.
- Material analysis changes require a new version.
- Material Recommendation changes require a new version.
- Supersession must preserve the original version.
- Circular supersession is prohibited.
- Retryable version creation must not create duplicate versions.

### Validation Rules

A Market Intelligence version may become `VALIDATED` only when:

- Required sources are approved.
- Collection is permitted.
- Required evidence exists.
- Evidence integrity passes.
- Provenance is complete.
- Product and geographic scope are sufficiently resolved.
- Comparability requirements pass or limitations are accepted.
- Sampling and coverage requirements pass or limitations are accepted.
- Deterministic calculations pass.
- Privacy and security controls pass.
- Material conflicts are resolved or disclosed.
- No blocking legal or licensing issue exists.

### Review Rules

Human Review is required according to policy for:

- Low-quality or disputed evidence.
- Conflicting sources.
- Ambiguous Vehicle matching.
- Small or biased samples.
- High-impact pricing Recommendation.
- Inventory acquisition Recommendation.
- Campaign-spend Recommendation.
- Regulatory intelligence.
- Finance-rate interpretation.
- Trade-In benchmark affecting appraisal policy.
- Cross-border comparison.
- Restricted source.
- AI-generated high-impact analysis.
- Another material legal, commercial, privacy, or security risk.

### Publication Rules

Publication requires:

- Validated evidence.
- Completed analysis.
- Current freshness state.
- Completed required reviews.
- Approved visibility.
- Approved audience.
- No blocking source-license restriction.
- No blocking privacy restriction.
- No blocking security restriction.
- Publication snapshot and hash.
- Publication authority.
- Expiration or refresh policy.

Publication must not imply that any Recommendation was approved for execution.

### Stale and Expired Rules

- Freshness must be evaluated deterministically.
- Stale intelligence must be labelled.
- Expired intelligence must not support unrestricted current Recommendations.
- Consumers must receive freshness status.
- A stale record may remain available for historical analysis.
- Refresh must create new evidence or a new validated version where material.
- A User must not manually remove stale state without a governed refresh or Decision.

### Recommendation Rules

Every Recommendation must preserve:

- Intelligence version.
- Evidence.
- Assumptions.
- Limitations.
- Scope.
- Owner role.
- Priority.
- Expiration.
- Action Class.
- Required Human authority.

A Recommendation does not authorize:

- Price change.
- Discount.
- Inventory movement.
- Vehicle sourcing.
- Campaign launch.
- Customer contact.
- Quotation change.
- Trade-In appraisal.
- Deal change.

### Downstream-Decision Rules

A downstream Decision must:

- Reference the exact Market Intelligence version.
- Preserve the Decision-maker.
- Preserve scope.
- Preserve reason.
- Preserve conditions.
- Preserve effective period.
- Preserve evidence.
- Remain separate from the Recommendation.

A downstream operational action must use the responsible Domain Service.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Intelligence creation must support idempotency.
- Ingestion requests must support idempotency.
- Version creation must support idempotency.
- Calculation requests must support idempotency.
- Publication requests must support idempotency.
- Withdrawal requests must support idempotency.
- Retryable Commands must use `idempotency_key`.
- Event Consumers must prevent duplicate effects using `event_id`.
- Source ingestion must use source-specific deduplication identifiers.
- Duplicate retries must not create duplicate:
  - Intelligence records.
  - Intelligence versions.
  - Source records.
  - Observations.
  - Evidence records.
  - Calculations.
  - Forecasts.
  - Recommendations.
  - Reviews.
  - Publications.

---

## 6. State Machine

### Allowed States

```text
DRAFT
INGESTION_PENDING
COLLECTED
VALIDATION_PENDING
VALIDATED
ANALYSIS_PENDING
ANALYZED
REVIEW_PENDING
APPROVED
PUBLISHED
STALE
REJECTED
SUPERSEDED
ARCHIVED
```

### Principal Allowed Transitions

```text
DRAFT → INGESTION_PENDING
DRAFT → COLLECTED
DRAFT → REJECTED

INGESTION_PENDING → COLLECTED
INGESTION_PENDING → DRAFT
INGESTION_PENDING → REJECTED

COLLECTED → VALIDATION_PENDING
COLLECTED → DRAFT
COLLECTED → REJECTED

VALIDATION_PENDING → VALIDATED
VALIDATION_PENDING → COLLECTED
VALIDATION_PENDING → REJECTED

VALIDATED → ANALYSIS_PENDING
VALIDATED → ANALYZED
VALIDATED → REJECTED

ANALYSIS_PENDING → ANALYZED
ANALYSIS_PENDING → VALIDATED
ANALYSIS_PENDING → REJECTED

ANALYZED → REVIEW_PENDING
ANALYZED → APPROVED
ANALYZED → VALIDATED
ANALYZED → REJECTED

REVIEW_PENDING → APPROVED
REVIEW_PENDING → ANALYZED
REVIEW_PENDING → REJECTED

APPROVED → PUBLISHED
APPROVED → ANALYZED
APPROVED → REJECTED

PUBLISHED → STALE
PUBLISHED → SUPERSEDED
PUBLISHED → ARCHIVED

STALE → SUPERSEDED
STALE → ARCHIVED

REJECTED → ARCHIVED
SUPERSEDED → ARCHIVED
```

A refreshed analysis normally creates a new version rather than returning the published version to an editable state.

### Forbidden Ordinary Transitions

```text
DRAFT → VALIDATED
DRAFT → ANALYZED
DRAFT → PUBLISHED

INGESTION_PENDING → APPROVED
COLLECTED → PUBLISHED
VALIDATION_PENDING → PUBLISHED

VALIDATED → PUBLISHED
ANALYSIS_PENDING → PUBLISHED
ANALYZED → PUBLISHED

REJECTED → APPROVED
REJECTED → PUBLISHED

PUBLISHED → DRAFT
PUBLISHED → COLLECTED
PUBLISHED → VALIDATION_PENDING
PUBLISHED → ANALYSIS_PENDING

STALE → PUBLISHED

SUPERSEDED → PUBLISHED

ARCHIVED → DRAFT
ARCHIVED → PUBLISHED
```

Corrections to a published version require a new version, withdrawal, supersession, or governed correction Event.

### Entering DRAFT

Requires:

- Valid Tenant context.
- Intelligence series.
- Business question or approved monitoring purpose.
- Scope.
- Intelligence type.
- Responsible actor.
- Initial audit evidence.
- Idempotency protection.

### Entering INGESTION_PENDING

Requires:

- Approved source or quarantined-source review path.
- Collection method.
- Source permission.
- Ingestion request.
- Batch or request reference.
- Idempotency key.
- Security controls.

### Entering COLLECTED

Requires:

- Evidence received.
- Original references.
- Evidence hashes.
- Source timestamps.
- Deduplication.
- Tenant routing.
- Security scan.
- Collection completion or accepted partial state.

### Entering VALIDATION_PENDING

Requires:

- Evidence package.
- Source snapshot.
- Scope snapshot.
- Comparability evaluation.
- Sampling and coverage evaluation.
- Validation policy.
- Responsible validation workflow.

### Entering VALIDATED

Requires:

- Approved sources.
- Valid evidence integrity.
- Complete provenance.
- Sufficient permitted evidence.
- Successful deterministic calculations where required.
- Accepted comparability.
- Accepted coverage.
- Privacy and security clearance.
- No blocking license or legal issue.
- Validation snapshot and hash.

### Entering ANALYSIS_PENDING

Requires:

- Validated evidence.
- Defined analysis method.
- Analysis request.
- Input snapshot.
- Responsible analyst or approved analytical service.

### Entering ANALYZED

Requires:

- Completed analysis.
- Preserved methods and versions.
- Preserved assumptions.
- Preserved limitations.
- Preserved conflicting evidence.
- Risk and opportunity assessment where applicable.
- Recommendation separation.
- Analysis snapshot and hash.

### Entering REVIEW_PENDING

Requires:

- Identified review requirement.
- Frozen review snapshot.
- Assigned authorized reviewers.
- Review reason.
- Evidence and analysis package.

### Entering APPROVED

Requires:

- Completed required reviews.
- Approved evidence and analysis.
- Current freshness.
- Approved limitations.
- Approved audience.
- No blocking conflict.
- Approval Decision and evidence.

### Entering PUBLISHED

Requires:

- Approved version.
- Publication Decision.
- Publication audience.
- Publication scope.
- Publication artifact and hash.
- Current freshness.
- Expiration or refresh date.
- No blocking license, privacy, or security restriction.
- `published_at`.

### Entering STALE

Requires:

- Freshness policy determines that unrestricted current use is no longer permitted.
- Staleness reason.
- Evaluation timestamp.
- Consumer notification where applicable.

### Entering REJECTED

Requires:

- Rejection reason.
- Rejection authority.
- Evidence.
- Treatment of related Draft Recommendations.
- Audit history.

### Entering SUPERSEDED

Requires:

- Valid newer intelligence version.
- Same intelligence series.
- Atomic update of the current-version reference.
- Supersession reason.
- Replacement reference.

### Terminal States

For ordinary processing:

- `REJECTED`
- `SUPERSEDED`
- `ARCHIVED`

`PUBLISHED` and `STALE` remain immutable analytical records.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Intelligence series.
- Intelligence version.
- Evidence snapshot hash.
- Calculation snapshot hash.
- Analysis snapshot hash.
- Review Decision.
- Publication Decision.
- Record version.
- Applied Business Rules.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.
- Related Command.
- Related External Confirmation.

---

## 7. Relationships

### Tenant

- Every Market Intelligence record belongs to exactly one `tenant_id`.
- Tenant-owned relationships must remain within authorized Tenant scope.
- Shared market reference data requires explicit shared-reference governance.
- One Tenant must not access another Tenant’s internal aggregate intelligence.

### Intelligence Series

- One intelligence series may contain multiple immutable versions.
- Only one current version may exist.
- Historical versions remain linked.
- Circular supersession is prohibited.

### Source Registry

Every external or internal source must reference a governed Source Registry entry containing:

- Source identity.
- Ownership.
- Source type.
- Authority type.
- License.
- Usage restrictions.
- Security classification.
- Approved collection methods.
- Retention.
- Status.

### Market Observation

One Market Intelligence version may contain or reference multiple Market Observations.

Each observation must preserve:

- Source.
- Evidence.
- Scope.
- Time.
- Value.
- Unit.
- Normalization.
- Verification state.

### Evidence Package

One version must reference one governed publication evidence package.

The package may include:

- Supporting evidence.
- Contradicting evidence.
- Excluded evidence.
- Raw references.
- Normalized references.
- Hashes.
- Security classification.

### Vehicle

Market Intelligence may reference:

- Vehicle.
- Vehicle Model.
- Vehicle configuration.
- Vehicle segment.

Vehicle Domain Service owns canonical identity.

### Inventory Record

Market Intelligence may consume authorized aggregate Inventory projections.

It must not directly update Inventory Records.

Inventory decisions must reference the exact intelligence version where applicable.

### Lead and Qualified Lead

Market Intelligence may consume aggregate:

- Lead volume.
- Source trends.
- Qualification rates.
- Vehicle-interest trends.

Individual Lead and Qualified Lead records must not be exposed unnecessarily.

### Opportunity

Market Intelligence may consume aggregate Opportunity metrics.

It must not alter Opportunity state.

### Quotation

Market Intelligence may consume aggregate Quotation metrics and may support pricing review.

It must not alter a Quotation.

### Trade-In

Market Intelligence may provide market benchmarks to Trade-In workflows.

Trade-In Domain Service remains authoritative for appraisal and acquisition.

### Finance Application

Market Intelligence may provide general finance-market context.

Finance Application and Lender remain authoritative for Customer-specific finance outcomes.

### Deal

Market Intelligence may consume aggregate Deal and conversion outcomes.

It must not complete, cancel, or modify a Deal.

### Interaction

Market Intelligence may consume aggregated Interaction trends where permitted.

Original Interaction content remains governed by Interaction Domain Service.

### Campaign

Market Intelligence may recommend Campaign review.

Campaign execution remains a separate controlled workflow.

### Pricing and Policy

Market Intelligence may support:

- Pricing review.
- Discount-policy review.
- Incentive-policy review.
- Acquisition-policy review.
- Inventory-policy review.

Policy changes require their own governed version and approval.

### Human Decision

Recommendations may reference one or more authoritative Human Decisions.

The Decision remains a separate record.

### Command

A Decision derived from Market Intelligence may create a Command in another Domain workflow.

The Market Intelligence record must not execute the Command directly.

### External Confirmation

External action completion may be associated with the originating intelligence through:

- Decision.
- Command.
- Confirmation.
- Result measurement.

The External Confirmation remains authoritative to the responsible external system.

### AI Agent Run

AI Agent Runs may reference:

- Intelligence version.
- Evidence package.
- Input records.
- Model.
- Prompt.
- Output.
- Recommendation.
- Review.

AI output remains Derived Intelligence.

### Supporting Child Records

Market Intelligence may own or govern:

- Intelligence versions.
- Source relationships.
- Ingestion batches.
- Observations.
- Evidence packages.
- Normalization records.
- Comparability evaluations.
- Sample and coverage records.
- Calculation records.
- Trend analyses.
- Forecasts.
- Risk assessments.
- Opportunity assessments.
- Recommendations.
- Review requests.
- Review Decisions.
- Publication records.
- Consumption records.
- Outcome measurements.
- Conflicts.
- Data-quality issues.
- Reconciliation cases.
- Audit history.

---

## 8. Domain Events

The Canonical Event Catalog is the authoritative source for final:

- Event names.
- Event versions.
- Event envelopes.
- Payload Schemas.
- Producers.
- Consumers.
- Compatibility rules.
- Correction and reversal behaviour.

The following are required Market Intelligence Event concepts and do not replace the Event Catalog.

### Series and Version Event Concepts

- Market Intelligence series created.
- Market Intelligence Draft created.
- Market Intelligence Draft updated.
- Market Intelligence version created.
- Market Intelligence version superseded.
- Market Intelligence archived.

### Source and Ingestion Event Concepts

- Market source proposed.
- Market source approved.
- Market source suspended.
- Market source revoked.
- Market ingestion requested.
- Market ingestion started.
- Market ingestion completed.
- Market ingestion partially completed.
- Market ingestion failed.
- Market source duplicate detected.
- Market source record quarantined.
- Market ingestion reconciliation required.

### Evidence Event Concepts

- Market evidence recorded.
- Market evidence package created.
- Market evidence integrity validated.
- Market evidence integrity failed.
- Market evidence conflict detected.
- Market evidence excluded.
- Market evidence package frozen.

### Observation Event Concepts

- Market Observation recorded.
- Market Observation normalized.
- Market Observation validation requested.
- Market Observation validated.
- Market Observation rejected.
- Market Observation corrected.
- Market Observation superseded.

### Calculation Event Concepts

- Market calculation requested.
- Market calculation completed.
- Market calculation validated.
- Market calculation failed.
- Market calculation expired.
- Market calculation reconciliation required.

### Analysis Event Concepts

- Market analysis requested.
- Market trend analysis completed.
- Market forecast generated.
- Market risk assessment completed.
- Market opportunity assessment completed.
- Market analysis conflict detected.
- Market analysis completed.

### Recommendation Event Concepts

- Market Recommendation created.
- Market Recommendation reviewed.
- Market Recommendation activated.
- Market Recommendation declined.
- Market Recommendation expired.
- Market Recommendation superseded.
- Market Human Decision requested.

### Review and Publication Event Concepts

- Market Intelligence validation requested.
- Market Intelligence validated.
- Market Intelligence review requested.
- Market Intelligence approved.
- Market Intelligence rejected.
- Market Intelligence publication requested.
- Market Intelligence published.
- Market Intelligence publication withdrawn.
- Market Intelligence became stale.
- Market Intelligence expired.

### Consumption Event Concepts

- Market Intelligence consumed.
- Market Intelligence linked to Human Decision.
- Market Intelligence linked to downstream Command.
- Market Intelligence linked to External Confirmation.
- Market Intelligence outcome measurement requested.
- Market Intelligence outcome measured.

### Regulatory and Compliance Event Concepts

- Regulatory intelligence detected.
- Regulatory intelligence legal review requested.
- Regulatory intelligence compliance review requested.
- Market source license review requested.
- Market privacy review requested.
- Market security review requested.

### AI Event Concepts

- Market AI analysis requested.
- Market AI analysis completed.
- Market AI analysis failed.
- Market AI forecast generated.
- Market AI Recommendation generated.
- Market AI Human Review recommended.

AI Events must not imply:

- Source validation.
- Legal permission.
- Market fact.
- Price approval.
- Inventory Decision.
- Campaign approval.
- Finance commitment.
- Quotation change.
- Trade-In appraisal.
- Deal change.
- Human Approval.
- External action completion.

### Producer Rules

- Market Intelligence Domain Service publishes accepted intelligence and workflow-state changes.
- Source integrations publish normalized source-ingestion observations.
- Calculation and Analytics Services publish accepted calculation outcomes.
- Vehicle, Inventory, Lead, Opportunity, Quotation, Trade-In, Finance Application, Deal, and Interaction Domain Services publish their authoritative internal facts.
- AI Agents may publish Agent-run, analysis, forecast, anomaly, and Recommendation Events.
- AI Agents must not publish authoritative source-validation, pricing, Inventory, Campaign, finance, or commercial-action Events merely because they generated a Recommendation.

### Event Requirements

Every material Market Intelligence Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `market_intelligence_id`.
- `intelligence_series_id`.
- Intelligence version.
- Intelligence type.
- Scope.
- Source references.
- Evidence-package reference.
- Evidence hash.
- Calculation references.
- Analysis snapshot reference.
- Recommendation reference.
- Review Decision.
- Publication Decision.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Applied policy.
- Human Decision.
- Command.
- External Confirmation.
- Security classification.

Events are immutable.

Corrections, supersession, withdrawal, staleness, source revocation, and outcome correction must use new Events linked to prior Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate ASOS Event effects using `event_id`.

Source-record deduplication must use source-specific identifiers and remains a separate concern.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Source-document classification.
- Vehicle-description normalization.
- Geographic normalization.
- Data-quality analysis.
- Duplicate-listing detection.
- Outlier detection.
- Trend detection.
- Pattern discovery.
- Competitor-offer summarization.
- Regulatory-publication summarization.
- Economic-indicator summarization.
- Demand analysis.
- Supply analysis.
- Pricing analysis.
- Trade-In market analysis.
- Forecast generation.
- Scenario analysis.
- Risk assessment.
- Opportunity assessment.
- Recommendation drafting.
- Contradicting-evidence summarization.
- Executive-summary drafting.
- Human Review preparation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Approve a market source.
- Determine that data collection is lawful.
- Bypass source-license restrictions.
- Treat generated content as original source evidence.
- Alter original evidence.
- Fabricate market observations.
- Invent prices, rates, listings, incentives, or regulatory facts.
- Publish Market Intelligence without required authority.
- Approve a price change.
- Approve a discount.
- Approve Inventory acquisition.
- Reserve or allocate a Vehicle.
- Launch a Campaign.
- Contact Customers.
- Approve a Trade-In appraisal.
- Set Customer-specific finance terms.
- Modify a Quotation.
- Modify a Deal.
- Execute external Commands directly.
- Access another Tenant’s internal market data.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Market Intelligence identifier.
- Intelligence version.
- Input evidence references.
- Input-record versions.
- Input snapshot hash.
- Model reference.
- Model version.
- Prompt version where applicable.
- Feature or context snapshot.
- Confidence where meaningful.
- Evidence grounding.
- Alternative hypotheses.
- Assumptions.
- Limitations.
- Data freshness.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority.

### Source and Fact Grounding

AI must distinguish:

- Source quotation.
- Source fact.
- Normalized observation.
- Deterministic calculation.
- Interpretation.
- Forecast.
- Recommendation.

AI-generated text must not be stored as source evidence unless it is explicitly an AI output record.

AI summaries must preserve the underlying source references.

### Vehicle Matching

AI may propose mappings between source descriptions and canonical Vehicle configurations.

The mapping must preserve:

- Original description.
- Candidate Vehicle.
- Match rationale.
- Differences.
- Confidence where meaningful.
- Normalization version.
- Review requirement.

AI must not silently merge materially different:

- Model years.
- Trims.
- Conditions.
- Powertrains.
- Body styles.
- Specifications.

### Competitor Analysis

AI may summarize lawful competitor information.

It must not:

- Recommend unlawful coordination.
- Recommend price fixing.
- Recommend market allocation.
- Invent competitor transaction prices.
- Infer confidential competitor information without evidence.
- Expose source data beyond license restrictions.

### Forecasting

AI forecasts must preserve:

- Target.
- Horizon.
- Input period.
- Model.
- Assumptions.
- Confidence interval or scenarios where meaningful.
- Limitations.
- Expiration.
- Historical performance where governed.

AI forecasts must not be presented as guaranteed outcomes.

### Regulatory Analysis

AI may summarize published regulatory material.

AI must not:

- Provide authoritative legal interpretation.
- Decide applicability.
- Change Business Rules.
- Remove legal-review requirements.
- Represent a draft rule as enacted law.

Material regulatory intelligence requires authorized legal or compliance review.

### Internal Aggregate Analysis

AI access to internal data must be:

- Tenant-scoped.
- Purpose-limited.
- Aggregated where possible.
- Field-restricted.
- Logged.
- Approved.
- Prevented from exposing individual Customer records.
- Prevented from cross-Tenant training or inference without explicit governance.

### Recommendations

AI Recommendations must:

- Reference exact intelligence version.
- Reference evidence.
- Identify assumptions.
- Identify limitations.
- Identify affected Domain workflows.
- Identify Action Class.
- Identify required Human authority.
- Expire.
- Remain non-binding.

### Action Class 2

Controlled internal tasks such as:

- Requesting a data refresh.
- Requesting analyst review.
- Creating an internal monitoring task.
- Delivering an approved internal intelligence notification.

may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Intelligence status.
- Freshness.
- Visibility.
- Audience.
- Source restrictions.
- Data classification.
- Notification purpose.
- Template.
- Frequency.
- Human Review requirements.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision and the responsible Domain workflow.

Examples include:

- Price change.
- Discount-policy change.
- Vehicle acquisition.
- Inventory transfer.
- Campaign expenditure.
- Customer targeting.
- Trade-In policy change.
- Finance-program change.
- Regulatory-policy change.
- Quotation change.
- Deal change.

### AI Context and Embeddings

Market Intelligence data must not enter unrestricted embeddings.

Normally excluded or restricted fields include:

- Licensed source content beyond permitted use.
- Full paid reports.
- Restricted competitor data.
- Customer-level internal data.
- Customer identifiers.
- User identifiers.
- Finance records.
- Payment records.
- Contract documents.
- Internal cost and margin data.
- Confidential pricing strategies.
- Security credentials.
- Legal-review notes.
- Restricted regulatory advice.

Approved context may include:

- Published intelligence summaries.
- Redacted observations.
- Approved metrics.
- Approved Recommendations.
- Public evidence snippets within permitted usage.
- Non-sensitive market segments.
- Freshness metadata.
- Source references.

Every vector record must enforce:

- `tenant_id`.
- Publication and visibility scope.
- Source-license restrictions.
- Source references.
- Intelligence version.
- Freshness.
- Security classification.
- Retention.
- Expiration.
- Withdrawal and deletion propagation.

### Prompt Injection and Untrusted Content

External reports, websites, files, APIs, marketplace descriptions, competitor pages, and documents are untrusted input.

AI Agents must treat them as data, not instructions.

Source content must not:

- Change system policy.
- Grant permissions.
- Override Tenant scope.
- Reveal secrets.
- Trigger external Commands.
- Change publication state.
- Change pricing.
- Change Inventory.
- Change Customer communication.
- Modify audit records.
- Request hidden system instructions.

### Explainability

Material AI intelligence must explain:

- Evidence used.
- Source authority.
- Scope.
- Time period.
- Normalization.
- Sample.
- Coverage.
- Comparability.
- Calculation method.
- Data freshness.
- Contradictory evidence.
- Assumptions.
- Limitations.
- Confidence where meaningful.
- Alternative hypotheses.
- Required Human authority.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Market Intelligence API behaviour.

### REST Resources

```text
GET    /api/v1/market-intelligence
POST   /api/v1/market-intelligence
GET    /api/v1/market-intelligence/{market_intelligence_id}
PATCH  /api/v1/market-intelligence/{market_intelligence_id}

GET    /api/v1/market-intelligence-series/{intelligence_series_id}
GET    /api/v1/market-intelligence-series/{intelligence_series_id}/versions
POST   /api/v1/market-intelligence/{market_intelligence_id}/version-requests

POST   /api/v1/market-intelligence/{market_intelligence_id}/ingestion-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/evidence-submissions
POST   /api/v1/market-intelligence/{market_intelligence_id}/validation-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/calculation-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/analysis-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/forecast-requests

POST   /api/v1/market-intelligence/{market_intelligence_id}/review-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/review-decisions
POST   /api/v1/market-intelligence/{market_intelligence_id}/recommendation-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/publication-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/publication-withdrawal-requests

POST   /api/v1/market-intelligence/{market_intelligence_id}/refresh-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/conflict-review-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/correction-requests
POST   /api/v1/market-intelligence/{market_intelligence_id}/archive-requests

POST   /api/v1/market-intelligence/{market_intelligence_id}/consumption-records
POST   /api/v1/market-intelligence/{market_intelligence_id}/outcome-measurement-requests

GET    /api/v1/market-intelligence/{market_intelligence_id}/sources
GET    /api/v1/market-intelligence/{market_intelligence_id}/evidence
GET    /api/v1/market-intelligence/{market_intelligence_id}/observations
GET    /api/v1/market-intelligence/{market_intelligence_id}/calculations
GET    /api/v1/market-intelligence/{market_intelligence_id}/analysis
GET    /api/v1/market-intelligence/{market_intelligence_id}/recommendations
GET    /api/v1/market-intelligence/{market_intelligence_id}/consumption
GET    /api/v1/market-intelligence/{market_intelligence_id}/history
GET    /api/v1/market-intelligence/{market_intelligence_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, legal entity, team, User, audience, and source scope must be validated.
- Cross-Tenant queries must be blocked by default.
- Shared public reference data must use explicit shared-reference authorization.

### Example Create Request

```json
{
  "title": "Compact SUV advertised-price and supply trend in Greater Cairo",
  "intelligence_type": "PRICING_TREND",
  "scope": "REGION",
  "priority": "STANDARD",
  "visibility": "PRICING_TEAM",
  "time_horizon": "SHORT_TERM",
  "business_question": "How have comparable compact SUV advertised prices and listing supply changed during the latest observation period?",
  "geographic_scope": {
    "country_codes": ["EG"],
    "market_region_id": "c71e2a69-3eb0-4135-8d6b-cfe47681e94d"
  },
  "vehicle_scope": {
    "product_category": "COMPACT_SUV",
    "model_year_from": 2025,
    "model_year_to": 2026,
    "vehicle_condition": "NEW"
  },
  "time_scope": {
    "observation_period_start": "2026-07-01T00:00:00Z",
    "observation_period_end": "2026-07-31T23:59:59Z"
  }
}
```

The request must include:

```text
Idempotency-Key: fde629d2-fc60-4eb9-842c-36b3f41de95f
```

### Example Create Response

```json
{
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "intelligence_series_id": "c625e47d-d9d4-4d62-af92-2cdd67894a80",
  "intelligence_number": "MI-2026-000184",
  "intelligence_version": 1,
  "status": "DRAFT",
  "intelligence_type": "PRICING_TREND",
  "scope": "REGION",
  "data_quality_status": "NOT_ASSESSED",
  "freshness_status": "NOT_EVALUATED",
  "review_status": "NOT_REQUIRED",
  "publication_status": "NOT_REQUESTED",
  "record_version": 1,
  "created_at": "2026-08-01T20:45:00Z"
}
```

### Example Ingestion Request

```json
{
  "source_ids": [
    "f9baaf64-fe53-45da-ac3b-00f3d9cf6a59",
    "092d1536-546d-4661-ae02-e19a17c14131"
  ],
  "collection_method": "APPROVED_API_INTEGRATION",
  "observation_period_start": "2026-07-01T00:00:00Z",
  "observation_period_end": "2026-07-31T23:59:59Z",
  "expected_record_version": 2
}
```

The request must use an idempotency key.

A pending response may be:

```json
{
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "status": "INGESTION_PENDING",
  "ingestion_status": "QUEUED",
  "collection_job_id": "a833877d-f0ef-4690-90a5-38ddd3fa8e32",
  "command_id": "39585ea0-e867-4c33-b6f3-fba9d83c1a69",
  "record_version": 3
}
```

The API must not claim that evidence was collected until ingestion completes.

### Example Collected Response

```json
{
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "status": "COLLECTED",
  "ingestion_status": "COMPLETED",
  "source_record_count": 612,
  "accepted_record_count": 548,
  "duplicate_record_count": 52,
  "rejected_record_count": 12,
  "evidence_count": 548,
  "source_count": 2,
  "evidence_hash": "sha256:c46cf94d...",
  "record_version": 5
}
```

### Example Validation Request

```json
{
  "validation_scope": [
    "SOURCE_LICENSE",
    "EVIDENCE_INTEGRITY",
    "VEHICLE_NORMALIZATION",
    "GEOGRAPHIC_SCOPE",
    "COMPARABILITY",
    "SAMPLING",
    "CURRENCY_NORMALIZATION",
    "PRIVACY",
    "SECURITY"
  ],
  "expected_evidence_hash": "sha256:c46cf94d...",
  "expected_record_version": 5
}
```

### Example Validation Response

```json
{
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "status": "VALIDATED",
  "data_quality_status": "ACCEPTABLE",
  "comparability_status": "COMPARABLE_WITH_ADJUSTMENTS",
  "coverage_status": "ACCEPTABLE",
  "source_license_status": "PERMITTED",
  "evidence_integrity_status": "VALID",
  "evidence_consistency_status": "MOSTLY_CONSISTENT",
  "validation_snapshot_hash": "sha256:86cb2f2a...",
  "record_version": 8
}
```

### Example Analysis Request

```json
{
  "analysis_methods": [
    "PRICE_DISTRIBUTION",
    "PERIOD_COMPARISON",
    "SUPPLY_TREND",
    "OUTLIER_REVIEW"
  ],
  "normalized_currency_code": "EGP",
  "expected_record_version": 8
}
```

### Example Analyzed Response

```json
{
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "status": "ANALYZED",
  "market_price_min_amount": 1980000,
  "market_price_average_amount": 2174000,
  "market_price_median_amount": 2140000,
  "market_price_max_amount": 2490000,
  "market_listing_count": 548,
  "average_days_on_market": 24.6,
  "trend_direction": "INCREASING",
  "impact_level": "MODERATE",
  "analysis_snapshot_hash": "sha256:4bdc311a...",
  "freshness_status": "CURRENT",
  "record_version": 11
}
```

### Example Recommendation Response

```json
{
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "recommendation_id": "11f4a149-d09d-41af-af18-9da78dd816c4",
  "recommended_action_type": "REVIEW_PRICING",
  "recommended_action": "Review current compact SUV pricing positions against the validated comparable-market benchmark.",
  "recommendation_action_class": "ACTION_CLASS_3_BINDING_OR_HIGH_IMPACT",
  "recommendation_requires_human_decision": true,
  "recommendation_status": "REVIEW_PENDING",
  "recommendation_expires_at": "2026-08-08T20:45:00Z",
  "record_version": 12
}
```

The response must not describe the pricing change as approved.

### Example Publication Request

```json
{
  "publication_audience": [
    "PRICING_TEAM",
    "INVENTORY_TEAM",
    "MANAGEMENT"
  ],
  "publication_decision_id": "b8454b51-bc14-4630-bf5e-263f43577481",
  "expected_analysis_snapshot_hash": "sha256:4bdc311a...",
  "expected_record_version": 14
}
```

The request must use an idempotency key.

### Example Published Response

```json
{
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "intelligence_series_id": "c625e47d-d9d4-4d62-af92-2cdd67894a80",
  "intelligence_version": 1,
  "status": "PUBLISHED",
  "publication_status": "PUBLISHED",
  "published_at": "2026-08-01T21:30:00Z",
  "expires_at": "2026-08-08T21:30:00Z",
  "publication_artifact_hash": "sha256:97b2be31...",
  "record_version": 15
}
```

### Example Consumption Record

```json
{
  "consumer_type": "PRICING_REVIEW_WORKFLOW",
  "consumer_reference": "pricing-review://requests/PR-2026-00821",
  "purpose": "REVIEW_PRICING",
  "consumed_intelligence_version": 1,
  "consumed_freshness_status": "CURRENT",
  "resulting_decision_id": "46bc4173-fd4c-4d64-b1b5-d734fc030b6f"
}
```

The consumption record must not imply that a price change was executed.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Intelligence-version validation.
- Source approval.
- Source-license controls.
- Evidence integrity.
- Privacy controls.
- Security controls.
- Lifecycle validation.
- Deterministic calculations.
- Required Human Review.
- Required publication Decision.
- Idempotency where applicable.
- Audit recording.
- Event publication after accepted state change.
- External Confirmation tracking where applicable.

### Optimistic Concurrency

Updates must use an approved mechanism such as:

```text
If-Match: <record_version>
```

A stale version must return a conflict response.

### Idempotency

Retryable operations must support:

```text
Idempotency-Key
```

The same key and request intent must not create duplicate:

- Intelligence records.
- Intelligence versions.
- Ingestion Jobs.
- Evidence records.
- Calculations.
- Forecasts.
- Recommendations.
- Reviews.
- Publications.
- Withdrawal requests.
- Consumption records.

### Pending External Confirmation

Operations requiring an external provider or platform may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "market_intelligence_id": "2b816ea4-c2d1-4828-b3ea-135db0c09d38",
  "command_id": "39585ea0-e867-4c33-b6f3-fba9d83c1a69",
  "record_version": 3
}
```

The API must not describe the external operation as completed until authoritative evidence exists.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `INTELLIGENCE_VERSION_CONFLICT`
- `SOURCE_REQUIRED`
- `SOURCE_NOT_APPROVED`
- `SOURCE_SUSPENDED`
- `SOURCE_LICENSE_NOT_PERMITTED`
- `COLLECTION_METHOD_NOT_PERMITTED`
- `DUPLICATE_SOURCE_RECORD`
- `EVIDENCE_REQUIRED`
- `EVIDENCE_INTEGRITY_FAILED`
- `EVIDENCE_INSUFFICIENT`
- `EVIDENCE_CONFLICT`
- `VEHICLE_NORMALIZATION_REQUIRED`
- `GEOGRAPHIC_SCOPE_INVALID`
- `COMPARABILITY_FAILED`
- `SAMPLE_INSUFFICIENT`
- `COVERAGE_INSUFFICIENT`
- `CURRENCY_NORMALIZATION_FAILED`
- `CALCULATION_FAILED`
- `CALCULATION_MISMATCH`
- `PRIVACY_THRESHOLD_NOT_MET`
- `RESTRICTED_DATA_DETECTED`
- `LEGAL_REVIEW_REQUIRED`
- `COMPLIANCE_REVIEW_REQUIRED`
- `HUMAN_REVIEW_REQUIRED`
- `PUBLICATION_NOT_READY`
- `PUBLICATION_NOT_AUTHORIZED`
- `INTELLIGENCE_STALE`
- `INTELLIGENCE_EXPIRED`
- `RECOMMENDATION_EXPIRED`
- `INVALID_LIFECYCLE_TRANSITION`
- `PUBLISHED_VERSION_IMMUTABLE`
- `EXTERNAL_CONFIRMATION_PENDING`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Visibility.
- Source licensing.
- Evidence integrity.
- Privacy restrictions.
- Field authority.
- Intelligence-version immutability.
- Calculation controls.
- Concurrency.
- Idempotency.
- Human Review.
- Publication authority.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Market Intelligence Domain Service, Source Registry, Policy Engine, Calculation Services, or publication controls.

---

## 11. Database Design

### Recommended Tables

```text
market_intelligence_series
market_intelligence
market_intelligence_versions
market_sources
market_source_licenses
market_source_permissions
market_ingestion_jobs
market_ingestion_batches
market_source_records
market_observations
market_evidence_packages
market_evidence_items
market_vehicle_scope
market_geographic_scope
market_time_scope
market_normalization_records
market_comparability_evaluations
market_sampling_records
market_coverage_records
market_calculations
market_pricing_metrics
market_supply_metrics
market_demand_metrics
market_competitor_observations
market_incentive_observations
market_finance_rate_observations
market_trade_in_metrics
market_economic_observations
market_regulatory_observations
market_internal_aggregate_metrics
market_trend_analyses
market_forecasts
market_risk_assessments
market_opportunity_assessments
market_analyst_interpretations
market_ai_analyses
market_recommendations
market_decision_references
market_review_requests
market_review_decisions
market_publications
market_consumption_records
market_outcome_measurements
market_conflicts
market_external_references
market_external_confirmations
market_reconciliation_cases
market_data_quality_issues
market_status_history
market_record_versions
market_audit_log
```

### Market Intelligence Series Table

`market_intelligence_series` should contain:

- `intelligence_series_id`.
- `tenant_id`.
- Series title.
- Business question.
- Intelligence type.
- Scope.
- Current Market Intelligence identifier.
- Latest version.
- Series status.
- Responsible team.
- Created time.
- Updated time.

### Market Intelligence Table

The `market_intelligence` table should contain:

- Market Intelligence identifier.
- Intelligence series.
- Tenant and organizational scope.
- Intelligence version.
- Current-version indicator.
- Classification.
- Scope.
- Time horizon.
- Current lifecycle state.
- Current source and evidence projections.
- Current validation state.
- Current analysis state.
- Current Recommendation state.
- Current review and publication state.
- Freshness.
- Quality.
- Conflict.
- Source and synchronization state.
- Record version.
- Audit timestamps.

Repeating, historical, restricted, and large data must remain in child or controlled-storage records.

### Primary Keys

```text
PRIMARY KEY (intelligence_series_id)
```

for `market_intelligence_series`.

```text
PRIMARY KEY (market_intelligence_id)
```

for `market_intelligence`.

### Tenant Protection

Every Tenant-owned Market Intelligence table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

Shared external reference tables must use explicit access and licensing controls.

### Recommended Indexes

```text
idx_market_intelligence_tenant_status
  (tenant_id, status)

idx_market_intelligence_tenant_series_version
  (tenant_id, intelligence_series_id, intelligence_version)

idx_market_intelligence_tenant_current
  (tenant_id, intelligence_series_id, is_current_version)

idx_market_intelligence_type
  (tenant_id, intelligence_type, status)

idx_market_intelligence_scope
  (tenant_id, scope, market_region_id)

idx_market_intelligence_vehicle
  (tenant_id, vehicle_id)

idx_market_intelligence_model
  (tenant_id, vehicle_model_reference)

idx_market_intelligence_segment
  (tenant_id, market_segment_id)

idx_market_intelligence_freshness
  (tenant_id, freshness_status, expires_at)

idx_market_intelligence_publication
  (tenant_id, publication_status, published_at)

idx_market_intelligence_review
  (tenant_id, review_status)

idx_market_intelligence_quality
  (tenant_id, data_quality_status)

idx_market_intelligence_conflict
  (tenant_id, conflict_status)

idx_market_intelligence_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, intelligence_number, intelligence_version)
```

```text
UNIQUE (
  tenant_id,
  intelligence_series_id,
  intelligence_version
)
```

A partial unique constraint or equivalent transactional control should enforce one current version:

```text
UNIQUE (
  tenant_id,
  intelligence_series_id
)
WHERE is_current_version = true
```

Source-record deduplication may use:

```text
UNIQUE (
  market_source_id,
  source_record_reference,
  source_record_version
)
```

or the equivalent provider-specific deduplication key.

### Immutable Published Versions

The persistence layer must prevent material updates when:

```text
status IN (
  'PUBLISHED',
  'STALE',
  'SUPERSEDED',
  'ARCHIVED'
)
```

Permitted changes should be limited to governed projections such as:

- Freshness state.
- Consumption records.
- Outcome measurements.
- Reconciliation.
- Withdrawal.
- Legal hold.
- Audit.

Published evidence, calculations, analysis, and publication artifacts must remain immutable.

### Source Storage

`market_sources` should preserve:

- Source identifier.
- Type.
- Authority.
- Owner.
- Provider.
- License.
- Usage restrictions.
- Security classification.
- Approved methods.
- Geographic scope.
- Product scope.
- Status.
- Effective period.
- Review history.
- Related Events.

### Ingestion Storage

Ingestion tables should preserve:

- Source.
- Job.
- Batch.
- Method.
- Connector version.
- Command.
- Idempotency key.
- Requested time.
- Started time.
- Completed time.
- Source counts.
- Accepted counts.
- Duplicate counts.
- Rejected counts.
- Quarantined counts.
- Failure.
- External Confirmation.
- Reconciliation.
- Related Events.

### Source Record Storage

Source records should preserve:

- Source.
- Provider record identifier.
- Provider record version.
- Original payload reference.
- Original payload hash.
- Publication time.
- Retrieval time.
- Deduplication key.
- Security classification.
- Normalization state.
- Exclusion state.
- Related Events.

Large raw payloads should remain in controlled object or evidence storage.

### Observation Storage

`market_observations` should preserve:

- Observation identifier.
- Intelligence version.
- Source records.
- Observation type.
- Original value.
- Normalized value.
- Unit.
- Currency.
- Product scope.
- Geographic scope.
- Time.
- Verification.
- Evidence.
- Hash.
- Related Events.

### Evidence Storage

Evidence package and item tables should preserve:

- Intelligence version.
- Source.
- Observation.
- Evidence role.
- Original reference.
- Normalized reference.
- Hash.
- Integrity state.
- Inclusion or exclusion.
- Restriction.
- Security classification.
- Retention.
- Legal hold.
- Related Events.

### Normalization Storage

Normalization records should preserve:

- Input.
- Output.
- Normalization type.
- Method.
- Version.
- Adjustments.
- Confidence where meaningful.
- Human Review.
- Timestamp.
- Hash.
- Related Events.

### Calculation Storage

`market_calculations` should preserve:

- Calculation identifier.
- Intelligence version.
- Calculation type.
- Formula.
- Formula version.
- Inputs.
- Input hash.
- Normalization.
- Currency.
- Units.
- Rounding.
- Outputs.
- Output hash.
- Validation.
- Generated time.
- Expiration.
- Related Events.

### Forecast Storage

Forecast tables should preserve:

- Forecast identifier.
- Intelligence version.
- Target.
- Horizon.
- Method.
- Model.
- Model version.
- Inputs.
- Input hash.
- Outputs.
- Scenarios.
- Confidence intervals.
- Assumptions.
- Limitations.
- Review.
- Generated time.
- Expiration.
- Related Events.

### Recommendation Storage

`market_recommendations` should preserve:

- Recommendation identifier.
- Intelligence version.
- Type.
- Content.
- Scope.
- Evidence.
- Assumptions.
- Limitations.
- Priority.
- Owner role.
- Action Class.
- Human Decision requirement.
- Status.
- Expiration.
- Related Decisions.
- Related Events.

### Review and Publication Storage

Review and publication tables should preserve:

- Intelligence version.
- Request.
- Scope.
- Assigned roles.
- Decision.
- Reason.
- Limitations.
- Audience.
- Artifact.
- Artifact hash.
- Effective period.
- Withdrawal.
- Related Events.

### Consumption Storage

Consumption records should preserve:

- Intelligence version.
- Consumer.
- Purpose.
- Fields consumed.
- Freshness at consumption.
- Time.
- Resulting Decision.
- Resulting Command.
- Resulting External Confirmation.
- Measured outcome.
- Related Events.

### Derived Intelligence

AI-derived records must remain separate from original observations and deterministic calculations.

Each derived record should preserve:

- Output type.
- Value.
- Model.
- Model version.
- Prompt version.
- Input-record versions.
- Evidence references.
- Confidence.
- Assumptions.
- Limitations.
- Generated time.
- Expiration.
- Review status.

### Audit Storage

Market Intelligence audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace restricted source, Customer, legal, or commercial values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Source.
- Geographic region.
- Intelligence type.
- Observation date.
- Publication date.
- Retention class.
- Security classification.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Source-license enforcement.
- Evidence integrity.
- Version uniqueness.
- Published-version immutability.
- Referential integrity.
- Audit integrity.

### Hard Deletion

A Market Intelligence version must not be hard-deleted when referenced by:

- Pricing Decision.
- Inventory Decision.
- Vehicle-sourcing Decision.
- Campaign Decision.
- Trade-In policy.
- Finance-program review.
- Quotation.
- Deal.
- Playbook.
- Recommendation.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Audit evidence.
- Legal hold.

Withdrawal, supersession, archival, governed redaction, anonymization, or legally required deletion workflows must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `PUBLIC_SOURCE_REFERENCE` | Approved public source references |
| `LICENSED_SOURCE_RESTRICTED` | Paid reports and licensed datasets |
| `COMPETITOR_RESTRICTED` | Competitor observations and analysis |
| `INTERNAL_AGGREGATE` | Internal performance and demand aggregates |
| `INTERNAL_PRICING_RESTRICTED` | Internal selling-price and price-position data |
| `INTERNAL_COST_RESTRICTED` | Vehicle cost, margin, acquisition assumptions |
| `REGULATORY_AND_LEGAL` | Regulatory analysis and legal review |
| `DERIVED_INTELLIGENCE` | Forecasts, risks, Recommendations |
| `PUBLICATION_CONTROLLED` | Approved published intelligence |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, and history |

### Authentication

Every internal Market Intelligence operation requires an authenticated Human or service identity.

Anonymous creation, modification, ingestion, review, publication, or export is prohibited.

Public intelligence delivery must use an approved publication mechanism and must not expose restricted data.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Department.
- Team.
- User role.
- Source license.
- Intelligence type.
- Visibility.
- Publication audience.
- Geographic scope.
- Product scope.
- Requested field.
- Requested action.
- Business purpose.
- Data classification.
- Legal hold.
- Delegated authority.

### Example Role Boundaries

#### Sales User

May access approved published sales intelligence appropriate to assigned scope.

Must not access:

- Restricted source content.
- Internal cost.
- Margin.
- Confidential competitor detail.
- Legal-review notes.
- Raw Customer-level aggregate inputs.

#### Pricing Analyst

May access approved pricing evidence, comparable samples, calculations, and Recommendations within assigned scope.

Pricing Analyst access does not authorize an actual price change.

#### Inventory Analyst

May access approved supply, demand, scarcity, and sourcing intelligence.

Inventory Analyst access does not authorize Reservation, Allocation, transfer, or acquisition.

#### Market Analyst

May create, normalize, analyze, and prepare intelligence within assigned scope.

Market Analyst access does not automatically authorize publication or binding action.

#### Data Steward

May review:

- Source identity.
- Deduplication.
- Vehicle normalization.
- Geographic normalization.
- Data quality.
- Conflicts.
- Reconciliation.
- Lineage.

#### Legal or Compliance Reviewer

May access restricted source, license, regulatory, privacy, and compliance evidence required for assigned review.

#### Manager or Executive

May access approved intelligence and make configured Decisions within delegated authority.

Management access does not bypass legal, licensing, privacy, pricing, finance, Inventory, or Command controls.

#### AI Agent

May access only the minimum Market Intelligence context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Source-license-aware.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to Customer-level data, internal cost, margin, licensed reports, legal notes, and confidential competitor data.

#### Integration Service

May access only fields required for an approved source, analytics, publication, or downstream integration.

### Source-License Enforcement

Source-license restrictions must apply to:

- Storage.
- Display.
- Search.
- AI processing.
- Embeddings.
- Download.
- Export.
- Publication.
- Redistribution.
- Retention.
- Derived-data use.

A User’s role must not override source-license restrictions.

### Field-Level Protection

Restricted fields must use:

- Field-level authorization.
- Encryption.
- Tokenization where appropriate.
- Masking.
- Controlled evidence references.
- Export restrictions.
- AI-context restrictions.
- Purpose limitation.
- Audit logging.

Restricted examples include:

- Licensed report content.
- Internal Vehicle cost.
- Internal gross margin.
- Confidential pricing strategy.
- Restricted competitor observations.
- Customer-derived internal metrics.
- Legal-review notes.
- Source credentials.

### Internal Aggregate Protection

Internal aggregates must:

- Use approved aggregation.
- Enforce minimum group sizes where configured.
- Apply suppression.
- Prevent drill-down to individual Customers.
- Prevent cross-Tenant comparison without authorization.
- Prevent re-identification.
- Preserve source metric definitions.
- Restrict export.
- Restrict AI access.

### Customer Privacy

Market Intelligence must not expose unnecessary Customer information.

Customer-derived intelligence must:

- Use lawful purpose.
- Use approved aggregation.
- Follow retention.
- Support privacy rights.
- Prevent individual profiling beyond permitted purpose.
- Prevent unlawful discrimination.
- Prevent use of finance, health, religion, ethnicity, political views, or another prohibited attribute for market targeting.

### Competitor Data Protection

Competitor data must:

- Use permitted sources.
- Preserve source and date.
- Respect contractual restrictions.
- Avoid trade-secret handling without lawful authority.
- Avoid unlawful coordination.
- Restrict sensitive analysis.
- Prevent external redistribution where prohibited.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Internal data aggregation.
- Source subscriptions.
- Search.
- Vector retrieval.
- Calculations.
- Forecasts.
- Events.
- Queues.
- Caches.
- Documents.
- Analytics.
- Publications.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Source Credential Protection

Source credentials, API keys, tokens, cookies, signing secrets, and connector secrets must:

- Remain in approved secret-management systems.
- Never appear in Market Intelligence records.
- Never appear in Prompts.
- Never appear in ordinary Logs.
- Use rotation.
- Use least privilege.
- Use audited service identities.

### Ingestion Security

Source ingestion must use applicable:

- Authentication.
- Signature validation.
- TLS.
- Source validation.
- Tenant routing.
- Schema validation.
- Payload-size limits.
- Rate limiting.
- Replay protection.
- Deduplication.
- Malware scanning.
- Content-type validation.
- Quarantine.
- Security logging.

An external source record must not select its own unrestricted `tenant_id`.

Tenant routing must derive from trusted integration configuration.

### Document and File Security

Market reports, source files, and evidence documents must:

- Use controlled storage.
- Preserve hashes.
- Use non-predictable references.
- Be scanned for malware.
- Be treated as untrusted input.
- Prevent active-content execution.
- Prevent public indexing.
- Restrict download.
- Follow source-license restrictions.
- Follow retention.
- Support legal hold.
- Prevent unapproved model-training use.

### AI and Vector Security

AI and vector access must enforce:

- Tenant scope.
- Source-license scope.
- Publication scope.
- Visibility.
- Security classification.
- Intelligence version.
- Freshness.
- Retention.
- Withdrawal.
- Deletion propagation.

Restricted raw sources must not enter general-purpose embeddings.

### Command Security

Market Intelligence itself must not directly execute operational Commands.

When an authorized Decision initiates a downstream action, the Command must include:

- Authenticated service identity.
- `tenant_id`.
- Responsible Domain Object.
- Market Intelligence identifier and version.
- Decision identifier.
- Requested action.
- Current record versions.
- Human authority or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Market Intelligence activity must record:

- `tenant_id`.
- `market_intelligence_id`.
- `intelligence_series_id`.
- Intelligence version.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Source references.
- Source-license status.
- Evidence hash.
- Calculation references.
- Analysis snapshot hash.
- Recommendation.
- Previous value or secure hash.
- New value or secure hash.
- Authority category.
- Record version.
- Applied Business Rules.
- Human Decision.
- AI involvement.
- Model and Prompt versions.
- Command.
- Idempotency key.
- External Confirmation.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Events.

### Security Events

ASOS must detect and record:

- Cross-Tenant Market Intelligence access attempts.
- Unauthorized source access.
- Source-license violation.
- Prohibited scraping or collection attempt.
- Source credential exposure.
- Source webhook replay.
- Tenant-routing mismatch.
- Evidence-hash mismatch.
- Data tampering.
- Restricted report export.
- Customer-level data exposure.
- Re-identification attempt.
- Internal margin exposure.
- Unauthorized publication.
- Publication-audience violation.
- Stale intelligence used as current without authority.
- Unauthorized price or Inventory action.
- Command replay.
- AI access outside approved scope.
- Prompt injection inside source content.
- Audit-log tampering.

### Market Intelligence Integrity

The platform must detect or prevent:

- Unsupported market-wide conclusions.
- Duplicate evidence inflating sample size.
- Source-content substitution.
- Published-version modification.
- Hidden contradicting evidence.
- Incorrect Vehicle matching.
- Invalid currency comparison.
- Invalid geographic comparison.
- Advertised price represented as transaction price.
- Customer-level data leakage.
- Recommendation represented as Decision.
- Decision represented as completed action.
- AI output represented as source evidence.
- Stale intelligence presented as current.
- Multiple current versions in one series.

### Privacy and Retention

Market Intelligence retention must follow:

- Applicable law.
- Tenant policy.
- Source licenses.
- Provider agreements.
- Customer privacy rights.
- Regulatory requirements.
- Commercial record requirements.
- Legal holds.
- Audit requirements.

Privacy and deletion workflows must propagate to:

- Source projections.
- Evidence indexes.
- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Exports.
- Publications where required.
- Non-authoritative replicas.
- Backups according to policy.

Required lawful source, commercial, security, and audit evidence may remain only where permitted.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Source ingestion.
- Specific source connectors.
- AI analysis.
- Forecast generation.
- Intelligence publication.
- Internal notifications.
- External sharing.
- Market-data export.
- Downstream consumption.
- Vector indexing.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Customer](./Customer.md)
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Inventory Record](./InventoryRecord.md)
- [ASOS Lead](./Lead.md)
- [ASOS Qualified Lead](./QualifiedLead.md)
- [ASOS Opportunity](./Opportunity.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Quotation](./Quotation.md)
- [ASOS Trade-In](./TradeIn.md)
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Financial Contract](./FinancialContract.md)
- [ASOS Deal](./Deal.md)
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Market Intelligence baseline.

Market Intelligence preserves explicit separation among source evidence, observed facts, deterministic calculations, Human interpretation, AI-derived intelligence, Recommendations, Authoritative Human Decisions, Commands, and External Confirmations.

Market Intelligence provides decision support and does not independently authorize or execute binding commercial actions.

Published Market Intelligence versions are immutable.

Material changes require a new governed intelligence version.

Stale or expired intelligence must not be silently used as current decision support.

Source-ingestion deduplication, ASOS Event Consumer deduplication using `event_id`, and retryable Command protection using `idempotency_key` remain separate controls.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.

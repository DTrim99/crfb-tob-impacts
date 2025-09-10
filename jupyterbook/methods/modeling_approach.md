# Modeling Approach

This chapter describes the methodology used to analyze the budgetary impacts of the six Social Security taxation reform options. Our analysis employs PolicyEngine's microsimulation framework to provide precise, year-by-year estimates of the revenue effects of each policy option.

## PolicyEngine Microsimulation Framework

### Overview
PolicyEngine is an open-source microsimulation platform that models the effects of tax and transfer policy changes on individual taxpayers and households. The framework uses detailed household-level data to simulate policy changes and calculate their budgetary and distributional impacts.

### Data Source
Our analysis uses the Enhanced Current Population Survey (CPS) dataset, which provides:
- Detailed demographic and economic information for approximately 200,000 households
- Representative sampling weights for national-level estimates
- Comprehensive income and tax information
- Social Security benefit amounts and other transfer payments

### Microsimulation Process
The microsimulation process follows these steps:

1. **Baseline Calculation**: Calculate tax liabilities under current law for each household
2. **Reform Application**: Apply each policy reform to the same households
3. **Impact Calculation**: Compute the difference between baseline and reform scenarios
4. **Aggregation**: Sum impacts across all households using sampling weights
5. **Scaling**: Convert to national-level estimates

## Policy Reform Implementation

### Reform Definition
Each policy option is implemented as a PolicyEngine Reform object, which specifies changes to tax parameters. The reforms are defined using PolicyEngine's parameter system, which allows for precise specification of policy changes over time.

### Key Parameters Modified
The analysis modifies several key parameters across the different policy options:

- `gov.irs.social_security.taxability.rate.base`: Base taxation rate for Social Security benefits
- `gov.irs.social_security.taxability.rate.additional`: Additional taxation rate for higher-income beneficiaries
- `gov.irs.social_security.taxability.threshold.base.main.*`: Income thresholds for benefit taxation
- `gov.contrib.crfb.senior_deduction_extension.applies`: Extension of bonus senior deduction
- `gov.contrib.crfb.ss_credit.*`: Social Security tax credit parameters
- `gov.contrib.crfb.tax_employer_payroll_tax.*`: Employer payroll tax inclusion parameters

### Time Period Analysis
The analysis covers a 10-year budget window from 2026 to 2035, providing year-by-year estimates for each policy option. This timeframe allows for:
- Assessment of short-term implementation effects
- Analysis of medium-term revenue impacts
- Evaluation of policy sustainability over time

## Calculation Methodology

### Revenue Impact Calculation
For each policy option and year, the revenue impact is calculated as:

```
Revenue Impact = Baseline Income Tax - Reform Income Tax
```

Where:
- **Baseline Income Tax**: Total income tax liability under current law
- **Reform Income Tax**: Total income tax liability under the policy reform
- **Revenue Impact**: Positive values indicate revenue increases, negative values indicate revenue decreases

### Baseline Pre-computation
To improve computational efficiency, baseline income tax calculations are pre-computed for all years in the analysis period. This approach:
- Eliminates redundant calculations across policy options
- Ensures consistency in baseline assumptions
- Reduces total computation time

### Error Handling
The analysis includes robust error handling to ensure reliability:
- Individual calculation failures are logged but do not stop the overall process
- Failed calculations return zero impact values
- Checkpoint saving allows for resumption of interrupted analyses

## Policy-Specific Implementation Details

### Policy 1: Complete Repeal
```python
def get_repeal_social_security_reform():
    return Reform.from_dict({
        "gov.irs.social_security.taxability.rate.base": {
            "2026-01-01.2100-12-31": 0
        },
        "gov.irs.social_security.taxability.rate.additional": {
            "2026-01-01.2100-12-31": 0
        }
    }, country_id="us")
```

### Policy 2: Flat 85% Taxation
```python
def get_flat_ss_tax_reform():
    return Reform.from_dict({
        "gov.irs.social_security.taxability.rate.base": {
            "2024-01-01.2100-12-31": 0.85
        },
        "gov.irs.social_security.taxability.threshold.base.main.*": {
            "2024-01-01.2100-12-31": 0
        }
    }, country_id="us")
```

### Policy 4: Tax Credit Approach
The tax credit approach includes multiple variants with different credit amounts ($300, $600, $900, $1,200, $1,500) to analyze the sensitivity of results to credit design.

### Policy 6: Phased Implementation
The phased Roth-style swap includes complex time-varying parameters that gradually transition from benefit taxation to payroll contribution taxation over multiple years.

## Data Quality and Limitations

### Data Limitations
- The Enhanced CPS dataset represents a snapshot in time and may not fully capture future demographic or economic changes
- Behavioral responses to policy changes are not explicitly modeled
- Interactions with state tax systems are not included

### Assumptions
- Taxpayers comply with new policy provisions
- No significant changes in Social Security benefit levels or eligibility
- Economic conditions remain stable over the analysis period
- Other tax provisions remain unchanged

### Validation
- Results are validated against known PolicyEngine calculations
- Cross-checks are performed for policy implementations
- Sensitivity analysis is conducted for key parameters

## Computational Approach

### Incremental Processing
The analysis uses an incremental processing approach that:
- Saves results after each policy-year calculation
- Allows for resumption of interrupted analyses
- Provides real-time progress monitoring

### Performance Optimization
- Baseline calculations are pre-computed and reused
- Parallel processing is used where possible
- Memory management is optimized for large datasets

### Output Format
Results are saved in CSV format with the following structure:
- `policy_id`: Unique identifier for each policy option
- `policy_name`: Descriptive name of the policy
- `credit_value`: Credit amount (for Policy 4 variants)
- `year`: Analysis year (2026-2035)
- `impact_billions`: Revenue impact in billions of dollars

This methodology provides a robust foundation for analyzing the budgetary impacts of Social Security taxation reform options, enabling policymakers to make informed decisions based on quantitative evidence.

# Project Title: The Hotel Business Owner's Dilemma

## Context
You own a mid-sized, independent hotel ("The Azure Stay"). While the property is beautiful, profitability is stagnant. You have identified three critical pain points that require data-driven solutions

## Problem
You are selling rooms, but paying huge commissions to third parties (OTAs like Expedia/Booking.com). You need to identify which channels are actually profitable, not just which ones bring volume.

## Goal
Maximize Net Revenue (revenue after commissions) by optimizing the mix of booking channels.

## Pain Point
The core problem is high distribution costs. The hotel relies heavily on third-party channels, such as Online Travel Agencies (OTAs like Expedia and Booking.com), which charge substantial commission fees. While these OTAs successfully drive booking volume, the massive commission payouts severely erode the actual profit margins.

## SMART Objective
Increase Net ADR (Average Daily Rate after commissions) by 10% within 6 months by optimizing promotions (rate codes) across different booking channels based on their commission structures.

## Hypothesis

## Prompt used to generated mock dataset

Act as a Data Engineer. I need to generate a mock dataset for a hotel analytics project evaluating 'High Distribution Costs (Channel Profitability)'. Write a Python script using the pandas and random libraries to generate 4 tables and save them as CSV files. The schema and logic must be strictly followed:

### 1. dim_channels (4 rows)

- channel_id: ['CH_OTA_BKG', 'CH_OTA_EXP', 'CH_DIRECT', 'CH_WHOLE']
- channel_name: ['Booking.com', 'Expedia', 'Direct Website', 'Wholesale Partner']
- channel_type: ['OTA', 'OTA', 'Direct', 'Wholesale']
- commission_model: ['Percentage', 'Percentage', 'Marketing Cost', 'Net Rate']
- default_commission_rate: [0.18, 0.15, 0.00, 0.00]

### 2. dim_rate_codes (4 rows)

- rate_code_id: ['RACK', 'PROMO', 'CORP', 'NET']
- rate_name: ['Rack Rate', 'Promotional Rate', 'Corporate Rate', 'Net Rate']
- is_commissionable: [True, True, True, False]

### 3. fact_bookings (5,000 rows)

- booking_id: sequential strings (e.g., BK_00000)
- booking_date & check_in_date: Random dates in 2025/2026, where check_in_date is after booking_date
- channel_id: Randomly assigned from dim_channels
- rate_code_id: Randomly assigned from dim_rate_codes
- rooms_sold: Random integer between 1 and 3
- gross_room_revenue: Random float value
- commission_amount: Calculated field. If the rate code is_commissionable is True, calculate this as gross_room_revenue * default_commission_rate of the respective channel. Otherwise, it is 0
- net_room_revenue: Calculated field. gross_room_revenue - commission_amount
- status: Randomly assigned ['Confirmed', 'Cancelled', 'Checked-Out']

### 4. fact_marketing_spend (120 rows)

- spend_id: sequential strings (e.g., SP_000)
- spend_date: Random dates in 2025/2026
- channel_id: Strictly set to 'CH_DIRECT'
- platform: Randomly ['Google Ads', 'Facebook']
- cost_amount: Random integer for ad spend

Please output the complete, executable Python code to generate these files.

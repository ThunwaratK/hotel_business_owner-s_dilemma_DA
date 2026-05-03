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
- OTA channels have lower net revenue than other channels due to commission costs.
- When marketing spend is considered, direct channels may not always be the most cost-efficient.
- Offering promotional rates on OTA channels reduces Net ADR per booking compared with non-promotional OTA bookings.

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
- status: Randomly assigned ['Confirmed', 'Cancelled', 'Checked-Out']
- commission_amount: Calculated field. If the rate code is_commissionable is True, calculate this as gross_room_revenue * default_commission_rate of the respective channel. Otherwise, it is 0
- net_room_revenue: Calculated field. gross_room_revenue - commission_amount

### 4. fact_marketing_spend (120 rows)

- spend_id: sequential strings (e.g., SP_000)
- spend_date: Random dates in 2025/2026
- channel_id: Strictly set to 'CH_DIRECT'
- platform: Randomly ['Google Ads', 'Facebook']
- cost_amount: Random integer for ad spend
## Data Dictionary
## Data Dictionary

### dim_channels (data/hotel_csv/dim_channels.csv)
Channel reference table.

- channel_id: Unique channel key (e.g., CH_OTA_BKG).
- channel_name: Human-readable channel name.
- channel_type: Channel group (OTA, Direct, Wholesale).
- commission_model: How distribution cost is applied (Percentage, Marketing Cost, Net Rate).
- default_commission_rate: Percentage commission rate used for commissionable bookings.

### dim_rate_codes (data/hotel_csv/dim_rate_codes.csv)
Rate plan reference table.

- rate_code_id: Unique rate code key (RACK, PROMO, CORP, NET).
- rate_name: Human-readable rate name.
- is_commissionable: Whether the rate is subject to commission.

### fact_bookings (data/hotel_csv/bookings.csv)
Booking-level fact table. Date format is dd/mm/yyyy.

- booking_id: Unique booking key.
- booking_date: Booking creation date.
- check_in_date: Arrival date.
- channel_id: Foreign key to dim_channels.
- rate_code_id: Foreign key to dim_rate_codes.
- rooms_sold: Number of rooms in the booking.
- gross_room_revenue: Revenue before commission.
- status: Booking status (Confirmed, Cancelled, Checked-Out).
- is_commissionable: Pulled from dim_rate_codes for the booking rate.
- default_commission_rate: Pulled from dim_channels for the booking channel.
- commission_cost: If is_commissionable is TRUE, gross_room_revenue * default_commission_rate; else 0.
- net_room_revenue: gross_room_revenue - commission_cost.
- OCC: Occupancy rate value for the check_in_date (0 to 1). If recalculating, use rooms_sold / total_rooms, but total_rooms is not stored, so OCC is treated as provided.
- Day Name: Day of week for check_in_date.
- true_gross_room_revenue: If is_commissionable is TRUE, equals gross_room_revenue. If FALSE, gross_room_revenue / (1 - default_commission_rate).
- true_commission_cost: If is_commissionable is TRUE, equals commission_cost. If FALSE, true_gross_room_revenue * default_commission_rate.
- true_net_room_revenue: If is_commissionable is TRUE, equals net_room_revenue. If FALSE, equals gross_room_revenue.
- ADR: gross_room_revenue / rooms_sold.
- net_ADR: net_room_revenue / rooms_sold.
- net_RevPAR: net_ADR * OCC.

### fact_marketing_spend (data/hotel_csv/fact_marketing_spend.csv)
Direct marketing spend fact table. Date format is dd/mm/yyyy.

- spend_id: Unique spend key.
- spend_date: Spend date.
- channel_id: Always CH_DIRECT.
- platform: Marketing platform (Google Ads, Facebook).
- cost_amount: Spend amount.

  
## 5. Data Cleaning
- Filtered out cancelled bookings to eliminate non-realized revenue and ensure the analysis reflects only actual revenue-generating stays. This step improves the accuracy of Net ADR and channel profitability calculations by excluding reservations that did not materialize 

## x. Recommendations
- เพิ่ม Wholesale partners ทำให้มีการเข้าพักจากช่องทางนี้มากขึ้น เพื่อลดการเพิ่งพา OTA Channels
- ทำแผนเปลี่ยนลูกค้า OTA ให้กลับมาจองทาง Direct Website เช่นตอนเช็คอินแจกสิทธิ์ส่วนลดครั้งถัดไปเฉพาะการจองผ่านเว็บไซต์โรงแรม
- ปรับลดค่าใช้จ่ายในการทำโฆษณาลง ให้เหมาะสมกับรายได้ของช่องทาง Direct Website
- ทำโปรโมชันตามฤดูกาล
        
        Low season: ทำโปรโมชันมากขึ้น
        High season: จำกัดการทำโปรโมชันให้ลดลง และเน้นโปรโมชัน  
        ไปที่ห้องที่ขายได้น้อย
- คำนวณค่า Net ADR เพื่อกำหนดเกณฑ์ขั้นต่ำก่อน เพื่อพิจารณาก่อนทำโปรโมชัน

## x. Contributors
- นายชยานนท์      จันทพันธ์                66102010135 
- นายชโยดมปณ์  ธณวรโชติโภคิณ    66102010235 
- นายธัญวรัตม์      ก.วิบูลย์ศักดิ์ศรี      66102010567

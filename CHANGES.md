# Changes (final ER diagram = ChefConnect_ER_Diagram.png, saved here as ER-Diagram.png)

The final diagram matches the original table design, so the table definitions, inserts, queries
and Execution.txt are unchanged (the earlier experimental schema changes were not kept).

## Diagram vs. code check
| Diagram | Code |
|---|---|
| LOCATIONS: location_id, area, city | same |
| CHEF_AVAILABILITY (weak): chef_id, availability_dt, start_time (PK), end_time, status | same |
| CHEF_SPECIALITIES: chef_id, cuisine_id (PK, FK), skill_level; CHEFS 1:N, CUISINES 1:N | same (junction table) |
| PAYMENTS (weak): booking_id, payment_date, payment_type (PK) | same |
| REVIEWS (weak): booking_id (PK, FK), rating; BOOKINGS 1 : 0..1 REVIEWS | same |
| BOOKINGS derived: booking_total, grand_total, balance_due, chef_payout, platform_commission | calculated in queries; now also in view booking_financials |
| CHEFS.avg_rating (derived) | calculated in queries; now also in view chef_avg_rating |

## What was edited
* Database_Creation_Insertion.txt: added the two views (section 26). Tables and data unchanged.
* README.md: weak/strong entities, CHEF_SPECIALITIES entity, BOOKINGS-REVIEWS 0..1, derived list
  (platform_commission added, days_remaining removed), "15 tables" corrected to 12, diagram notes.
* Main_Table_Relations.txt: comment "1 booking : 0..1 review".
* ER-Diagram.png: replaced with your final diagram.


# Update: query levels reorganised (Q1 - Q100)

| File | Queries | Content |
|---|---|---|
| Simple_queries.txt | Q1 - Q40 | SELECT *, WHERE with AND / OR / NOT IN, LIKE, BETWEEN, IN, ORDER BY, LIMIT, DISTINCT, and INSERT / UPDATE / DELETE / ALTER / RENAME / DROP on a practice copy of menus |
| Medium_queries.txt | Q41 - Q70 | Aggregate functions, INNER / LEFT / RIGHT / SELF / CROSS joins, GROUP BY, HAVING, business analysis |
| Hard_queries.txt | Q71 - Q100 | Subqueries (Q71 - Q80), views (Q81 - Q84), stored procedures (Q85 - Q91), triggers (Q92 - Q97), CASE, stored function and transaction (Q98 - Q100) |

* Aggregate-only queries moved from Simple to Medium; subqueries moved from Medium to Hard.
* The earlier CTE, window-function and dashboard queries were removed (not needed at this level).
* Hard_queries.txt creates 3 views, 7 procedures, 6 triggers, 1 function and 1 helper table (booking_status_log).
* Execution.txt was regenerated for all 100 queries.
* Tables, data and the two views in Database_Creation_Insertion.txt are unchanged.
* Procedures sp_create_booking, sp_record_payment and sp_cancel_booking were made shorter: they now only insert / update,
  and the rules are checked by the triggers (Q93, Q94, Q96) and by the table constraints.
* Trigger trg_payment_check now has three simple checks (positive amount, cancelled booking, balance due).

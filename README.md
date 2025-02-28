# Kimia-Farma-Big-Data-Analytics-Project-Based-Internship-Program
SQL Code untuk meembuat tabel analisis
CREATE TABLE kimia_farma.tabel_analysis AS
SELECT
    a.transaction_id,
    a.date,
    a.branch_id,
    c.branch_name,
    c.kota,
    c.provinsi,
    a.rating AS rating_transaction,
    a.customer_name,
    a.product_id,
    p.product_name,
    a.price,
    a.discount_percentage,
    CASE 
        WHEN a.price <= 50000 THEN 0.1
        WHEN a.price > 50000 AND a.price <= 100000 THEN 0.15
        WHEN a.price > 100000 AND a.price <= 300000 THEN 0.2
        WHEN a.price > 300000 AND a.price <= 500000 THEN 0.25
        ELSE 0.3
    END AS gross_profit_percentage,
    (a.price * (1 - (a.discount_percentage / 100))) AS nett_sales,
    (a.price * (1 - (a.discount_percentage / 100)) * 
    CASE 
        WHEN a.price <= 50000 THEN 0.1
        WHEN a.price > 50000 AND a.price <= 100000 THEN 0.15
        WHEN a.price > 100000 AND a.price <= 300000 THEN 0.2
        WHEN a.price > 300000 AND a.price <= 500000 THEN 0.25
        ELSE 0.3
    END) AS nett_profit,
    c.rating AS rating_branch
FROM kimia_farma.kf_final_transaction a
JOIN kimia_farma.kf_kantor_cabang c ON a.branch_id = c.branch_id
JOIN kimia_farma.kf_product p ON a.product_id = p.product_id;

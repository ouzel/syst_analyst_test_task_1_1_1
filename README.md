Дрозд Софья Александровна
# Тестовое задание на системного аналитика в CDL 

![image](https://github.com/ouzel/syst_analyst_test_task_1_1_1/assets/90156908/9b253c07-f5a4-4cf0-ae8f-384e1a89798b)


```sql
/* total_members — количество пользователей,
total_members_with_orders — количество покупателей (пользователь хотя бы с одним заказом),
total_orders — количество заказов,
total_order_amount — сумма всех заказов. */
SELECT
  COUNT(DISTINCT member_id) AS total_members,
  COUNT(
    DISTINCT CASE
      WHEN order_id IS NOT NULL THEN member_id
    END
  ) AS total_members_with_orders,
  COUNT(order_id) AS total_orders,
  SUM(order_amount) AS total_order_amount
FROM tbl_member
  LEFT JOIN tbl_order ON tbl_member.member_id=tbl_order.member_id;
```

![image](https://github.com/ouzel/syst_analyst_test_task_1_1_1/assets/90156908/f29c12d7-bf69-40f4-bd82-256edf1178f1)


```sql
/* Все абоненты, у кого последнее устройство Samsung. */
SELECT *
FROM tbl_terminal_device
WHERE 
  imei LIKE '12345678%'
  AND created_dt=(
    SELECT MAX(created_dt)
    FROM tbl_terminal_device
    WHERE mob_num=tbl_terminal_device.mob_num
  )
```



```sql
/* Все абоненты, у кого предпоследнее устройство Samsung. */
SELECT mob_num, imei, created_dt
FROM
(
  SELECT 
    mob_num, imei, created_dt, 
    ROW_NUMBER() OVER (PARTITION BY mob_num ORDER BY created_dt DESC) AS rn
  FROM tbl_terminal_device
) tbl_terminal_device_with_rows
WHERE rn = 2 
  AND imei LIKE '12345678%';
```

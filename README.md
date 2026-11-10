# Домашнее задание к занятию "Расширенные возможности SQL" - Якшев Тимур


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. В личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1
```
SELECT
    CONCAT(s.first_name, ' ', s.last_name) AS employee_name,
    c.city,
    COUNT(cu.customer_id) AS customer_count
FROM store AS st
JOIN staff AS s
    ON s.store_id = st.store_id
JOIN customer AS cu
    ON cu.store_id = st.store_id
JOIN address AS a
    ON a.address_id = st.address_id
JOIN city AS c
    ON c.city_id = a.city_id
GROUP BY
    s.staff_id,
    s.first_name,
    s.last_name,
    c.city
HAVING COUNT(cu.customer_id) > 300;
```

---

### Задание 2
```
SELECT COUNT(*) AS film_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
---

### Задание 3
```
SELECT
    DATE_FORMAT(payment_date, '%Y-%m') AS payment_month,
    SUM(amount) AS total_amount,
    COUNT(r.rental_id) AS rental_count
FROM payment AS p
LEFT JOIN rental AS r
    ON r.rental_id = p.rental_id
GROUP BY DATE_FORMAT(payment_date, '%Y-%m')
ORDER BY total_amount DESC
LIMIT 1;
```
---

### Задание 4
```
SELECT
    s.staff_id,
    CONCAT(s.first_name, ' ', s.last_name) AS seller,
    COUNT(p.payment_id) AS sales_count,
    CASE
        WHEN COUNT(p.payment_id) > 8000 THEN 'Да'
        ELSE 'Нет'
    END AS Премия
FROM staff AS s
JOIN payment AS p
    ON p.staff_id = s.staff_id
GROUP BY
    s.staff_id,
    s.first_name,
    s.last_name;
```

---

### Задание 5
```
SELECT f.film_id, f.title
FROM film AS f
WHERE NOT EXISTS (
    SELECT 1
    FROM inventory AS i
    JOIN rental AS r ON r.inventory_id = i.inventory_id
    WHERE i.film_id = f.film_id
    );
```
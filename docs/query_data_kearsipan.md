
### menampilkan objek digital berdasarkan jenis
```
SELECT
now() as time_sec,
COUNT(CASE WHEN mime_type = 'image/jpeg' THEN 1 ELSE NULL END) AS jpeg,
COUNT(CASE WHEN mime_type = 'application/pdf' THEN 1 ELSE NULL END) AS pdf
FROM digital_object
where object_id is not null
```
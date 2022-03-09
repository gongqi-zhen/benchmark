
BigQueryベンチマーク結果

On Demand環境での計測、slotは10+α程度の利用だった

BigQueryResults.csv 標準出力
- https://github.com/gongqi-zhen/benchmark/blob/master/401-BenchmarkBigQuery.sh

sqlite.db 結果
- https://github.com/gongqi-zhen/bqcop
https://cloud.google.com/bigquery/docs/reference/rest/v2/jobs/list

```
% sqlite3 sqlite.db
SQLite version 3.32.3 2020-06-18 14:16:19
Enter ".help" for usage hints.
sqlite> .schema bq_jobs --indent
CREATE TABLE IF NOT EXISTS "bq_jobs"(
  "id" integer primary key autoincrement,
  "created_at" datetime,
  "updated_at" datetime,
  "deleted_at" datetime,
  "job_id" varchar(255),
  "query" varchar(255),
  "user_email" varchar(255),
  "total_bytes_billed" bigint,
  "start_time" datetime,
  "end_time" datetime
);
CREATE INDEX idx_bq_jobs_deleted_at ON "bq_jobs"(deleted_at);
sqlite> SELECT 1.0 * total_bytes_billed / 1000 / 1000 /1000 as 走査サイズ（GB）,
   ...>        (1.0 * total_bytes_billed / 1000 / 1000 / 1000/ 1000) * 5 as 金額（ドル）,
   ...>        start_time as 実行開始日時
   ...> FROM bq_jobs
   ...> ORDER BY total_bytes_billed DESC
   ...> LIMIT 10;
508.146221056|2.54073110528|2022-02-04 10:14:57.462+09:00
390.004211712|1.95002105856|2022-02-04 10:12:18.106+09:00
311.755276288|1.55877638144|2022-02-04 10:50:36.352+09:00
292.695310336|1.46347655168|2022-02-04 10:59:11.977+09:00
284.82469888|1.4241234944|2022-02-04 10:57:45.85+09:00
253.464936448|1.26732468224|2022-02-04 10:44:08.93+09:00
235.8771712|1.179385856|2022-02-04 10:21:00.363+09:00
220.235563008|1.10117781504|2022-02-04 10:25:49.438+09:00
214.195765248|1.07097882624|2022-02-04 10:56:52.083+09:00
199.556595712|0.99778297856|2022-02-04 10:54:24.856+09:00
sqlite>
```
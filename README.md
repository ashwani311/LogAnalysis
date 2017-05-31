# LogAnalysis
Udacity Log Analysis Project

## Views

* View 1: article_views

    ```
      CREATE VIEW
          article_views
      AS
           SELECT
              a.id,
              a.author,
              a.title,
              au.name,
              v.views
           FROM
              (
              SELECT
                  l.path,
                  count(*) as views
              FROM
                  log l,
                  articles a
              WHERE
                  CONCAT('/article/',a.slug) = l.path
              AND
                  l.status = '200 OK'
              GROUP BY
                  l.path
              )   v,
              articles a,
              authors au
           WHERE
              CONCAT('/article/',a.slug) = v.path
           AND
              au.id = a.author;
    ```
    | Column  | Type    |
    | :-------| :-------|
    | id      | Integer |
    | author  | text    |
    | title   | text    |
    | name    | text    |
    | views   | Integer |
    

* View 2: error_report
 
    ```
      CREATE VIEW 
              error_report
        AS
          SELECT
              error_report.date,
              round(100*error_report.errors/total_requests.total,2) as percent_error
          FROM
              (SELECT
                  date(time) as date,
                  COUNT(*) as errors
               FROM
                  log
               WHERE
                  status != '200 OK'
               GROUP BY
                  date(time)
               ORDER BY
                  COUNT(*)
               ) error_report,
               (SELECT
                  date(time) as date,
                  COUNT(*) as total
                FROM
                  log
                GROUP BY
                  date
               ) total_requests
           WHERE
                total_requests.date = error_report.date;
    ```
    | Column         | Type    |
    | :--------------| :-------|
    | date           | Date    |
    | percent_error  | text    |
    

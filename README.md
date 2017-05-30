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

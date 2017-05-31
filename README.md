# LogAnalysis

Udacity Log Analysis Project

## Project Overview
>In this project, you'll work with data that could have come from a real-world web application, with fields representing information that a web server would record, such as HTTP status codes and URL paths. The web server and the reporting tool both connect to the same database, allowing information to flow from the web server into the report.

## Steps To Run

#### Prerequisites:

  * [Python3](https://www.python.org/)
  
  * [Vagrant](https://www.vagrantup.com/)
  
  * [VirtualBox](https://www.virtualbox.org/)
  
#### Virtual Environment Setup:
  
  * Install Vagrant and VirtualBox
  
  * Download or Clone [fullstack-nanodegree-vm](https://github.com/udacity/fullstack-nanodegree-vm) repository.
  
  * Download the [data](https://d17h27t6h515a5.cloudfront.net/topher/2016/August/57b5f748_newsdata/newsdata.zip) from here.
  
  * Unzip this file after downloading it. The file inside is called newsdata.sql.
  
  * Copy the newsdata.sql file and content of this current repository, by either downloading or cloning it from
  [Here](https://github.com/ashwani311/LogAnalysis)
  
  * Launch the Vagrant VM inside Vagrant sub-directory in the downloaded fullstack-nanodegree-vm repository using command:
  ```
    $ vagrant up
  ```
  
  * Then Log into this using command:
  ```
    $ vargrant ssh
  ```
  
  * Change directory to /vagrant and look around with ls and cd.
  
  * Load the data in local database using the command:
  ```
    > psql -d news -f newsdata.sql
  ```
  
  * Use `psql -d news` to connect to database.
  
  Once connected to DB, create the following views with the given query queries:
  
### Views

* View 1: `article_views`

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
    

* View 2: `error_report`
 
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
    

## Run the Analysis
 *  RUN `python main.py`

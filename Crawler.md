
### 1. main flow
  - feature settledown
  - requirements(rps, html size, etc)
  - baseline: single machine, whole workflow
  - scaling 
  
  
#### 1. features
  - extract contents for a set of urls and all hidden urls contained inside their html content
  - provide "query by url" function
  - 
  
  
#### 2. requirements
  - some of the websites require frequent scraping(freq update)
    - we need to think about ip blocking for DDOS preventing(if freq scraping a website)
    - some small server of the websites may crash
    - solution
      - set reasonable scraping interval
  


#### 3. baseline
  - single machine, single thread
    - input a set of url
    - add urls to a set, use Bloom Filter to ensure uniqueness of the url(prevent loop) // use BF to reduce space
    - send GET request to urls, extract web content and parse hidden url
    - add hidden url to pool, restart phase2
   
#### 4. scaling
  - we allow multiple machine to send scraping request to our system. Considering that, we can set up a load balancer after these machines. our LB will redirect these requests to app servers  
    - undelying discussion:
      - consistent hashing detail, round robin detail
  - app servers will push urls into a shared concurrent queue(pool) for working servers to retrieve tasks from queue.
  - our application server will retrieve task, check uniqueness by comparing with BF, if valid, send GET req to website and start parsing the web content
  - if we find more hidden valid url, app server will push these new urls into the queue. Then extract new tasks
  - storage layer
    - If we need to provide query service(such as Google), we need to store url with its meta data into our permanent storage
    - considering urls are not related colsely, we can use Non-relational database to store url objects
    - 
  
    

# graphCV

### Getting started
Clone the project repository to your local projects folder.
```
cd /your/path/to/projects
git clone <githubURL>
```

Create and activate a new anaconda environment
```
conda create -n graphCV python=3.12
conda activate graphCV
```

Navigate to the project folder and pip install the requirements:
```
cd /your/path/to/projects/graphCV
pip install -r requirements.txt
```

### Aura DB
Log into your Aura DB account and start a free instance. Download the instance username and password info.

### Organization
- We will have one central node - the person - with properties of personal website, github, linkedin, google scholar etc. 
- The central node will be linked to "skills" nodes by relationship "has skill".
- Off the "skills" nodes, other node families will be "publications", "projects", "work experience" 

We make a .csv file for each node to define the node and properties.
We make a .csv file for each relationship between nodes, e.g., "has skill" to link central node with skills.

### Convert Google Drive links to downloadable links
- [Conversion Website](https://sites.google.com/site/gdocs2direct/)

### Loading a csv file into neo4j instance
In the instance "QUERY" tab, enter:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=10xWMWzbhxDBUjLz9F487qa4t8Y2B804f'
AS row
MERGE (n:MyProfile {Name: row.Name})
SET
n.Education = row.Education,
n.Statement = row.Statement,
n.GitHub = row.GitHub,
n.LinkedIn = row.LinkedIn,
n.GoogleScholar = row.Google_Scholar
```
To view node in graph, enter:
```
MATCH (n:Name) RETURN n
```
Now load the "skills" node:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=1FsjsXIeD2LTPpTj-kaj4DKUoW34G48JW'
AS row
MERGE (s:MySkills {skill_name: row.skill_name})
SET
s.skillID = row.skillID
```
View the "skills" nodes:
```
MATCH (s:Skill) RETURN s
```
Load the "publications" node:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=1GJ_yrUvWJeds6hxZaEGoQk5jNasX9lGH'
AS row
MERGE (p:Publication {Name: row.title})
SET
p.publicationID = row.publicationID,
p.authors = row.authors,
p.abstract = row.abstract,
p.url = row.url
```
Load the "work experience" node:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=18q_E-ywA2CbeEcswMqQCAv4T7mKnKRhP'
AS row
MERGE (w:WorkExperience {Name: row.company_name})
SET
w.work_experienceID = row.work_experienceID,
w.date = row.date
```

Now load the relationships between the Name and Skills nodes:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=1i7YbfjCcWMJgvTq8YvSphaY90hyLwKAB' AS row
MATCH (n:MyProfile {Name: row.Name})
MATCH (s:MySkills {skill_name: row.skill_name})
MERGE (n)-[r:HAS_SKILL]->(s)
SET r.role = row.role
```
Verify that importing the author relationships worked:
```
MATCH (n:Name)-[r:HAS_SKILL]->(s:Skill) RETURN n, r, s LIMIT 25
```
Now load the relationships between the Skills and Publications nodes:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=1AdZUnAe0YeEQmM6XujIzDxlLwC4kuE_j' AS row
MATCH (p:Publication {publicationID: row.publicationID})
MATCH (s:MySkills {skillID: row.skillID})
MERGE (s)-[r:USED_IN]->(p)
SET r.role = row.role
```

Now load the relationships between the Skills and Work Experience nodes:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=18zOaygeeoDIbZ2J20JvkAGng9K0ras5A' AS row
MATCH (w:WorkExperience {work_experienceID: row.work_experienceID})
MATCH (s:MySkills {skillID: row.skillID})
MERGE (s)-[r:USED_IN]->(w)
SET r.role = row.role
```
### Documentation:
- [Introduction to Vector Indexes and Unstructured Data](https://graphacademy.neo4j.com/courses/llm-vectors-unstructured/?_gl=1*1qrf93q*_gcl_aw*R0NMLjE3Mjg4NDMzMTcuQ2p3S0NBanczNjI0QmhCQUVpd0FreGdUT24wQW9GQmNIMU5xNEpuTzNfVXdFamEwSXdIMlVwVHdBNGJWc2duTThhS2Rxa0R0N1QyTjdCb0NvdVlRQXZEX0J3RQ..*_gcl_au*MTQ0NTc3NjU2NS4xNzI4ODMzMDQ1*_ga*ODkwMDE3NzYzLjE3Mjg4MzMwNDU.*_ga_DL38Q8KGQC*MTcyODgzMzA0My4xLjEuMTcyODg0NDMyOS4wLjAuMA..*_ga_DZP8Z65KK4*MTcyODgzMzA0My4xLjEuMTcyODg0NDMyOS4wLjAuMA..)
- [Vector Indexes](https://neo4j.com/docs/cypher-manual/current/indexes/semantic-indexes/vector-indexes/)
- [Cypher Cheat Sheet](https://neo4j.com/docs/cypher-cheat-sheet/5/aura-dbe/)
- [Embeddings and Vector Indexes Tutorial](https://neo4j.com/docs/genai/tutorials/embeddings-vector-indexes/setup/vector-index/)
- [Embed neo4j as iframe](https://github.com/neo4j-contrib/rabbithole)

### Miscellaneous:
- [GitHub repo for embedding force graphs in a web app](https://github.com/vasturiano/3d-force-graph)
- [Google Drive direct download link converter](https://sites.google.com/site/gdocs2direct/)
- [Great repo for embedding neo4j graphs in webpages and apps](https://github.com/Nhogs/popoto-examples)
- [ONgDB](https://graphfoundation.org/projects/ongdb/): An open source fork of an earlier Neo4j (v3.5) version
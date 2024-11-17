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
FROM 'https://drive.google.com/uc?export=download&id=1eziClqZnRub515V52zEGPZ28bPcEtaQf'
AS row
MERGE (n:Name {Name: row.Name})
SET
n.Name = row.Name,
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
FROM 'https://drive.google.com/uc?export=download&id=1fCeXIsNd_LvXhZLiDnTbBQVXo2YWWuNm'
AS row
MERGE (s:Skill {skill_name: row.skill_name})
SET
s.Skill = row.skill_name
```
View the "skills" nodes:
```
MATCH (s:Skill) RETURN s
```
Now load the relationships between the Name and Skills nodes:
```
LOAD CSV WITH HEADERS
FROM 'https://drive.google.com/uc?export=download&id=1uxQwrtUK9lkFfXDM1Rdrlx7uHtrGOaRd' AS row
MATCH (n:Name {Name: row.Name})
MATCH (s:Skill {skill_name: row.skill_name})
MERGE (n)-[r:HAS_SKILL]->(s)
SET r.role = row.role
```
Verify that importing the author relationships worked:
```
MATCH (n:Name)-[r:HAS_SKILL]->(s:Skill) RETURN n, r, s LIMIT 25
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
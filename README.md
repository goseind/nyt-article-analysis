# NYT Articles and Comments Analysis MongoDB/Docker

Analyzing New York Times Articles and Comments (2020) with MongoDB on a Docker container.

## Instructions

You can find a more detaild description of this repository in this [Blog Post](https://gosein.de/mongo/mongodb/big-data/kaggle/mongodb-atlas/mongodb-charts/analysis/docker/2022/04/17/nyt-articles-analysis-mongodb.html), which also includes a [Video Tutorial on YouTube](https://youtu.be/2HfKDJpGgQ8).

Below you'll find the commands used in this example step by step, as well as the MongoDB queries in this [file](nyt-mongosh-queries.mongodb).

### Run the Docker Container

Commands to setup the docker container and copy the data into it.

* Link to the official [MongoDB Docker Container](https://hub.docker.com/_/mongo)
* Link to the [data of NYT](https://www.kaggle.com/datasets/benjaminawd/new-york-times-articles-comments-2020)

```bash
# Run the docker container
docker run --name some-mongo -p 27018:27017 -d mongo:latest # make sure to expose the port so we can access it later with MangoDB Compass

# Check if the container is running
docker ps

# Copy data from host to docker container
docker cp nyt-articles-2020.csv some-mongo:/nyt.csv

# Open bash on docker container
docker exec -it some-mongo bash
```

### Create a database and import

Commands to create a database and import the data into a collection.

```bash
# Run mongo shell
mongo

# Show existing databases
show dbs

# Create and use new database
use nyt

# Exit mongo shell
quit

# Import the previously copied csv into the new database
mongoimport --db nyt --collection nyt_articles --type csv --headerline --ignoreBlanks --file nyt.csv

# Import with specific datatypes
mongoimport --db nyt --collection nyt_articles_2 --type csv --columnsHaveTypes --fields 'newsdesk.string(),section.string(),subsection.string(),material.string(),headline.string(),abstract.string(),keywords.string(),word_count.int32(),pub_date.date(),n_comments.int32(),uniqueID.string()' --file nyt.csv

# Return to mongo shell
mongo

# Check the databases again to see if nyt is listed now
show dbs

# Use the nyt database
use nyt
```

### Explore and analyze the data

Now that we have stored the data in a database we can execute queries on it. For this part please refer to the separate [query document](nyt-mongosh-queries).

### Using MongoDB Atlas and Charts for visualization

In my [additional blog post](https://gosein.de/mongo/mongodb/big-data/kaggle/mongodb-atlas/mongodb-charts/analysis/docker/2022/04/17/nyt-articles-analysis-mongodb.html) I explain how to use MongoDB Atlas to create a cluster, import the data there and visualize it with MongoDB Charts.

## Data Source

* [New York Times Articles & Comments (2020)](https://www.kaggle.com/datasets/benjaminawd/new-york-times-articles-comments-2020), Benjamin Dornel, 04/2022

## License

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

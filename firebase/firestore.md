# Order and Limit Data with Cloud Firestore

Cloud Firestore provides powerful query functionality for specifying which documents you want to retrieve from a collection. These queries can also be used with either `get()` or `addSnapshotListener()`, as described in Get Data.

**Note:** While the code samples cover multiple languages, the text explaining the samples refers to the Web method names.

## Order and limit data

By default, a query retrieves all documents that satisfy the query in ascending order by document ID. You can specify the sort order for your data using `orderBy()`, and you can limit the number of documents retrieved using `limit()`. If you specify a `limit()`, the value must be greater than or equal to zero.

**Note:** An `orderBy()` clause also filters for existence of the given field. The result set will not include documents that do not contain the given field.

For example, you could query for the first 3 cities alphabetically with:

```javascript
const firstThreeRes = await citiesRef.orderBy('name').limit(3).get();
```

You could also sort in descending order to get the *last* 3 cities:

```javascript
const lastThreeRes = await citiesRef.orderBy('name', 'desc').limit(3).get();
```

You can also order by multiple fields. For example, if you wanted to order by state, and within each state order by population in descending order:

```javascript
const byStateByPopRes = await citiesRef.orderBy('state').orderBy('population', 'desc').get();
```

You can combine `where()` filters with `orderBy()` and `limit()`. In the following example, the queries define a population threshold, sort by population in ascending order, and return only the first few results that exceed the threshold:

```javascript
const biggestRes = await citiesRef.where('population', '>', 2500000)
  .orderBy('population').limit(2).get();
```

However, if you have a filter with a range comparison (`<`, `<=`, `>`, `>=`), your first ordering must be on the same field, see the list of `orderBy()` limitations below.

## Limitations

Note the following restriction for `orderBy()` clauses:
* An `orderBy()` clause also filters for existence of the given fields. The result set will not include documents that do not contain the given fields.

### `orderBy` and existence

When you order a query by a given field, the query can return only the documents where the order-by field exists.

For example, the following query would not return any documents where the `population` field is not set, even if they otherwise meet the query filters.

```java
db.collection("cities").whereEqualTo("country", "USA").orderBy("population");
```

A related effect applies to inequalities. A query with an inequality filter on a field also implies ordering by that field. The following query does not return documents without a `population` field even if `country = USA` in that document. As a workaround, you can execute separate queries for each ordering or you can assign a value for all fields that you order by.

```java
db.collection("cities").where(or("country", USA"), greaterThan("population", 250000));
```

The query above includes an implied order-by on the inequality and is equivalent to the following:

```java
db.collection("cities").where(or("country", USA"), greaterThan("population", 250000)).orderBy("population");
```

# Query with Range and Inequality Filters on Multiple Fields Overview

Cloud Firestore supports using range and inequality filters on multiple fields in a single query. You can have range and inequality conditions on multiple fields and simplify your application development by delegating implementation of post-filtering logic to Cloud Firestore.

## Range and inequality filters on multiple fields

The following query uses range filters on population and density to return all cities where population is higher than 1,000,000 people and population density is less than 10,000 people per unit of area.

```javascript
db.collection("cities")
  .where('population', '>', 1000000),
  .where('density', '<', 10000)
```

## Indexing considerations

Before you run your queries, read about queries and the Cloud Firestore data model.

In Cloud Firestore, the ORDER BY clause of a query determines which indexes can be used to serve the query. For example, an ORDER BY a ASC, b ASC query requires a composite index on the a ASC, b ASC fields.

To optimize the performance and cost of Cloud Firestore queries, optimize the order of fields in the index. To do this, ensure that your index is ordered from left to right such that the query distills to a dataset that prevents scanning of unnecessary index entries.

Suppose you want to search through a collection of employees and find United States employees whose salary is more than $100,000 and whose number of years of experience is greater than 0. Based on your understanding of the dataset, you know that the salary constraint is more selective than the experience constraint. The ideal index that would reduce the number of index scans would be the (salary [...], experience [...]). Thus, the query that would be fast and cost-efficient would order salary before experience and look as follows:

```javascript
db.collection("employees")
  .where("salary", ">", 100000)
  .where("experience", ">", 0)
  .orderBy("salary")
  .orderBy("experience");
```

## Best practices for optimizing indexes

When optimizing indexes, note the following best practices.

### Order index fields by equalities followed by most selective range or inequality field

Cloud Firestore uses the leftmost fields of a composite index to satisfy the equality constraints and the range or inequality constraint, if any, on the first field of the orderBy() query. These constraints can reduce the number of index entries that Cloud Firestore scans. Cloud Firestore uses the remaining fields of the index to satisfy other range or inequality constraints of the query. These constraints don't reduce the number of index entries that Cloud Firestore scans but filter out unmatched documents so that the number of documents that are returned to the clients are reduced.

For more information about creating efficient indexes, see index properties.

### Order fields in decreasing order of query constraint selectivity

To ensure that Cloud Firestore selects the optimal index for your query, specify an orderBy() clause that orders fields in decreasing order of query constraint selectivity. Higher selectivity matches a smaller subset of documents, while lower selectivity matches a larger subset of documents. Ensure that you select range or inequality fields with higher selectivity earlier in the index ordering than fields with lower selectivity.

To minimize the number of documents that Cloud Firestore scans and returns over the network, you should always order fields in the decreasing order of query constraint selectivity. If the result set is not in the required order and the result set is expected to be small, you can implement client-side logic to reorder it as per your ordering expectation.

For example, suppose you want to search through a collection of employees to find United States employees whose salary is more than $100,000 and order the results by the year of experience of the employee. If you expect only a small number of employees will have salaries greater than $100,000, then the most efficient way to write the query is as follows:

```javascript
const querySnapshot = await db.collection('employees')
                              .where("salary", ">", 100000)
                              .orderBy("salary")
                              .get();

// Order results by `experience`
```

While adding an ordering on experience to the query will yield the same set of documents and obviate re-ordering the results on the clients, the query may read many more extraneous index entries than the earlier query. This is because Cloud Firestore always prefers an index whose index fields prefix match the order by clause of the query. If experience were added to the order by clause, then Cloud Firestore will select the (experience [...], salary [...]) index for computing query results. Since there are no other constraints on experience, Cloud Firestore will read all index entries of the employees collection before applying the salary filter to find the final result set. This means that index entries which don't satisfy the salary filter are still read, thus increasing the latency and cost of the query.

## Pricing

Queries with range and inequality filters on multiple fields are billed based on documents read and index entries read.

For detailed information, see the Pricing page.

## Limitations

Apart from the query limitations, note the following limitations before using queries with range and inequality filters on multiple fields:

* Queries with range or inequality filters on document fields and only equality constraints on the document key (__name__) aren't supported.
* Cloud Firestore limits the number of range or inequality fields to 10. This is to prevent queries from becoming too expensive to run.

# Optimize Queries with Range and Inequality Filters on Multiple Fields

This page provides examples of indexing strategy that you can use for queries with range and inequality filters on multiple fields to create an efficient query experience.

Before you optimize your queries, read about the related concepts.

## Optimize queries with Query Explain

To determine if your query and indexes are optimal, you can use Query Explain to get the query plan summary and execution statistics of the query:

```javascript
let q = db.collection("employees")
      .where("salary", ">", 100000)
      .where("experience", ">",0);

let options = { analyze : 'true' };
let explainResults = await q.explain(options);

let planSummary = explainResults.metrics.planSummary;
let stats = explainResults.metrics.executionStats;

console.log(planSummary);
console.log(stats);
```

The following example shows how the use of correct index ordering reduces the number of index entries that Cloud Firestore scans.

## Simple queries

With the earlier example of a collection of employees, the simple query that runs with the `(experience ASC, salary ASC)` index is as follows:

```java
db.collection("employees")
  .whereGreaterThan("salary", 100000)
  .whereGreaterThan("experience", 0)
  .orderBy("experience")
  .orderBy("salary");
```

The query scans 95000 index entries only to return five documents. Since the query predicate isn't satisfied, a large number of index entries are read but are filtered out.

```
// Output query planning info
{
    "indexesUsed": [
        {
            "properties": "(experience ASC, salary ASC, __name__ ASC)",
            "query_scope": "Collection"
        }
    ],

    // Output Query Execution Stats
    "resultsReturned": "5",
    "executionDuration": "2.5s",
    "readOperations": "100",
    "debugStats": {
        "index_entries_scanned": "95000",
        "documents_scanned": "5",
        "billing_details": {
            "documents_billable": "5",
            "index_entries_billable": "95000",
            "small_ops": "0",
            "min_query_cost": "0"
        }
    }
}
```

You can infer from domain expertise that most employees will have at least some experience but few will have a salary that is more than 100000. Given this insight, you can see that the `salary` constraint is more selective than the `experience` constraint. To influence the index that Cloud Firestore uses to execute the query, specify an `orderBy` clause that orders the `salary` constraint before the `experience` constraint.

```java
db.collection("employees")
  .whereGreaterThan("salary", 100000)
  .whereGreaterThan("experience", 0)
  .orderBy("salary")
  .orderBy("experience");
```

When you explicitly use the `orderBy()` clause to add the predicates, Cloud Firestore uses the `(salary ASC, experience ASC)` index to run the query. Since the selectivity of the first range filter is higher in this query compared to the earlier query, the query runs faster and is more cost efficient.

```
// Output query planning info
{
    "indexesUsed": [
        {
            "properties": "(salary ASC, experience ASC, __name__ ASC)",
            "query_scope": "Collection"
        }
    ],

    // Output Query Execution Stats
    "resultsReturned": "5",
    "executionDuration": "0.2s",
    "readOperations": "6",
    "debugStats": {
        "index_entries_scanned": "1000",
        "documents_scanned": "5",
        "billing_details": {
            "documents_billable": "5",
            "index_entries_billable": "1000",
            "small_ops": "0",
            "min_query_cost": "0"
        }
    }
}
```

# Summarize Data with Aggregation Queries

An aggregation query processes the data from multiple index entries to return a single summary value.

Cloud Firestore supports the following aggregation queries:
- `count()`
- `sum()`
- `average()`

Cloud Firestore calculates the aggregation and transmits only the result back to your application. Compared to executing a full query and calculating the aggregation in your app, aggregation queries save on both billed document reads and bytes transferred.

Aggregation queries rely on the existing index configuration that your queries already use, and scale proportionally to the number of index entries scanned. Latency increases with the number of items in the aggregation.

**Note:** While the code samples cover multiple languages, the text explaining the samples refers to the Web method names.

## Use the count() aggregation

The `count()` aggregation query lets you determine the number of documents in a collection or query.

For more information about the example data, see Getting data.

The following `count()` aggregation returns the total number of cities in the cities collection.

```javascript
const collectionRef = db.collection('cities');
const snapshot = await collectionRef.count().get();
console.log(snapshot.data().count);
```

The `count()` aggregation takes into account any filters on the query and any limit clauses.

```javascript
const collectionRef = db.collection('cities');
const query = collectionRef.where('state', '==', 'CA');
const snapshot = await query.count().get();
console.log(snapshot.data().count);
```

## Use the sum() aggregation

Use the `sum()` aggregation to return the total sum of numeric values that match a given query—for example:

```javascript
const coll = firestore.collection('cities');
const sumAggregateQuery = coll.aggregate({
         totalPopulation: AggregateField.sum('population'),
       });

const snapshot = await sumAggregateQuery.get();
console.log('totalPopulation: ', snapshot.data().totalPopulation);
```

The `sum()` aggregation takes into account any filters on the query and any limit clauses—for example:

```javascript
const coll = firestore.collection('cities');
const q = coll.where("capital", "==", true);
const sumAggregateQuery = q.aggregate({
        totalPopulation: AggregateField.sum('population'),
      });

const snapshot = await sumAggregateQuery.get();
console.log('totalPopulation: ', snapshot.data().totalPopulation);
```

## Use the average() aggregation

Use the `average()` aggregation to return the average of numeric values that match a given query, for example:

```javascript
const coll = firestore.collection('cities');
const averageAggregateQuery = coll.aggregate({
        averagePopulation: AggregateField.average('population'),
      });

const snapshot = await averageAggregateQuery.get();
console.log('averagePopulation: ', snapshot.data().averagePopulation);
```

The `average()` aggregation takes into account any filters on the query and any limit clauses, for example:

```javascript
const coll = firestore.collection('cities');
const q = coll.where("capital", "==", true);
const averageAggregateQuery = q.aggregate({
        averagePopulation: AggregateField.average('population'),
      });

const snapshot = await averageAggregateQuery.get();
console.log('averagePopulation: ', snapshot.data().averagePopulation);
```

## Calculate multiple aggregations in a query

You can combine multiple aggregations in a single aggregation pipeline. This can reduce the number of index reads required. If the query includes aggregations on multiple fields, the query might requires a composite index. In that case, Cloud Firestore suggests an index.

The following example performs multiple aggregations in a single aggregation query:

```javascript
const coll = firestore.collection('cities');
const aggregateQuery = coll.aggregate({
    countOfDocs: AggregateField.count(),
    totalPopulation: AggregateField.sum('population'),
    averagePopulation: AggregateField.average('population')
  });

const snapshot = await aggregateQuery.get();
console.log('countOfDocs: ', snapshot.data().countOfDocs);
console.log('totalPopulation: ', snapshot.data().totalPopulation);
console.log('averagePopulation: ', snapshot.data().averagePopulation);
```

Queries with multiple aggregations include only the documents that contain all the fields in each aggregation. This might lead to different results from performing each aggregation separately.

## Security rules for aggregation queries

Cloud Firestore Security Rules work the same on aggregation queries as on queries that return documents. In other words, if and only if your rules allow clients to execute certain collection or collection group queries, clients can also perform the aggregation on those queries. Learn more about how Cloud Firestore Security Rules interact with queries.

## Behavior and limitations

As you work with aggregation queries, note the following behavior and limitations:

- You can't use aggregation queries with real-time listeners and offline queries. Aggregation queries are only supported through a direct server response. Queries are served only by the Cloud Firestore backend, skipping the local cache and any buffered updates. This behavior is identical to operations that are performed inside Cloud Firestore transactions.

- If an aggregation can't resolve within 60 seconds, it returns a DEADLINE_EXCEEDED error. Performance depends on your index configuration and on the size of the dataset.

**Note:** Most queries scale based on the on the size of the result set, not the dataset. However, aggregation queries scale based on the size of the dataset and the number of index entries scanned.

- If the operation can't be completed within the 60 second deadline, a possible workaround is to use counters for large datasets.

- Aggregation queries read from index entries and include only indexed fields.

- Adding an OrderBy clause to an aggregation query limits the aggregation to the documents where the sorting field exists.

- For `sum()` and `average()` aggregations, non-numeric values are ignored. `sum()` and `average()` aggregations take into account only integer values and floating-point number values.

- When combining multiple aggregations in a single query, note that `sum()` and `average()` ignore non-numeric values while `count()` includes non-numeric values.

- If you combine aggregations that are on different fields, the calculation includes only the documents that contain all those fields.

## Pricing

Pricing for aggregation queries depends on the number of index entries that the query matches. You are charged a small number of reads for a large number of matched entries. You are charged one read operation for each batch of up to 1000 index entries read.

For more information about aggregation queries pricing, see Aggregation queries.

# Paginate Data with Query Cursors

With query cursors in Cloud Firestore, you can split data returned by a query into batches according to the parameters you define in your query.

Query cursors define the start and end points for a query, allowing you to:
- Return a subset of the data.
- Paginate query results.

However, to define a specific range for a query, you should use the `where()` method described in Simple Queries.

## Add a simple cursor to a query

Use the `startAt()` or `startAfter()` methods to define the start point for a query. The `startAt()` method includes the start point, while the `startAfter()` method excludes it.

For example, if you use `startAt(A)` in a query, it returns the entire alphabet. If you use `startAfter(A)` instead, it returns B-Z.

```javascript
const startAtRes = await db.collection('cities')
  .orderBy('population')
  .startAt(1000000)
  .get();
```

Similarly, use the `endAt()` or `endBefore()` methods to define an end point for your query results.

```javascript
const endAtRes = await db.collection('cities')
  .orderBy('population')
  .endAt(1000000)
  .get();
```

## Use a document snapshot to define the query cursor

You can also pass a document snapshot to the cursor clause as the start or end point of the query cursor. The values in the document snapshot serve as the values in the query cursor.

For example, take a snapshot of a "San Francisco" document in your data set of cities and populations. Then, use that document snapshot as the start point for your population query cursor. Your query will return all the cities with a population larger than or equal to San Francisco's, as defined in the document snapshot.

```javascript
const docRef = db.collection('cities').doc('SF');
const snapshot = await docRef.get();
const startAtSnapshot = db.collection('cities')
  .orderBy('population')
  .startAt(snapshot);

await startAtSnapshot.limit(10).get();
```

## Paginate a query

Paginate queries by combining query cursors with the `limit()` method. For example, use the last document in a batch as the start of a cursor for the next batch.

```javascript
const first = db.collection('cities')
  .orderBy('population')
  .limit(3);

const snapshot = await first.get();

// Get the last document
const last = snapshot.docs[snapshot.docs.length - 1];

// Construct a new query starting at this document.
// Note: this will not have the desired effect if multiple
// cities have the exact same population value.
const next = db.collection('cities')
  .orderBy('population')
  .startAfter(last.data().population)
  .limit(3);

// Use the query for pagination
// ...
```

## Set cursor based on multiple fields

When using a cursor based on a field value (not a DocumentSnapshot), you can make the cursor position more precise by adding additional fields. This is particularly useful if your data set includes multiple documents that all have the same value for your cursor field, making the cursor's position ambiguous. You can add additional field values to your cursor to further specify the start or end point and reduce ambiguity.

For example, in a data set containing all the cities named "Springfield" in the United States, there would be multiple start points for a query set to start at "Springfield":

### Cities
| Name | State |
|------|-------|
| Springfield | Massachusetts |
| Springfield | Missouri |
| Springfield | Wisconsin |

To start at a specific Springfield, you could add the state as a secondary condition in your cursor clause.

```javascript
// Will return all Springfields
const startAtNameRes = await db.collection('cities')
  .orderBy('name')
  .orderBy('state')
  .startAt('Springfield')
  .get();

// Will return 'Springfield, Missouri' and 'Springfield, Wisconsin'
const startAtNameAndStateRes = await db.collection('cities')
  .orderBy('name')
  .orderBy('state')
  .startAt('Springfield', 'Missouri')
  .get();
```

# Paginate Data with Query Cursors

With query cursors in Cloud Firestore, you can split data returned by a query into batches according to the parameters you define in your query.

Query cursors define the start and end points for a query, allowing you to:
- Return a subset of the data.
- Paginate query results.

However, to define a specific range for a query, you should use the `where()` method described in Simple Queries.

## Add a simple cursor to a query

Use the `startAt()` or `startAfter()` methods to define the start point for a query. The `startAt()` method includes the start point, while the `startAfter()` method excludes it.

For example, if you use `startAt(A)` in a query, it returns the entire alphabet. If you use `startAfter(A)` instead, it returns B-Z.

```javascript
const startAtRes = await db.collection('cities')
  .orderBy('population')
  .startAt(1000000)
  .get();
```

Similarly, use the `endAt()` or `endBefore()` methods to define an end point for your query results.

```javascript
const endAtRes = await db.collection('cities')
  .orderBy('population')
  .endAt(1000000)
  .get();
```

## Use a document snapshot to define the query cursor

You can also pass a document snapshot to the cursor clause as the start or end point of the query cursor. The values in the document snapshot serve as the values in the query cursor.

For example, take a snapshot of a "San Francisco" document in your data set of cities and populations. Then, use that document snapshot as the start point for your population query cursor. Your query will return all the cities with a population larger than or equal to San Francisco's, as defined in the document snapshot.

```javascript
const docRef = db.collection('cities').doc('SF');
const snapshot = await docRef.get();
const startAtSnapshot = db.collection('cities')
  .orderBy('population')
  .startAt(snapshot);

await startAtSnapshot.limit(10).get();
```

## Paginate a query

Paginate queries by combining query cursors with the `limit()` method. For example, use the last document in a batch as the start of a cursor for the next batch.

```javascript
const first = db.collection('cities')
  .orderBy('population')
  .limit(3);

const snapshot = await first.get();

// Get the last document
const last = snapshot.docs[snapshot.docs.length - 1];

// Construct a new query starting at this document.
// Note: this will not have the desired effect if multiple
// cities have the exact same population value.
const next = db.collection('cities')
  .orderBy('population')
  .startAfter(last.data().population)
  .limit(3);

// Use the query for pagination
// ...
```

## Set cursor based on multiple fields

When using a cursor based on a field value (not a DocumentSnapshot), you can make the cursor position more precise by adding additional fields. This is particularly useful if your data set includes multiple documents that all have the same value for your cursor field, making the cursor's position ambiguous. You can add additional field values to your cursor to further specify the start or end point and reduce ambiguity.

For example, in a data set containing all the cities named "Springfield" in the United States, there would be multiple start points for a query set to start at "Springfield":

### Cities
| Name | State |
|------|-------|
| Springfield | Massachusetts |
| Springfield | Missouri |
| Springfield | Wisconsin |

To start at a specific Springfield, you could add the state as a secondary condition in your cursor clause.

```javascript
// Will return all Springfields
const startAtNameRes = await db.collection('cities')
  .orderBy('name')
  .orderBy('state')
  .startAt('Springfield')
  .get();

// Will return 'Springfield, Missouri' and 'Springfield, Wisconsin'
const startAtNameAndStateRes = await db.collection('cities')
  .orderBy('name')
  .orderBy('state')
  .startAt('Springfield', 'Missouri')
  .get();
```

# Search with Vector Embeddings

The page shows you how to use Cloud Firestore to perform K-nearest neighbor (KNN) vector searches using the following techniques:

- Store vector values
- Create and manage KNN vector indexes
- Make a K-nearest-neighbor (KNN) query using one of the supported vector distance measures

## Store Vector Embeddings

You can create vector values such as text embeddings from your Cloud Firestore data, and store them in Cloud Firestore documents.

### Write Operation with a Vector Embedding

The following example shows how to store a vector embedding in a Cloud Firestore document:

```javascript
import {
  Firestore,
  FieldValue,
} from "@google-cloud/firestore";

const db = new Firestore();
const coll = db.collection('coffee-beans');
await coll.add({
  name: "Kahawa coffee beans",
  description: "Information about the Kahawa coffee beans.",
  embedding_field: FieldValue.vector([1.0 , 2.0, 3.0])
});
```

### Compute Vector Embeddings with a Cloud Function

To calculate and store vector embeddings whenever a document is updated or created, you can set up a Cloud Function:

```javascript
/**
 * A vector embedding will be computed from the
 * value of the `content` field. The vector value
 * will be stored in the `embedding` field. The
 * field names `content` and `embedding` are arbitrary
 * field names chosen for this example.
 */
async function storeEmbedding(event: FirestoreEvent<any>): Promise<void> {
  // Get the previous value of the document's `content` field.
  const previousDocumentSnapshot = event.data.before as QueryDocumentSnapshot;
  const previousContent = previousDocumentSnapshot.get("content");

  // Get the current value of the document's `content` field.
  const currentDocumentSnapshot = event.data.after as QueryDocumentSnapshot;
  const currentContent = currentDocumentSnapshot.get("content");

  // Don't update the embedding if the content field did not change
  if (previousContent === currentContent) {
    return;
  }

  // Call a function to calculate the embedding for the value
  // of the `content` field.
  const embeddingVector = calculateEmbedding(currentContent);

  // Update the `embedding` field on the document.
  await currentDocumentSnapshot.ref.update({
    embedding: embeddingVector,
  });
}
```

## Create and Manage Vector Indexes

Before you can perform a nearest neighbor search with your vector embeddings, you must create a corresponding index. The following examples demonstrate how to create and manage vector indexes with the Google Cloud CLI. Vector indexes can also be managed with the Firebase CLI and Terraform.

### Create a Vector Index

Before you create a vector index, upgrade to the latest version of the Google Cloud CLI:

```bash
gcloud components update
```

To create a vector index, use gcloud firestore indexes composite create:

```bash
gcloud firestore indexes composite create \
--collection-group=collection-group \
--query-scope=COLLECTION \
--field-config field-path=vector-field,vector-config='vector-configuration' \
--database=database-id
```

where:

- collection-group is the ID of the collection group.
- vector-field is the name of the field that contains the vector embedding.
- database-id is the ID of the database.
- vector-configuration includes the vector dimension and index type. The dimension is an integer up to 2048. The index type must be flat. Format the index configuration as follows: `{"dimension":"DIMENSION", "flat": "{}"}`.

The following example creates a composite index, including a vector index for field vector-field and an ascending index for field color. You can use this type of index to pre-filter data before a nearest neighbor search.

```bash
gcloud firestore indexes composite create \
--collection-group=collection-group \
--query-scope=COLLECTION \
--field-config=order=ASCENDING,field-path="color" \
--field-config field-path=vector-field,vector-config='{"dimension":"1024", "flat": "{}"}' \
--database=database-id
```

### List All Vector Indexes

```bash
gcloud firestore indexes composite list --database=database-id
```

Replace database-id with the ID of the database.

### Delete a Vector Index

```bash
gcloud firestore indexes composite delete index-id --database=database-id
```

where:

- index-id is the ID of the index to delete. Use indexes composite list to retrieve the index ID.
- database-id is the ID of the database.

### Describe a Vector Index

```bash
gcloud firestore indexes composite describe index-id --database=database-id
```

where:

- index-id is the ID of the index to describe. Use or indexes composite list to retrieve the index ID.
- database-id is the ID of the database.

## Make a Nearest-Neighbor Query

You can perform a similarity search to find the nearest neighbors of a vector embedding. Similarity searches require vector indexes. If an index doesn't exist, Cloud Firestore suggests an index to create using the gcloud CLI.

The following example finds 10 nearest neighbors of the query vector.

```javascript
import {
  Firestore,
  FieldValue,
  VectorQuery,
  VectorQuerySnapshot,
} from "@google-cloud/firestore";

// Requires a single-field vector index
const vectorQuery: VectorQuery = coll.findNearest({
  vectorField: 'embedding_field',
  queryVector: [3.0, 1.0, 2.0],
  limit: 10,
  distanceMeasure: 'EUCLIDEAN'
});

const vectorQuerySnapshot: VectorQuerySnapshot = await vectorQuery.get();
```

### Vector Distances

Nearest-neighbor queries support the following options for vector distance:

- **EUCLIDEAN**: Measures the EUCLIDEAN distance between the vectors. To learn more, see Euclidean.
- **COSINE**: Compares vectors based on the angle between them which lets you measure similarity that isn't based on the vectors magnitude. We recommend using DOT_PRODUCT with unit normalized vectors instead of COSINE distance, which is mathematically equivalent with better performance. To learn more see Cosine similarity to learn more.
- **DOT_PRODUCT**: Similar to COSINE but is affected by the magnitude of the vectors. To learn more, see Dot product.

### Choose the Distance Measure

Depending on whether or not all your vector embeddings are normalized, you can determine which distance measure to use to find the distance measure. A normalized vector embedding has a magnitude (length) of exactly 1.0.

In addition, if you know which distance measure your model was trained with, use that distance measure to compute the distance between your vector embeddings.

#### Normalized Data

If you have a dataset where all vector embeddings are normalized, then all three distance measures provide the same semantic search results. In essence, although each distance measure returns a different value, those values sort the same way. When embeddings are normalized, DOT_PRODUCT is usually the most computationally efficient, but the difference is negligible in most cases. However, if your application is highly performance sensitive, DOT_PRODUCT might help with performance tuning.

#### Non-normalized Data

If you have a dataset where vector embeddings aren't normalized, then it's not mathematically correct to use DOT_PRODUCT as a distance measure because dot product doesn't measure distance. Depending on how the embeddings were generated and what type of search is preferred, either the COSINE or EUCLIDEAN distance measure produces search results that are subjectively better than the other distance measures. Experimentation with either COSINE or EUCLIDEAN might be necessary to determine which is best for your use case.

#### Unsure if Data is Normalized or Non-normalized

If you're unsure whether or not your data is normalized and you want to use DOT_PRODUCT, we recommend that you use COSINE instead. COSINE is like DOT_PRODUCT with normalization built in. Distance measured using COSINE ranges from 0 to 2. A result that is close to 0 indicates the vectors are very similar.

### Pre-filter Documents

To pre-filter documents before finding the nearest neighbors, you can combine a similarity search with other query operators. The and and or composite filters are supported. For more information about supported field filters, see Query operators.

```javascript
// Similarity search with pre-filter
// Requires composite vector index
const preFilteredVectorQuery: VectorQuery = coll
    .where("color", "==", "red")
    .findNearest({
      vectorField: "embedding_field",
      queryVector: [3.0, 1.0, 2.0],
      limit: 5,
      distanceMeasure: "EUCLIDEAN",
    });

const vectorQueryResults = await preFilteredVectorQuery.get();
```

### Retrieve the Calculated Vector Distance

You can retrieve the calculated vector distance by assigning a distance_result_field output property name on the FindNearest query, as shown in the following example:

```javascript
const vectorQuery: VectorQuery = coll.findNearest(
    {
      vectorField: 'embedding_field',
      queryVector: [3.0, 1.0, 2.0],
      limit: 10,
      distanceMeasure: 'EUCLIDEAN',
      distanceResultField: 'vector_distance'
    });

const snapshot: VectorQuerySnapshot = await vectorQuery.get();

snapshot.forEach((doc) => {
  console.log(doc.id, ' Distance: ', doc.get('vector_distance'));
});
```

If you want to use a field mask to return a subset of document fields along with a distanceResultField, then you must also include the value of distanceResultField in the field mask, as shown in the following example:

```javascript
const vectorQuery: VectorQuery = coll
    .select('name', 'description', 'vector_distance')
    .findNearest({
      vectorField: 'embedding_field',
      queryVector: [3.0, 1.0, 2.0],
      limit: 10,
      distanceMeasure: 'EUCLIDEAN',
      distanceResultField: 'vector_distance'
    });
```

### Specify a Distance Threshold

You can specify a similarity threshold that returns only documents within the threshold. The behavior of the threshold field depends on the distance measure you choose:

- EUCLIDEAN and COSINE distances limit the threshold to documents where distance is less than or equal to the specified threshold. These distance measures decrease as the vectors become more similar.
- DOT_PRODUCT distance limits the threshold to documents where distance is greater than or equal to the specified threshold. Dot product distances increase as the vectors become more similar.

The following example shows how to specify a distance threshold to return up to 10 nearest documents that are, at most, 4.5 units away using the EUCLIDEAN distance metric:

```javascript
const vectorQuery: VectorQuery = coll.findNearest({
  vectorField: 'embedding_field',
  queryVector: [3.0, 1.0, 2.0],
  limit: 10,
  distanceMeasure: 'EUCLIDEAN',
  distanceThreshold: 4.5
});

const snapshot: VectorQuerySnapshot = await vectorQuery.get();

snapshot.forEach((doc) => {
  console.log(doc.id);
});
```

## Limitations

As you work with vector embeddings, note the following limitations:

- The maximum supported embedding dimension is 2048. To store larger indexes, use dimensionality reduction.
- The maximum number of documents to return from a nearest-neighbor query is 1000.
- Vector search does not support real-time snapshot listeners.
- Only the Python, Node.js, Go, and Java client libraries support vector search.

# Choose a Data Structure

Remember, when you structure your data in Cloud Firestore, you have a few different options:
* Documents
* Multiple collections
* Subcollections within documents

Consider the advantages of each option as they relate to your use case. A few example structures for hierarchical data are outlined in this guide.

## Nested Data in Documents

You can nest complex objects like arrays or maps within documents.
* **Advantages:** If you have simple, fixed lists of data that you want to keep within your documents, this is easy to set up and streamlines your data structure.
* **Limitations:** This isn't as scalable as other options, especially if your data expands over time. With larger or growing lists, the document also grows, which can lead to slower document retrieval times.
* **What's a possible use case?** In a chat app, for example, you might store a user's 3 most recently visited chat rooms as a nested list in their profile.
```
class alovelace
    name : 
      first : "Ada"
      last : "Lovelace"
    born : 1815
    rooms : 
      0 : "Software Chat"
      1 : "Famous Figures"
      2 : "Famous SWEs"
```

## Subcollections

You can create collections within documents when you have data that might expand over time.
* **Advantages:** As your lists grow, the size of the parent document doesn't change. You also get full query capabilities on subcollections, and you can issue collection group queries across subcollections.
* **Limitations:** You can't easily delete subcollections.
* **What's a possible use case?** In the same chat app, for example, you might create collections of users or messages within chat room documents.
```
collections_bookmark science
  class software
    name : "software chat"
    collections_bookmark users
      class alovelace
        first : "Ada"
        last : "Lovelace"
      class sride
        first : "Sally"
        last : "Ride"

  class astrophysics
    ...
```

## Root-level Collections

Create collections at the root level of your database to organize disparate data sets.
* **Advantages:** Root-level collections are good for many-to-many relationships and provide powerful querying within each collection.
* **Limitations:** Getting data that is naturally hierarchical might become increasingly complex as your database grows.
* **What's a possible use case?** In the same chat app, for example, you might create one collection for users and another for rooms and messages.
```
collections_bookmark users
  class alovelace
    first : "Ada"
    last : "Lovelace"
    born : 1815
  class sride
    first : "Sally"
    last : "Ride"
    born : 1951

collections_bookmark rooms
  class software
    collections_bookmark messages
      class message1
        from : "alovelace"
        content : "..."
      class message2
        from : "sride"
        content : "..."
```
# Add Data to Cloud Firestore

This document explains how to set, add, or update individual documents in Cloud Firestore. To write data in bulk, see Transactions and batched writes.

## Overview

You can write data to Cloud Firestore in one of the following ways:

* Set the data of a document within a collection, explicitly specifying a document identifier.
* Add a new document to a collection. In this case, Cloud Firestore automatically generates the document identifier.
* Create an empty document with an automatically generated identifier, and assign data to it later.

Note: While the code samples cover multiple languages, the text explaining the samples refers to the Web method names.

## Before You Begin

Before you can initialize Cloud Firestore to set, add, or update data, you must complete the following steps:

* Create a Cloud Firestore database. For more information, see Get started with Cloud Firestore
* If you use the web or mobile client libraries, authenticate with security rules. For more information, see Getting started with security rules.
* If you use the server client libraries or REST API, authenticate with Identity and Access Management (IAM). For more information, see Security for server client libraries.

## Initialize Cloud Firestore

Initialize an instance of Cloud Firestore:

### Initialize on Cloud Functions

```javascript
const { initializeApp, applicationDefault, cert } = require('firebase-admin/app');
const { getFirestore, Timestamp, FieldValue, Filter } = require('firebase-admin/firestore');

initializeApp();

const db = getFirestore();
```

### Initialize on Google Cloud

```javascript
const { initializeApp, applicationDefault, cert } = require('firebase-admin/app');
const { getFirestore, Timestamp, FieldValue, Filter } = require('firebase-admin/firestore');

initializeApp({
  credential: applicationDefault()
});

const db = getFirestore();
```

### Initialize on Your Own Server

To use the Firebase Admin SDK on your own server (or any other Node.js environment), use a service account. Go to IAM & admin > Service accounts in the Google Cloud console. Generate a new private key and save the JSON file. Then use the file to initialize the SDK:

```javascript
const { initializeApp, applicationDefault, cert } = require('firebase-admin/app');
const { getFirestore, Timestamp, FieldValue, Filter } = require('firebase-admin/firestore');

const serviceAccount = require('./path/to/serviceAccountKey.json');

initializeApp({
  credential: cert(serviceAccount)
});

const db = getFirestore();
```

## Set a Document

To create or overwrite a single document, use the following language-specific set() methods:

Use the set() method:

```javascript
const data = {
  name: 'Los Angeles',
  state: 'CA',
  country: 'USA'
};

// Add a new document in collection "cities" with ID 'LA'
const res = await db.collection('cities').doc('LA').set(data);
```

If the document does not exist, it will be created. If the document does exist, its contents will be overwritten with the newly provided data, unless you specify that the data should be merged into the existing document, as follows:

```javascript
const cityRef = db.collection('cities').doc('BJ');

const res = await cityRef.set({
  capital: true
}, { merge: true });
```

If you're not sure whether the document exists, pass the option to merge the new data with any existing document to avoid overwriting entire documents. For documents that contain maps, if you specify a set with a field that contains an empty map, the map field of the target document is overwritten.

## Data Types

Cloud Firestore lets you write a variety of data types inside a document, including strings, booleans, numbers, dates, null, and nested arrays and objects. Cloud Firestore always stores numbers as doubles, regardless of what type of number you use in your code.

```javascript
const data = {
  stringExample: 'Hello, World!',
  booleanExample: true,
  numberExample: 3.14159265,
  dateExample: Timestamp.fromDate(new Date('December 10, 1815')),
  arrayExample: [5, true, 'hello'],
  nullExample: null,
  objectExample: {
    a: 5,
    b: true
  }
};

const res = await db.collection('data').doc('one').set(data);
```

## Custom Objects

Using Map or Dictionary objects to represent your documents is often inconvenient, so Cloud Firestore supports writing documents with custom classes. Cloud Firestore converts the objects to supported data types.

// Node.js uses JavaScript objects

## Add a Document

When you use set() to create a document, you must specify an ID for the document to create, as shown in the following example:

```javascript
await db.collection('cities').doc('new-city-id').set(data);
```

If there isn't a meaningful ID for the document, Cloud Firestore can auto-generate an ID for you. You can call the following language-specific add() methods:

Use the add() method:

```javascript
// Add a new document with a generated id.
const res = await db.collection('cities').add({
  name: 'Tokyo',
  country: 'Japan'
});

console.log('Added document with ID: ', res.id);
```

Important: Unlike "push IDs" in the Firebase Realtime Database, Cloud Firestore auto-generated IDs don't provide any automatic ordering. If you want to order your documents by creation date, store a timestamp as a field in the documents.

In some cases, it can be useful to create a document reference with an auto-generated ID, then use the reference later. For this use case, you can call doc() in the following way:

```javascript
const newCityRef = db.collection('cities').doc();

// Later...
const res = await newCityRef.set({
  // ...
});
```

In the backend, .add(...) and .doc().set(...) are equivalent, so you can use either option.

## Update a Document

To update some fields of a document without overwriting the entire document, use the following language-specific update() methods:

Use the update() method:

```javascript
const cityRef = db.collection('cities').doc('DC');

// Set the 'capital' field of the city
const res = await cityRef.update({capital: true});
```

## Server Timestamp

You can set a field in your document to a server timestamp which tracks when the server receives the update.

```javascript
// Create a document reference
const docRef = db.collection('objects').doc('some-id');

// Update the timestamp field with the value from the server
const res = await docRef.update({
  timestamp: FieldValue.serverTimestamp()
});
```

When updating multiple timestamp fields inside of a transaction, each field receives the same server timestamp value.

## Update Fields in Nested Objects

If your document contains nested objects, you can use the dot notation to reference nested fields within the document when you call update():

```javascript
const initialData = {
  name: 'Frank',
  age: 12,
  favorites: {
    food: 'Pizza',
    color: 'Blue',
    subject: 'recess'
  }
};

// ...
const res = await db.collection('users').doc('Frank').update({
  age: 13,
  'favorites.color': 'Red'
});
```

Dot notation allows you to update a single nested field without overwriting other nested fields. If you update a nested field without dot notation, you will overwrite the entire map field, as shown in the following example:

```javascript
// Create our initial doc
db.collection("users").doc("frank").set({
  name: "Frank",
  favorites: {
    food: "Pizza",
    color: "Blue",
    subject: "Recess"
  },
  age: 12
}).then(function() {
  console.log("Frank created");
});

// Update the doc without using dot notation.
// Notice the map value for favorites.
db.collection("users").doc("frank").update({
  favorites: {
    food: "Ice Cream"
  }
}).then(function() {
  console.log("Frank food updated");
});

/*
Ending State, favorite.color and favorite.subject are no longer present:
/users
    /frank
        {
            name: "Frank",
            favorites: {
                food: "Ice Cream",
            },
            age: 12
        }
 */
```

## Update Elements in an Array

If your document contains an array field, you can use arrayUnion() and arrayRemove() to add and remove elements. arrayUnion() adds elements to an array but only elements not already present. arrayRemove() removes all instances of each given element.

```javascript
// ...
const washingtonRef = db.collection('cities').doc('DC');

// Atomically add a new region to the "regions" array field.
const unionRes = await washingtonRef.update({
  regions: FieldValue.arrayUnion('greater_virginia')
});
// Atomically remove a region from the "regions" array field.
const removeRes = await washingtonRef.update({
  regions: FieldValue.arrayRemove('east_coast')
});

// To add or remove multiple items, pass multiple arguments to arrayUnion/arrayRemove
const multipleUnionRes = await washingtonRef.update({
  regions: FieldValue.arrayUnion('south_carolina', 'texas')
  // Alternatively, you can use spread operator in ES6 syntax
  // const newRegions = ['south_carolina', 'texas']
  // regions: FieldValue.arrayUnion(...newRegions)
});
```

## Increment a Numeric Value

You can increment or decrement a numeric field value as shown in the following example. An increment operation increases or decreases the current value of a field by the given amount.

Note: If the field doesn't exist or if the current field value is not a numeric value, the operation sets the field to the given value.

```javascript
// ...
const washingtonRef = db.collection('cities').doc('DC');

// Atomically increment the population of the city by 50.
const res = await washingtonRef.update({
  population: FieldValue.increment(50)
});
```

Increment operations are useful for implementing counters. Note that updating a single document too quickly can lead to contention or errors. If you need to update your counter at a very high rate, see the Distributed counters page.

# Transactions and Batched Writes

Cloud Firestore supports atomic operations for reading and writing data. In a set of atomic operations, either all of the operations succeed, or none of them are applied. There are two types of atomic operations in Cloud Firestore:

* **Transactions**: a transaction is a set of read and write operations on one or more documents.
* **Batched Writes**: a batched write is a set of write operations on one or more documents.

## Updating Data with Transactions

Using the Cloud Firestore client libraries, you can group multiple operations into a single transaction. Transactions are useful when you want to update a field's value based on its current value, or the value of some other field.

A transaction consists of any number of get() operations followed by any number of write operations such as set(), update(), or delete(). In the case of a concurrent edit, Cloud Firestore runs the entire transaction again. For example, if a transaction reads documents and another client modifies any of those documents, Cloud Firestore retries the transaction. This feature ensures that the transaction runs on up-to-date and consistent data.

Transactions never partially apply writes. All writes execute at the end of a successful transaction.

When using transactions, note that:

* Read operations must come before write operations.
* A function calling a transaction (transaction function) might run more than once if a concurrent edit affects a document that the transaction reads.
* Transaction functions should not directly modify application state.
* Transactions will fail when the client is offline.

The following example shows how to create and run a transaction:

```javascript
// Initialize document
const cityRef = db.collection('cities').doc('SF');
await cityRef.set({
  name: 'San Francisco',
  state: 'CA',
  country: 'USA',
  capital: false,
  population: 860000
});

try {
  await db.runTransaction(async (t) => {
    const doc = await t.get(cityRef);

    // Add one person to the city population.
    // Note: this could be done without a transaction
    //       by updating the population using FieldValue.increment()
    const newPopulation = doc.data().population + 1;
    t.update(cityRef, {population: newPopulation});
  });

  console.log('Transaction success!');
} catch (e) {
  console.log('Transaction failure:', e);
}
```

## Passing Information Out of Transactions

Do not modify application state inside of your transaction functions. Doing so will introduce concurrency issues, because transaction functions can run multiple times and are not guaranteed to run on the UI thread. Instead, pass information you need out of your transaction functions. The following example builds on the previous example to show how to pass information out of a transaction:

```javascript
const cityRef = db.collection('cities').doc('SF');
try {
  const res = await db.runTransaction(async t => {
    const doc = await t.get(cityRef);
    const newPopulation = doc.data().population + 1;
    if (newPopulation <= 1000000) {
      await t.update(cityRef, { population: newPopulation });
      return `Population increased to ${newPopulation}`;
    } else {
      throw 'Sorry! Population is too big.';
    }
  });
  console.log('Transaction success', res);
} catch (e) {
  console.log('Transaction failure:', e);
}
```

## Transaction Failure

A transaction can fail for the following reasons:

* The transaction contains read operations after write operations. Read operations must always come before any write operations.
* The transaction read a document that was modified outside of the transaction. In this case, the transaction automatically runs again. The transaction is retried a finite number of times.
* The transaction exceeded the maximum request size of 10 MiB.

Transaction size depends on the sizes of documents and index entries modified by the transaction. For a delete operation, this includes the size of the target document and the sizes of the index entries deleted in response to the operation.

A failed transaction returns an error and does not write anything to the database. You do not need to roll back the transaction; Cloud Firestore does this automatically.

## Batched Writes

If you do not need to read any documents in your operation set, you can execute multiple write operations as a single batch that contains any combination of set(), update(), or delete() operations. Each operation in the batch counts separately towards your Cloud Firestore usage. A batch of writes completes atomically and can write to multiple documents. The following example shows how to build and commit a write batch:

```javascript
// Get a new write batch
const batch = db.batch();

// Set the value of 'NYC'
const nycRef = db.collection('cities').doc('NYC');
batch.set(nycRef, {name: 'New York City'});

// Update the population of 'SF'
const sfRef = db.collection('cities').doc('SF');
batch.update(sfRef, {population: 1000000});

// Delete the city 'LA'
const laRef = db.collection('cities').doc('LA');
batch.delete(laRef);

// Commit the batch
await batch.commit();
```

Like transactions, batched writes are atomic. Unlike transactions, batched writes do not need to ensure that read documents remain un-modified which leads to fewer failure cases. They are not subject to retries or to failures from too many retries. Batched writes execute even when the user's device is offline.

A batched write with hundreds of documents might require many index updates and might exceed the limit on transaction size. In this case, reduce the number of documents per batch. To write a large number of documents, consider using a bulk writer or parallelized individual writes instead.

Note: For bulk data entry, use a server client library with parallelized individual writes. Batched writes perform better than serialized writes but not better than parallel writes. You should use a server client library for bulk data operations and not a mobile/web SDK.

## Data Validation for Atomic Operations

For mobile/web client libraries, you can validate data using Cloud Firestore Security Rules. You can ensure that related documents are always updated atomically and always as part of a transaction or batched write. Use the getAfter() security rule function to access and validate the state of a document after a set of operations completes but before Cloud Firestore commits the operations.

For example, imagine that the database for the cities example also contains a countries collection. Each country document uses a last_updated field to keep track of the last time any city related to that country was updated. The following security rules require that an update to a city document must also atomically update the related country's last_updated field:

```
service cloud.firestore {
  match /databases/{database}/documents {
    // If you update a city doc, you must also
    // update the related country's last_updated field.
    match /cities/{city} {
      allow write: if request.auth != null &&
        getAfter(
          /databases/$(database)/documents/countries/$(request.resource.data.country)
        ).data.last_updated == request.time;
    }

    match /countries/{country} {
      allow write: if request.auth != null;
    }
  }
}
```

## Security Rules Limits

In security rules for transactions or batched writes, there is a limit of 20 document access calls for the entire atomic operation in addition to the normal 10 call limit for each single document operation in the batch.

For example, consider the following rules for a chat application:

```
service cloud.firestore {
  match /databases/{db}/documents {
    function prefix() {
      return /databases/{db}/documents;
    }
    match /chatroom/{roomId} {
      allow read, write: if request.auth != null && roomId in get(/$(prefix())/users/$(request.auth.uid)).data.chats
                            || exists(/$(prefix())/admins/$(request.auth.uid));
    }
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId
                            || exists(/$(prefix())/admins/$(request.auth.uid));
    }
    match /admins/{userId} {
      allow read, write: if request.auth != null && exists(/$(prefix())/admins/$(request.auth.uid));
    }
  }
}
```

The snippets below illustrate the number of document access calls used for a few data access patterns:

```javascript
// 0 document access calls used, because the rules evaluation short-circuits
// before the exists() call is invoked.
db.collection('user').doc('myuid').get(...);

// 1 document access call used. The maximum total allowed for this call
// is 10, because it is a single document request.
db.collection('chatroom').doc('mygroup').get(...);

// Initializing a write batch...
var batch = db.batch();

// 2 document access calls used, 10 allowed.
var group1Ref = db.collection("chatroom").doc("group1");
batch.set(group1Ref, {msg: "Hello, from Admin!"});

// 1 document access call used, 10 allowed.
var newUserRef = db.collection("users").doc("newuser");
batch.update(newUserRef, {"lastSignedIn": new Date()});

// 1 document access call used, 10 allowed.
var removedAdminRef = db.collection("admin").doc("otheruser");
batch.delete(removedAdminRef);

// The batch used a total of 2 + 1 + 1 = 4 document access calls, out of a total
// 20 allowed.
batch.commit();
```

For more information on how to resolve latency issues caused by large writes and batched writes, errors due to contention from overlapping transactions, and other issues consider checking out the troubleshooting page.
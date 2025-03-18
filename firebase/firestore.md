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
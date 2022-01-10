# SQL

Rule 1: Always use CTEs

When writing a complex query it’s a good idea to break it down into smaller components. As tempting as it might be to solve the query in one step don’t.

CTEs make your query easier to write and maintain in the future.

Rule 2: Keep CTEs small and single purpose

Your CTE needs to be an encapsulated logical component that helps you build your final query easier. It shouldn't try to do too much.

Rule 3: Don’t repeat yourself (DRY)

If you find yourself doing the same joins in a query, abstract it to a CTE.

If you find yourself doing the same joins in multiple queries, abstract them to a view.

Rule 4: Don’t tangle dependency chains

In modern data warehouses it's common to build tables on top of other tables in multiple layers of dependency.

Make sure your joins are at the same layer otherwise your dependency graph will look like spaghetti code.

Rule 5: Reduce your data before joining

Modern data warehouses are fast but that doesn’t mean you shouldn’t try to improve query performance.

If you only need six months of data, pre-filter that in a CTE first before your final select.

Rule 6: Only select the columns you need

It’s tempting to do SELECT \* everywhere but modern cloud warehouses use columnar storage so the more columns you choose the slower, more expensive your query will be.

Rule 7: Expect the unexpected

From NULLs, to missing data, duplicate rows and random values, real world data is messy. A well-written query is robust enough to handle many of these cases without crashing or giving inaccurate results.

Rule 8: Start with a left join

Real world data is messy. You never know if the column you’re joining on is fully represented in both tables. An inner join will filter out the non-matching rows and they could be important.

[https://mobile.twitter.com/ergestx/status/1479811885377765383](https://mobile.twitter.com/ergestx/status/1479811885377765383)

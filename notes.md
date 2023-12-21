![image](https://github.com/artempohribnyi/sql_notes/assets/113499718/fb8efe5a-624c-498b-8fca-9fd7e0da5def)

**Differentiating Techniques**

Let's take a minute to compare the techniques we've been using in this and the previous chapter. Joins allow you to directly combine information from 2 or more tables, and on their own, are mostly limited to simple combinations and aggregations of tables already present in your database. A correlated subquery allows you to combine information between a subquery and a table, or another subquery. These help you simplify your syntax and circumvent the limits of a join -- namely, that you can't join two separate columns in one table, to a single column in another at a time. We saw this in the "match" table, where you can't retrieve the team name from the "team" table for both the home and away teams at the same time. A correlated query can solve that problem. However, it's important to remember that correlated subqueries take a long time to process, and will slow down your query performance. Multiple and nested subqueries are useful when your data requires multi-step transformations before it's in the form you need for your final query. Breaking down the steps of your query process allows for better accuracy and reproducibility in your work. Finally, common table expressions allow you to organize your subqueries sequentially by declaring them at the beginning of your query. Since CTEs are processed one at a time before your main query, you can reference information from a CTE created earlier, thus serving as an alternative to nested subqueries.

## Window functions
![image](https://github.com/artempohribnyi/sql/assets/113499718/766b04ab-e6ce-4798-8e7e-412b343b356a)

![image](https://github.com/artempohribnyi/sql/assets/113499718/0d42ce16-e97b-4ebf-af2e-e06dac89d33c)

## Pivoting

![image](https://github.com/artempohribnyi/sql/assets/113499718/32975d04-1048-4632-9cec-bc350bec48b1)

![image](https://github.com/artempohribnyi/sql/assets/113499718/5dd61044-fac7-481b-82cb-c8ec547b3839)

![image](https://github.com/artempohribnyi/sql/assets/113499718/bf36b878-469b-45d3-b84d-b6b8f70df7be)

![image](https://github.com/artempohribnyi/sql/assets/113499718/ed8100c4-e9c3-42c5-a4cb-98451d865c0c)

![image](https://github.com/artempohribnyi/sql/assets/113499718/452815bb-508d-4720-90a8-0d9f7a4ea5b1)

![image](https://github.com/artempohribnyi/sql/assets/113499718/ead431a3-7124-4109-b9e1-633ea06e41c7)




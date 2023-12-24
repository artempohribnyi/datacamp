# 1. Introduction to full-text search

Great job! The last three chapters have explored many of the built-in functions available to you in PostgreSQL that will become invaluable skills for transforming and manipulating data using SQL.

# 2. Topics

In this chapter, we are going to get a bit more advanced and explore some of the features of PostgreSQL that allow you to extend its capabilities using custom code. In this chapter we'll explore: An introduction into the full text search capabilities of PostgreSQL that allow you to improve the manner by which you search text columns in your database. An overview of how to extend the features and capabilities of PostgreSQL using extensions. And finally we'll explore how to improve full text search with extensions and some advanced capabilities that you get when you combine the two.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/e601be49-1325-4c65-af37-6f2d9b97d414)


# 3. The LIKE operator

If you remember back in the prerequisite material, the LIKE operator can be used in a WHERE clause to search for a pattern in a column. To accomplish this, you use something called a wildcard as a placeholder for some other values. There are two wildcards you can use with LIKE. The first is the underscore sign which matches exactly one character. The second is the percent sign which will match zero or more characters of variable length. In this example we use the percent wildcard to match any title from the film table that starts with the word ELF followed by zero or more characters.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/3ff693bf-608d-4a5a-97f2-27047a9cad06)


# 4. The LIKE operator

Changing the position of the percent wildcard in our query produces a very different result. In this example, we use the percent sign wildcard to match a string that begins with one or more characters followed by the string ELF.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/2b22b754-e318-449d-907b-cd29f9fb934a)


# 5. The LIKE operator

LIKE is a great tool to use when searching for specific characters in a string. Sometimes when you are preforming a text search, you will want to match variations of the characters you are searching. For example, using LIKE to search the title column for any string that contains the word elf in all lowercase will return zero results. This may be counterintuitive to what you would expect because the LIKE operator matches the exact characters in the query and is case sensitive.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/fae450dc-aa83-418a-8dd3-ee243f32fd5b)


# 6. LIKE versus full-text search

However, look at this query which performs a full text search by using the functions to_tsvector and to_tsquery and the match operator to search the title column. Because full text search accounts for variations of the search string and is case insensitive you will notice that you get the expected results.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/66dc57f7-491e-48c1-84b1-0baa61143cd2)


# 7. What is full-text search?

So what is full-text search. Full text search provides a means for performing natural language queries of text data by using stemming, fuzzy string matching to handle spelling mistakes and a mechanism to rank results by similarity to the search string.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/5044dc34-eebc-4a07-9a44-cd47bc0c0bbd)


# 8. Full-text search syntax explained

Full text search can get complex but even a basic full text search query can be a very powerful tool. The example you see here is a basic technique for querying a document, in this case the column title, to match the characters elf. The WHERE clause of the query uses the match operator to compare the values returned by two built-in functions to perform the search, to_tsvector and to_tsquery. These functions convert text and string data to a tsvector data type which is a sorted list of words that have been normalized into variants of the same word. These variants are called `lexemes`.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/f6bf13aa-0603-4cad-914d-3c0a736d0ec2)


# 9. Let's practice!

Alright, let's get reacquainted with the LIKE operator and compare it to basic full text search results.


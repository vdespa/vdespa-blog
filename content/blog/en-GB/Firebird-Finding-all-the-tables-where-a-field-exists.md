# Firebird - Finding all the tables where a field exists #

So you have in front of you a huge database with thousands of tables and you want to find all the tables that have a certain field.

<!--BREAK-->


    SELECT *
    FROM RDB$RELATION_FIELDS
    WHERE RDB$FIELD_NAME = "FILED_YOU_ARE_LOOKING_FOR"

We also assume that the field name has on all the tables the same name.

You may also want to use wildcards.

#### References ####
- [How to get a list of tables, views and columns in Firebird database?](http://www.firebirdfaq.org/faq174/)




# My database standards

## My background
I have worked as a developer for the past 5 years. My main interests recently have been creating common code to follow the DRY principle more and to make code more readable. After collaborating with many clients and colleagues I have noticed that everyone uses slightly different standards which is irritating from an application development and analysis point of view as it reduces the ability to create common code and it requires a lot more checking to create queries as you are never quite sure the table/column names someone has used.

I should emphasis that there is **no right or wrong way** - the most important lesson you should take away is being *consistent* with whatever standard you use. 

## Benefit

The development of database standards helps to:

* Improve interoperability between databases.

* Enable the reuse of application code between systems - less code = less bugs.

* Faster development of application.

* Faster development of queries for data analysis.

## Database engine used

These standards were made using Postgres database syntax - code *may vary slightly with other database engines*.

## Schema names

* **Do not use underscores** to represent spaces between words.

* **Avoid abbreviations** - where reasonable.

* **All lowercase**.

## Table names

* Please use **singluar names** (e.g. person not persons table).

* **Do not use underscores** to represent spaces between words.

* **Avoid abbreviations** - where reasonable.

* **All lowercase**.

## Column names

* **Do not use underscores** to represent spaces between words.

* **Avoid abbreviations** - where reasonable.

* **All lowercase**.

## Foreign key columns

The foreign key column should be the **foreign table name** and the **foreign column name** that it is referencing.

e.g. projectuser table has **projectid** column which references the **project** table **id** column.

## Constraint names

All lowercase.


## Primary key

tablename_pkey

*Follows the same standard that postgres uses.*

ALTER TABLE ONLY [SCHEMA_NAME].[TABLE_NAME]
ADD CONSTRAINT [TABLE_NAME]_pkey PRIMARY KEY (id);



## Foreign key

foreigntablename_foreigntablecolumn_fkey

*Follows the same standard that postgres uses.*

ALTER TABLE ONLY [SCHEMA_NAME].[TABLE_NAME]
ADD CONSTRAINT [FOREIGN_TABLE_NAME]_[FOREIGN_COLUMN_NAME]_fkey FOREIGN KEY ([COLUMN_NAME]) REFERENCES [FOREIGN_SCHEMA_NAME].[FOREIGN_TABLE_NAME]([FOREIGN_COLUMN_NAME]);

Examples:

-- add foreign key constraint from createdby to user.id
ALTER TABLE ONLY public.example
ADD CONSTRAINT example_createdby_fkey FOREIGN KEY (createdby) REFERENCES public.user(id);

-- add foreign key constraint from modifiedby to user.id
ALTER TABLE ONLY backoffice.example
ADD CONSTRAINT example_modifiedby_fkey FOREIGN KEY (modifiedby) REFERENCES public.user(id);

## Standard table
id - serial (auto-increment) primary key

createdby - who created the record

modifiedby - who modified the record

createdon - when record was created

modifiedon - when record was modified

del - If the delete flag is set (we avoid using properly deleting records so we can preserve a record history) 

CREATE TABLE [SCHEMA_NAME].[TABLE_NAME] (
    id SERIAL PRIMARY KEY,
    [INSERT_OTHER_TABLES_HERE]
	  createdby integer NOT NULL,
    createdon timestamp without time zone DEFAULT now() NOT NULL,
    modifiedby integer,
    modifiedon timestamp without time zone,
    del boolean DEFAULT false NOT NULL
);
 
Note: The default table above to includes PRIMARY KEY - thus should not be needed unless creating additional keys (compound keys).

## Lookup table
The lookup table includes the standard table columns and in addition:

name - the name of the lookup

comment - additional information not explained completely from name alone.

ordinal - allows the application to sort by something other than either the order entered (id) or alphabetical (name)

CREATE TABLE [SCHEMA_NAME].[TABLENAME] (
    id SERIAL PRIMARY KEY,
    name character varying(30) NOT NULL,
    comment character varying(200),
	  ordinal integer NOT NULL,
	  createdby integer NOT NULL,
    createdon timestamp without time zone DEFAULT now() NOT NULL,
    modifiedby integer,
    modifiedon timestamp without time zone,
    del boolean DEFAULT false NOT NULL
);

Try to avoid adding any more columns as this will make any lookup application code need to be customised.

## Logic in the database

Avoid creating database functions (where possible).

If you cannot avoid, ensure that you add the code to Bitbucket so that we can track the history of the code.

**All lowercase**.

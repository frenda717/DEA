1. Practicing aggregation queries in SQL
    Women and children first!

    You probably know the story of the Titanic, an ocean liner that tragically sank in 1912. A famous policy of the time was "women and children first" when loading up the lifeboats in such a situation. Does the data show this is what actually happened on the Titanic?

    We've loaded up a sample of 100 passengers on the Titanic, which includes their age in years, whether they survived, and their self-reported gender, into a table named titanic. Explore this data, and compute:


    A listing of the first ten rows in the titanic table, to help you understand its structure and column names.

    The overall survival rate of the passengers in this data set. This result should be labeled overall_rate.

    The overall survival rate of "women and children," identified by a gender of 'female' or age of 12 or younger. This result should be labeled women_children_rate.

    The overall survival rate of everyone else who does not fit our definition of "women and children." This result should be labeled others_rate.

    Compute these as four separate SQL queries; we're not looking to use grouping here yet.

    Did women and children have better odds of surviving the Titanic than others did?

        (SELECT * FROM titanic LIMIT 10;)

        SELECT AVG(Survived) AS overall_rate FROM titanic;

        SELECT AVG(Survived) AS women_children_rate FROM titanic WHERE Age <= 12 OR Sex = 'female' ;

        SELECT AVG(Survived) AS others_rate FROM titanic WHERE Age > 12 AND Sex != 'female' ;

    
    Output:

        PassengerId	Survived	Pclass	Name	Sex	Age	SibSp	Parch	Ticket	Fare	Cabin	Embarked
        1	0	3	Braund, Mr. Owen Harris	male	22	1	0	A/5 21171	7.25	null	S
        2	1	1	Cumings, Mrs. John Bradley (Florence Briggs Thayer)	female	38	1	0	PC 17599	71.2833	C85	C
        3	1	3	Heikkinen, Miss. Laina	female	26	0	0	STON/O2. 3101282	7.925	null	S
        4	1	1	Futrelle, Mrs. Jacques Heath (Lily May Peel)	female	35	1	0	113803	53.1	C123	S
        5	0	3	Allen, Mr. William Henry	male	35	0	0	373450	8.05	null	S
        6	0	3	Moran, Mr. James	male	null	0	0	330877	8.4583	null	Q
        7	0	1	McCarthy, Mr. Timothy J	male	54	0	0	17463	51.8625	E46	S
        8	0	3	Palsson, Master. Gosta Leonard	male	2	3	1	349909	21.075	null	S
        9	1	3	Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)	female	27	0	2	347742	11.1333	null	S
        10	1	2	Nasser, Mrs. Nicholas (Adele Achem)	female	14	1	0	237736	30.0708	null	C
        overall_rate
        0.41
        women_children_rate
        0.7111111111111111
        others_rate
        0.1282051282051282

     "everyone else" as having a non-female sex and being over the age of 12, rather than just testing for "male". This ensures we capture people who did not report as male or female, or perhaps left that data blank.


2. Practicing grouping queries in SQL
    Did class matter? We have the same sample of 100 passengers on the Titanic, but this time we want to explore if the passenger class of their ticket (first, second, or third class) affected their odds of survival.

    Your sample dataset is in a table named titanic. This contains a column named Survived, which is 1 if they survived and 0 if not. There is also a Pclass column indicating their passenger class (1, 2, or 3.)


    Your task is to use GROUP BY in SQL to produce the survival rate for each passenger class in our dataset. Your output should contain a table with two columns named Pclass and survival_rate. The results should be sorted in ascending order by passenger class.

    First Attempt (Correct)

        (SELECT * FROM titanic LIMIT 20; (print the column first) )

        SELECT Pclass, AVG(Survived) AS survival_rate FROM titanic GROUP BY Pclass ORDER BY Pclass;

    Although you could compute survival rate with complicated CASE and mathematical functions, a simple AVG will do - the math works out.

    We produce survival_rates for each distinct Pclass using GROUP BY Pclass, and enforce the specified sorting of the results using ORDER BY.

    Output:
    
        Pclass	survival_rate
        1	0.47619047619047616
        2	0.6666666666666666
        3	0.3114754098360656


3. Practicing join queries in SQL
    We've loaded up a couple of tables of data from a fictional retailer: Products, containing information about the products sold by the company, and Suppliers, containing information about the companies that provided those products. These two tables are connected by columns named SupplierID.

    Create a report of every ProductName in the Products table, together with the CompanyName associated with each product's supplier.

    This query should be written in such a way that every product is listed in your report, even if no match exists in the Suppliers table for its SupplierID. Your final results should be sorted alphabetically by ProductName.


    Hint:
    The requirement to produce results for every product even if there is no matching SupplierID in the Suppliers table means a simple JOIN or INNER JOIN won't do.

    Don't forget to use ORDER BY to produce the results in the order specified.

    Remember you can refer to columns in specific tables using the format tablename.columnname.

    First Attempt:

        (SELECT * FROM Products LIMIT 10;)

        (SELECT * FROM Suppliers LIMIT 10;)

        SELECT Products.ProductName, Suppliers.CompanyName

    Correct Answer:

        (SELECT * FROM Products LIMIT 10;)

        (SELECT * FROM Suppliers LIMIT 10;)

        SELECT Products.ProductName, Suppliers.CompanyName FROM Products
        LEFT JOIN Suppliers ON Products.SupplierID = Suppliers.SupplierID
        ORDER BY Products.ProductName;


    We are using LEFT JOIN to fulfill the requirement to produce a row for every product, even if there is no matching SupplierID in the Suppliers table.

    ORDER BY is used to produce the specified sort order in the results.

    Output:

        ProductName	CompanyName
        Alice Mutton	Pavlova, Ltd.
        Aniseed Syrup	Exotic Liquids
        Boston Crab Meat	New England Seafood Cannery
        Camembert Pierrot	Gai pâturage
        Carnarvon Tigers	Pavlova, Ltd.
        Chai	Exotic Liquids
        Chang	Exotic Liquids
        Chartreuse verte	Aux joyeux ecclésiastiques
        Chef Anton's Cajun Seasoning	New Orleans Cajun Delights
        Chef Anton's Gumbo Mix	New Orleans Cajun Delights
        Chocolade	Zaanse Snoepfabriek
        Côte de Blaye	Aux joyeux ecclésiastiques
        Escargots de Bourgogne	Escargots Nouveaux
        Filo Mix	G'day, Mate
        Flotemysost	Norske Meierier
        Geitost	Norske Meierier
        Genen Shouyu	Mayumi's
        ...

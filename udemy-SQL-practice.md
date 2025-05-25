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

        SELECT * FROM titanic LIMIT 10;

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

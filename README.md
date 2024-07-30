# NoSQL Challenge

The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating.
In this challenge, suppose I've been contracted by the editors of a food magazine, Eat Safe, Love, to evaluate some of the ratings data in order to help their journalists and food critics decide where to focus future articles.

This challenge is divided into 3 parts:

    Part 1: Database and Jupyter Notebook Set Up
    Part 2: Updating the Database
    Part 3: Exploratory Analysis

Where Part 1 and Part 2 are done in NoSQL_setup.ipynb;
And Part 3 is done in NoSQL_analysis.ipynb.

## Part 1: Database and Jupyter Notebook Set Up

Connected to MongoDB, then:

    Imported the data provided in the establishments.json file from our Terminal. Named the database uk_food and the collection establishments.
    Within the notebook, imported the libraries we need: PyMongo and Pretty Print (pprint).
    Created an instance of the Mongo Client.
    Confirmed that we created the database and loaded the data properly:
        List the databases we have in MongoDB. Confirmed that uk_food is listed.
        List the collection(s) in the database and ensured that establishments is there.
        Find and display one document in the establishments collection using find_one and display with pprint.
    Assigned the establishments collection to a variable to prepare the collection for use.

## Part 2: Update the Database

Before you can perform any queries or analysis, the magazine editors have some requested modifications for the database:

    An exciting new halal restaurant just opened in Greenwich, but hasn't been rated yet. The magazine has asked us to include it in our analysis. Add the following information to the database:

    {
        "BusinessName":"Penang Flavours",
        "BusinessType":"Restaurant/Cafe/Canteen",
        "BusinessTypeID":"",
        "AddressLine1":"Penang Flavours",
        "AddressLine2":"146A Plumstead Rd",
        "AddressLine3":"London",
        "AddressLine4":"",
        "PostCode":"SE18 7DY",
        "Phone":"",
        "LocalAuthorityCode":"511",
        "LocalAuthorityName":"Greenwich",
        "LocalAuthorityWebSite":"http://www.royalgreenwich.gov.uk",
        "LocalAuthorityEmailAddress":"health@royalgreenwich.gov.uk",
        "scores":{
            "Hygiene":"",
            "Structural":"",
            "ConfidenceInManagement":""
        },
        "SchemeType":"FHRS",
        "geocode":{
            "longitude":"0.08384000",
            "latitude":"51.49014200"
        },
        "RightToReply":"",
        "Distance":4623.9723280747176,
        "NewRatingPending":True
    }

    Find the BusinessTypeID for "Restaurant/Cafe/Canteen" and return only the BusinessTypeID and BusinessType fields.
    Updated the new restaurant with the BusinessTypeID we found.
    The magazine is not interested in any establishments in Dover, so I checked how many documents contain the Dover Local Authority. Then, removed any establishments within the Dover Local Authority from the database, and checked again the number of documents to ensure they were deleted.
    Some of the number values are stored as strings, when they should be stored as numbers.
        Used update_many to convert latitude and longitude to decimal numbers.
        Use update_many to convert RatingValue to integer numbers.

## Part 3: Exploratory Analysis

"Eat Safe, Love" has specific questions they want you to answer, which will help them find the locations they wish to visit and avoid. 🤔

Some notes to be aware of while you are exploring the dataset:

    RatingValue refers to the overall rating decided by the Food Authority and ranges from 1-5. The higher the value, the better the rating.

    Note: This field also includes non-numeric values such as 'Pass', where 'Pass' means that the establishment
              passed their inspection but isn't given a number rating. We will coerce non-numeric values to nulls
              during the database setup before converting ratings to integers.

    The scores for Hygiene, Structural, and ConfidenceInManagement work in reverse. This means, the higher the value, the worse the establishment is in these areas.

Used the following questions to explore the database, and find the answers, so we can provide them to the magazine editors.

    Which establishments have a hygiene score equal to 20?
    Which establishments in London have a RatingValue greater than or equal to 4?
    Hint: The London Local Authority has a longer name than "London" so we used $regex as part of the search.
    What are the top 5 establishments with a RatingValue of 5, sorted by lowest hygiene score (↑), nearest to the new restaurant added, "Penang Flavours"?
    Hint: We compared the geocode to find the nearest locations. Search within 0.01 degree on either side of the latitude and longitude.
    How many establishments in each Local Authority area have a hygiene score of 0? Sort the results from highest to lowest (↓), and print out the top ten local authority areas.
    Hint: We had to use the aggregation method to answer this.

# nosql-challenge
Module 12 Challenge Repo

My final submission files are titled "NoSql_setup_final.ipynb" and "NoSQL_analysis_final.ipynb."

## References

I utilized the AI Xpert Learning Assistant to ask questions regarding queries that contained ranges using both greater than or equal to and lesser than or equal to. While ultimately this information was further discussed and corrected via tutor (see below), the exchange with the AI Xpert helped me work through the logic of what I wanted to do. This is an excerpt of the chatbot discussion:


---------------


Student User Logo
This code is causing an error because it says documents must only have string keys. How can I fix this? degree_search = 0.01 latitude = 51.49014200 longitude = 0.08384000 query = {'RatingValue': 5, latitude: {'$gte' : (latitude - degree_search)}, latitude: {'$lte' : (latitude + degree_search)}, longitude: {'$gte' : (longitude - degree_search)}, longitude: {'$lte' : (longitude + degree_search)}} sort = [('scores.Hygiene', -1)] limit = 5 results = list(establishments.find(query).sort(sort).limit(limit)) # Print the results pprint(results)
Xpert User Logo

To fix the error stating that documents must only have string keys, you need to ensure that the keys in your query dictionary are strings. In the provided code snippet, the keys latitude and longitude are not enclosed in quotes, which is causing the issue.

Here is the corrected version of the code snippet with the keys latitude and longitude as strings:

degree_search = 0.01
latitude = 51.49014200
longitude = 0.08384000

query = {'RatingValue': 5, 'latitude': {'$gte': (latitude - degree_search)},
         'latitude': {'$lte': (latitude + degree_search)},
         'longitude': {'$gte': (longitude - degree_search)},
         'longitude': {'$lte': (longitude + degree_search)}}

sort = [('scores.Hygiene', -1)]
limit = 5

results = list(establishments.find(query).sort(sort).limit(limit))

# Print the results
pprint(results)


By enclosing the keys latitude and longitude in quotes within the query dictionary, you should no longer encounter the error related to documents having only string keys.

Student User Logo
is there a better way to look within a range of values than using both $gte and $lte?
Xpert User Logo

Yes, MongoDB provides a concise way to query for a range of values using the $geoWithin operator. This operator allows you to specify a range for latitude and longitude values without explicitly using both $gte and $lte.

Here's an example of how you can use $geoWithin to query for documents within a specified range of latitude and longitude values:

degree_search = 0.01
latitude = 51.49014200
longitude = 0.08384000

query = {
    'RatingValue': 5,
    'location': {
        '$geoWithin': {
            '$box': [
                [latitude - degree_search, longitude - degree_search],
                [latitude + degree_search, longitude + degree_search]
            ]
        }
    }
}

sort = [('scores.Hygiene', -1)]
limit = 5

results = list(establishments.find(query).sort(sort).limit(limit))

# Print the results
pprint(results)


In this updated query, we use the $geoWithin operator with the $box operator to define a bounding box that represents the range of latitude and longitude values. This allows you to query for documents within that specified range without the need to use separate operators for greater than or less than comparisons.

-------------

## Tutoring

After double checking my work with a tutor, I got new information for further adjustments and placing my query range requirements in single lines rather than breaking them up. We determined this adjustment does affect the output, so the way we adjusted the code is the correct way to run it.
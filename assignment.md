# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: From the `movies` collection, return the documents with the `plot` that starts with `"war"` in acending order of released date, print only title, plot and released fields. Limit the result to 5.

Answer:

```python
for m in movies.find(
    {
        "plot": {
            "$regex": "^war", "$options": "i"
        }, 
        "released": {"$exists": True}
    }).sort('released', pymongo.ASCENDING).limit(5):

    print(f"TITLE: {m['title']} \n.  PLOT: {m['plot']} \n.  RELEASED: {m['released']}")

```

### Question 2

Question: Group by `rated` and count the number of movies in each.

Answer:

```python
stage_group_rated = {
   "$group": {
         "_id": "$rated",
         "movie_count": { "$sum": 1 }, 
   }   
}
stage_sorted_rated = {
   "$sort": {"_id": 1}   
}

pipeline = [
   stage_group_rated,
   stage_sorted_rated
]
results = movies.aggregate(pipeline)
print (type(results))

# Loop through the 'rated-summary' documents:
for rated_summary in results:
   print(rated_summary)
```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python
#Count the number of movies with 3 comments or more
#Give 2 options: 
#using 'movies' collection directly
print("-- movies --")
cnt = 0
for m in movies.find({"num_mflix_comments": {"$gte": 3}}):
    cnt+=1
    #print(f"TITLE: {m['title']} \n. Count Comments: {m['num_mflix_comments']}\n. ID: {m['_id']}")
print(f"From Movies collections using key num_mflix_comments for more than 3: {cnt}")


#using 'comments' collection directly
print(' ')
print("-- comments --")

comments = db.comments

stage_group_comment = {
   "$group": {
         "_id": "$movie_id",
         "movie_count": { "$sum": 1 }, 
   }   
}

stage_filter_count = {"$match": {"movie_count": {"$gte": 3}}}


pipeline = [
   stage_group_comment,
   stage_filter_count
]
results = comments.aggregate(pipeline)

# Loop through the 'rated-summary' documents:
cnt = 0
for rated_summary in results:
   cnt+=1
print(f"From Comment collection Count for Group by Movies and then filter only movie more than 3 - it found: {cnt}")

```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.

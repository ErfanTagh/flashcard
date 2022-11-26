# flashcard-backend

It's pretty facinating that we can develop a whole back-end project which includes a database in a single Python file!\
The main.py files containes all the rest-api api endpoints and database query executions which interacts with the [site's front-end](https://github.com/ErfanTagh/flashcard-frontend). 

Redis database is used for storing users flashcard values and answers.

```
@app.route('/words/rand/<token>', methods=['GET'])
def getwordrand(token):
    key = token
    print("token" + key)
    dataaa = redis.hgetall(key)
    if len(dataaa) == 0:

        return json.dumps(["You Don't Have Anything to Memorize ", "Please Add Cards!"])

    else:
        res = random.choice(list(dataaa.items()))

        return json.dumps(res)

```
The functions above receives the users email as the parameter and returns a random key-value of the users stored values-answers.\

The front sends the new value-answer as a POST request to the backend using the following function:

```
@app.route('/sendwords', methods=['POST'])
def send_word():
    data = request.json
    token = data['token']
    key = token

    entry = redis.hgetall(token)

    word = data['word']
    ans = data['ans']

    if word not in entry.keys():
        entry[word] = ans

        redis.hset(key, mapping=entry)

    return {"status": 200}

```

Rest-Api functions for deleting and editing key-values are also implemented in this file.














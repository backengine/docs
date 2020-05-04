### Using Backengine in Unity sample

**You should finish `*STEP 2*` at [Add Backengine to your Unity project](AddToUnity.md)**

Now you can try our leaderboard demo included in `Unity SDK`.

- In Unity Editor, open `Login_Leaderboard_Demo` scene at `Backengine\Demo\Scenes`
- Click register
- You should see a leaderboard here with some score records.
- Click `[+1 POINT]` to add some score then click `[SUBMIT SCORE]`. You should see your score in the leaderboard.
- Now try stop and open `Login_Leaderboard_Demo` again. Login with your registered user before to see if all thing go right.

#### Understand Leaderboard demo.

##### Now we will take a closer look to understand how this demo works

First, look at `Backengine\Demo\Scripts\Models` folder. We should see `UserModel.cs` and `ScoreModel.cs` here.  
These scripts are `BEModel` class, the model class for the Users and Score schema that you created earlier on the Backengine dashboard.  
These classes contain exactly the properties that correspond to the schema fields.  
You can create them yourself or simply download them from the Backengine dashboard after creating Schemas.  

#### Register

First, open `UIRegister.cs`
Look at

```C#
UserModel model = new UserModel();
model.email = Email.text;
model.name = Name.text;
model.password = Password.text;
```

We are creating an UserModel with data from input here.

Then we will make a request to the Backengine server.

```C#
BERequest.Instance.InsertAuth(model, (error, response) => {
  if (!error)
  {
    UserModel user = response.data;
  }

});
```

We call `BERequest.Instance.InsertAuth` to create a new document in `USERS` schema, which is an `Auth Schema` type, that we created on Backengine dashboard.  
Pass `UserModel` we created before as parameter. And if there is no `error` we could receive an user data created in `response.data`.

#### Login

Now, open `UILogin.cs` to see how to authenticate a user.

Look at

```C#
var requestData = new RequestData<UserModel>();
requestData = requestData.Where(x => (x.email==Email.text) && (x.password== Password.text));
```

To create data for your request, you should create an instance of `RequestData`.  
The code above, mean you are creating a `Condition` with an `UserModel` where `email==Email.text` and `password==Password.text`.

Next

```C#
BERequest.Instance.Auth(requestData, (error, response) =>
  {
    if (!error){
      UserModel user = response.data;
    }
  });
```

Now we call `BERequest.Instance.Auth` to perform user authentication with the requestdata we created.  

If you do not want to use `query style` code . You can also make requests using the UserModel like this.

```C#
UserModel user = new UserModel();
user.email = Email.text;
user.password = Password.text;

BERequest.Instance.Auth<UserModel>(user, (error, response) => {
  if (!error){
    UserModel user = response.data;
  }
});
```

#### Fetching scores board

Open `UILeaderboard.cs`

```C#
var requestData = new RequestData<ScoreModel>();
requestData.GetField(x=>x.score).GetRefs(x=>x.user).Sort(x=>x.score, SortType.Desc).Take(1,20);
BERequest.Instance.SelectMany(requestData, (error, response) => {
  if (!error){
    List<ScoreModel> scores =response.data;
    myScore = scores.Find(score => score.userRefs.id == Manager.User.id);
    if (myScore != null)
      BestScore.text = "BestScore: " + myScore.score.ToString();
    else BestScore.text = "BestScore: 0";
  }
});
```
To fetch documents from `SCORE` schema we call `BERequest.Instance.SelectMany`.
You should pass a `RequestData` to let server know how want to retrieve data.

```C#
var requestData = new RequestData<ScoreModel>();
requestData.GetField(x=>x.score).GetRefs(x=>x.user).Sort(x=>x.score, SortType.Desc).Take(1,20);
```
We created a `RequestData` type `ScoreModel`. We want to get `score` field so we call `GetField(x=>x.score)`
then we want get `user` field.  
If you remember, we defined `user` field in `SCORE` schema as a `Reference` field. That means `user` field should contains `id` from `USERS` schema.<br/> 
But we don't want only get `user's id`, we want get all user info belong with `score`. To do that, we should use `GetRefs(x=>x.user)`.<br/>
Then we wouldn't get `user` field as `user's id` anymore, we would get `userRefs` field as `UserModel` object.<br/>
Use `Sort(x=>x.score, SortType.Desc)` to sort data descending by `score` field.<br/>
Last, use `Take(1,20)` to get first 20 documents.  

#### Submit a score

In `UILeaderboard.cs` look at `OnSubmitScore` method

```C#
if (myScore != null){
  myScore.score = currentScore;
  BERequest.Instance.UpdateOne(myScore, (error, response) => {
    if (!error){
      RequestScores();
    }
  });
}
else{
  myScore = new ScoreModel();
  myScore.user = Manager.User.id;
  myScore.score = currentScore;
  BERequest.Instance.Insert(myScore, (error, response) => {
    if (!error){
      myScore = response.data;
      RequestScores();
    }
  });
}
```
In this method, we check if we fetched myScore before<br/>
Then we use `BERequest.Instance.UpdateOne` to update myScore ,a `ScoreModel`, to `SCORE` schame.
else if we didn't get myScore, that means we hasn't submited a score before.<br/>
Then we use `BERequest.Instance.Insert` to insert new score to `SCORE` schema.

#### Delete a score

Open `ScoreItem.cs` and look at `Delete` method

```C#
BERequest.Instance.DeleteOne(scoreModel, (error, response) => {
});
```
To delete exactly your score, Call `BERequest.Instance.DeleteOne` and pass `scoreModel` as param.<br/>
You should pass exact your score, because `SCORE` schema is a `Relation Schema` with `Require Edit Auth`.<br/>
If you pass other score as param, you should receive a response with `403 error code`.

>In this demo we showed you how to authenticate users using `Auth Schema`.<br/>
How to select, insert, update and delete data with Unity SDK. <br/>
For details on how to use it, and all methods in the Unity SDK. You can find them all at `Unity SDK API References`

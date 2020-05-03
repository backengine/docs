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

##### Register

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

We call `BERequest.Instance.InsertAuth` to create a new document in `users` schema, which is an `Auth Schema` type, that we created on Backengine dashboard.
Pass `UserModel` we created before as parameter. And if there is no `error` we could receive an user data created in `response.data`.

##### Login

Now, open `UILogin.cs` to see how to authenticate a user.

Look at

```C#
var requestData = new RequestData<UserModel>();
requestData = requestData.Where(x => (x.email==Email.text) && (x.password== Password.text));
```

To create data for your request, you should create an instance of `RequestData` class.
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
requestData.GetField(x=>x.score).GetRefs(x=>x.user).Sort(x=>x.score, SortType.Desc);
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

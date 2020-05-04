# Using Backengine to create a simple login/register and leaderboard app

**To **CONTINUE** you should finish `*STEP 1*` in `Getting started` session of this documents for your plaftorm.**

### Prepair Backengine Project

### Create `USERS` schema

- Click `View` button in your app created in above step.
- In your app overview page, click `Create a schema` or ![Plus Button](/images/plusbutton.png) button to create a new schema.
- Enter `users` in `Schema Name` box.
- Select `Auth Schema` checkbox to define this is an `Auth Schema` type.
- Click `Next`.

**In next steps we create `fields` for `users` schema.**

##### Create `email` field

- Click [+] button on the top right.
- Input `email`, select `String` type, select `Unique`, `Require`, `For Auth`, do not select `Encrypted`.
- Click [&#10004;] to save.

##### Create `password` field

- Click [+] button on the top right.
- Input `password`, select `String` type, select `Require`, `Encrypted`, `For Auth`, do not select `Unique`.
- Click [&#10004;] to save.

##### Create `name` field

- Click [+] button on the top right.
- Input `name`, select `String` type, select `Require`, do not select `Encrypted`, `For Auth`, `Unique`.
- Click [&#10004;] to save.

Then click `[CREATE SCHEMA]` button. You should see `users` schema after processing.

### Create `SCORE` schema

- In your app overview page, click `Create a schema` or ![Plus Button](/images/plusbutton.png) button to create a new schema.
- Enter `score` in `Schema Name` box.
- Select `Edit Require Auth` checkbox to define this is an `Relation Schema` type.
- Click `Next`.

**In next steps we create `fields` for `users` schema.**

##### Create `score` field

- Click [+] button on the top right.
- Input `score`, select `Number` type, select `Require`, do not select `Encrypted`, `For Auth`, `Unique`.
- Click [&#10004;] to save.

##### Create `user` reference field.

- Click [+] button on the top right.
- Input `user`, select `Reference` type, select `Require`, do not select `Encrypted`, `For Auth`, `Unique`.
- Select `users` in `Ref` column.
- Click [&#10004;] to save.

Then click `[CREATE SCHEMA]` button. You should see `score` schema after processing.

### Download Model Scripts to using in client project.

After created schemas, you can download model scripts belong to your schemas that we generated for you.
In `Apps` page. Look at your project. You should see `[Download Models]` button. Click to download.


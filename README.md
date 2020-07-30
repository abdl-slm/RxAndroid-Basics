# RxJava/RxAndroid Basics

Why use RxJava in Android?

Lets say, we want to make a network call and update the UI upon the completion. Usually AsyncTask is the way we do it.

Example Code:

```
  public class NetworkRequestTask extends AsyncTask<Void, Void, User> {

      private final int userId;

      public NetworkRequestTask(int userId) {
          this.userId = userId;
      }

      @Override protected User doInBackground(Void... params) {
          return networkService.getUser(userId);
      }

      @Override protected void onPostExecute(User user) {
          nameTextView.setText(user.getName());
          // ...set other views
      }
  }

  private void onButtonClicked(Button button) {
     new NetworkRequestTask(123).execute()
  }
 ```

However, the disadvantages of this is: 

- Memory/context leaks are easily created since NetworkRequestTask is an inner class
- If we wwant to chain network calls, nested asynctask will be required, hence reducing readibility.

 
# RxJava consists of 3 cores: 

- Observable: emits items (the stream).
- Observer: consumes those items.
- Operators: Emissions from Observable objects can further be modified, transformed, and manipulated by chaining Operator calls.

# Observable:

```
Observable<Integer> observable = Observable.create(new Observable.OnSubscribe<Integer>() {
   @Override public void call(Subscriber<? super Integer> subscriber) {
       subscriber.onNext(1);
       subscriber.onNext(2);
       subscriber.onNext(3);
       subscriber.onCompleted();
   }
});
```

# Observer:

 Users are notified when something happens within the observable, there are 3 types of events
 
 - Observer#onNext(T) - invoked when an item is emitted from the stream
 - Observable#onError(Throwable) - invoked when an error has occurred within the stream
 - Observable#onCompleted() - invoked when the stream is finished emitting items.

```
Observable<Integer> observable = Observable.just(1, 2, 3);
observable.subscribe(new Observer<Integer>() {
   @Override public void onCompleted() {
       Log.d("Test", "In onCompleted()");
   }

   @Override public void onError(Throwable e) {
       Log.d("Test", "In onError()");
   }

   @Override public void onNext(Integer integer) {
       Log.d("Test", "In onNext():" + integer);
   }
});
```

# Operators:

# Retrofit and RxJava




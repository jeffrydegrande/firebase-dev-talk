!SLIDE 
# Firebase #

!SLIDE bullets smaller
# What? #

* It's a database.
* It's an API
* "It's a *powerful* API to store & sync data in realtime"
* That's the official tag line
* My take: a persisted, nested data structure, expressed in json, with realtime
  synchronisation, close enough to REST to be familiar

!SLIDE bullets
# javascript! #

* officially supported libraries for
* angular
* ember
* react !! :)
* backbone
* vulcan (chrome devtool thingy)

!SLIDE bullets
# also .. #

* iOS
* Android
* & REST

!SLIDE bullets
# security #

* have firebase handle authentication using facebook, twitter, github ...
* or handle it yourself: com.firebase.security.token.TokenGenerator => javascript: ref.auth(token)


!SLIDE code smaller
# security rules / validations

    @@@ javascript
    { "rules": {
        "mentions": {
          ".read": "auth != null"
         }
      },
      "validations": {
      }
    }

!SLIDE code smaller

# examples 

    @@@ javascript

    var ref = new Firebase("https://publicdata-cryptocurrency.firebaseio.com/bitcoin");

    ref.child("bid").on("value", showPrice);
    ref.child("ask").on("value", showPrice);
    ref.child("last").on("value", showPrice);

    function showPrice(snapshot) {
      console.log(snapshot.key() + ": " + snapshot.val());
    }

!SLIDE code smaller
# collections

    @@@ javascript

    var ref = new Firebase("https://hypotheticalengagorapp.firebaseio.com/mentions");

    ref.on("child_added", function(snapshot) {
      // snapshot.val() is a mention
    });

    // there are functions available, like limit() to limit the amount of data you
    // pull in, otherwise you do pull in everything beneath this reference.
    // Deleting data is always on option too


!SLIDE code smaller
# write data

    @@@ javascript

    var ref = new Firebase("https:.......");

    // set a value at ref. overwrites data at this location. value
    // can be nested
    ref.set({some: "data"});


    // generate a new ref with a unique name and write date to it
    ref.push({some: "data"});


!SLIDE code smaller

# firebase to elasticsearch (from scala in this case)

    @@@ scala

    val client = ElasticClient.remote("localhost", 9300)

    var ref = new Firebase("https://somethingsomethingsomething.firebase.io/comments")
    var cb = new ValueEventListener() {
      override def onDataChange(snap: DataSnapshot) {
        val comments = Comment.fromSnapshot(snap).asScala.toMap
        comments.values.map { message =>
          client.execute {
            index into "comments" fields (
              "name" -> message.getAuthor,
              "message" -> message.getBody
            )
          }
        }
      }
    }

    ref.addValueEventListener(cb)
    
     

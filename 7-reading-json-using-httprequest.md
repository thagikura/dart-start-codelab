<toc-element></toc-element>

### Create piratenames.json

&rarr; Use **File > New File...** to create a JSON-encoded file
named `piratenames.json` with the following content.

Put the file in `web` alongside the Dart and HTML files you've been editing.

    { "names": [ "Anne", "Bette", "Cate", "Dawn",
            "Elise", "Faye", "Ginger", "Harriot",
            "Izzy", "Jane", "Kaye", "Liz",
            "Maria", "Nell", "Olive", "Pat",
            "Queenie", "Rae", "Sal", "Tam",
            "Uma", "Violet", "Wilma", "Xana",
            "Yvonne", "Zelda",
            "Abe", "Billy", "Caleb", "Davie",
            "Eb", "Frank", "Gabe", "House",
            "Icarus", "Jack", "Kurt", "Larry",
            "Mike", "Nolan", "Oliver", "Pat",
            "Quib", "Roy", "Sal", "Tom",
            "Ube", "Val", "Walt", "Xavier",
            "Yvan", "Zeb"],
      "appellations": [ "Awesome", "Captain",
            "Even", "Fighter", "Great", "Hearty",
            "Jackal", "King", "Lord",
            "Mighty", "Noble", "Old", "Powerful",
            "Quick", "Red", "Stalwart", "Tank",
            "Ultimate", "Vicious", "Wily", "aXe", "Young",
            "Brave", "Eager",
            "Kind", "Sandy",
            "Xeric", "Yellow", "Zesty"]}


Key information:

* The file contains a JSON-encoded map,
which contains two lists of strings.


### Edit piratebadge.html

&rarr; Disable the input field and the button
by adding a `disabled` attribute to the `<input>` and `<button>` tags.

    ...
      <div>
        <input type="text" id="inputName" maxlength="15" disabled>
      </div>
      <div>
        <button id="generateButton" disabled>Aye! Gimme a name!</button>
      </div>
    ...

Key information:

* The Dart code enables the text field and
the button after the pirate names are successfully read from
the JSON file.


### Edit piratebadge.dart

&rarr; Import the dart:async library.

    import 'dart:html';
    import 'dart:math' show Random;
    import 'dart:convert' show JSON;
    import 'dart:async' show Future;

Key information:

* The `dart:async` library provides for asynchronous programming.

* A `Future` provides a way to get a value in the future.
  (If you're a JavaScript developer: Futures are similar to Promises.)


&rarr; Replace the `names` and `appellations` lists with these static, empty lists:

    class PirateName {
      ...
      static List<String> names = [];
      static List<String> appellations = [];
      ...
    }

Key information:

* **Be sure to remove `final` from these declarations.**

* `[]` is equivalent to `new List()`.

* A List is a _generic_ type&mdash;a List can contain any kind of object.
If you intend for a list to contain only strings,
you can declare it as `List<String>`.


&rarr; Add two static functions to the PirateName class:

    class PirateName {
      ...

      static Future readyThePirates() {
        var path = 'piratenames.json';
        return HttpRequest.getString(path)
            .then(_parsePirateNamesFromJSON);
      }
      
      static _parsePirateNamesFromJSON(String jsonString) {
        Map pirateNames = JSON.decode(jsonString);
        names = pirateNames['names'];
        appellations = pirateNames['appellations'];
      }
    }

Key information:

* `HttpRequest` is a utility for retrieving data from a URL.

* `getString()` is a convenience method for doing a simple
GET request that returns a string.

* The code uses a `Future` to perform the GET asynchronously.

* The callback function for `.then()` is called when
the Future completes successfully.

* When the Future completes successfully,
the pirate names are read from the JSON file.

* `readyThePirates` returns the Future so the main program has the
opportunity to do something after the file is read.


&rarr; Add a top-level variable.

    SpanElement badgeNameElement;

    void main() {
      ...
    }

Key information:

* Stash the span element for repeated use instead of querying the DOM for it.


&rarr; Change the `main()` function to stash the input and span elements,
and to get names from the JSON file.

After these changes, `main()` should look like this:

    void main() {
      InputElement inputField = querySelector('#inputName');
      inputField.onInput.listen(updateBadge);
      genButton = querySelector('#generateButton');
      genButton.onClick.listen(generateBadge);
      
      badgeNameElement = querySelector('#badgeName');
      
      PirateName.readyThePirates()
        .then((_) {
          //on success
          inputField.disabled = false; //enable
          genButton.disabled = false;  //enable
          setBadgeName(getBadgeNameFromStorage());
        })
        .catchError((arrr) {
          print('Error initializing pirate names: $arrr');
          badgeNameElement.text = 'Arrr! No names.';
        });
    }

Key information:

* Stash the input element in a local variable (`inputField`).
* Stash the span element in the global variable (`badgeNameElement`).
* The `readyThePirates()` function returns a Future.
* When the Future successfully completes,
the `then()` callback function is called.
* Using underscore (`_`) as a parameter name
indicates that the parameter is ignored.
* The callback function enables the UI
and gets the stored name.
* If the Future encounters an error
the `catchError()` callback function is called
and the program displays an error message,
leaving the UI disabled.
* The callback functions for `then()` and `catchError()` are defined inline.


### Run the app

&rarr; Save your files with **File > Save All**.

&rarr; Run the app by right-clicking `piratebadge.html` and
selecting **Run in Dartium**.

&rarr; If you want to see what happens when the app can't find the `.json` file,
change the file name in the code and run the program again.

&rarr; Your app should look like it did before,
but with a bigger pool of names and appellations.


#### Problems?

Check your code against the files in <io-location-string noclone="true" starterpath="/step7"></io-location-string>.

<toc-element></toc-element>

### Edit piratebadge.dart

&rarr; Import the JSON converter from the `dart:convert` library.

    import 'dart:html';
    import 'dart:math' show Random;
    import 'dart:convert' show JSON;

Key information:

* `JSON` provides convenient access to the most common JSON use cases.


&rarr; Add a named constructor to the PirateName class.

    class PirateName {
      ...
      PirateName.fromJSON(String jsonString) {
        Map storedName = JSON.decode(jsonString);
        _firstName = storedName['f'];
        _appellation = storedName['a'];
      }
    }

Key information:

* The constructor creates a new PirateName instance from a JSON-encoded string.

* `PirateName.fromJson` is a named constructor.

* `JSON.decode()` parses a JSON string and creates Dart objects from it.

* The pirate name is decoded into a `Map` object.


&rarr; Add a getter to the PirateName class
that encodes a pirate name in a JSON string.

    class PirateName {
      ...
      String get jsonString => JSON.encode({"f": _firstName, "a": _appellation});
    }

Key information:

* The getter formats the JSON string using the map format.


&rarr; Declare a **top-level** string.

    final String TREASURE_KEY = 'pirateName';

    void main() {
      ...
    }

Key information:

* You store key-value pairs in local storage. This string is the key.
The value is the pirate name.


&rarr; Save the pirate name when the badge name changes.

    void setBadgeName(PirateName newName) {
      if (newName == null) {
        return;
      }
      querySelector('#badgeName').text = newName.pirateName;
      window.localStorage[TREASURE_KEY] = newName.jsonString;
    }

Key information:

* Local storage is provided by the browser's `Window`.


&rarr; Add a top-level function called `getBadgeNameFromStorage()`.

    void setBadgeName(PirateName newName) {
      ...
    }

    PirateName getBadgeNameFromStorage() {
      String storedName = window.localStorage[TREASURE_KEY];
      if (storedName != null) {
        return new PirateName.fromJSON(storedName);
      } else {
        return null;
      }
    }

Key information:

* The function retrieves the pirate name from local storage
and creates a PirateName object from it.


&rarr; Call the function from the `main()` function.

    void main() {
      ...
      setBadgeName(getBadgeNameFromStorage());
    }


Key information:

* Initialize the badge name from local storage.


### Run the app

&rarr; Save your files with **File > Save All**.

&rarr; Run the app by right-clicking `piratebadge.html` and
selecting **Run in Dartium**.

&rarr; Click the button to put a name on the badge.

&rarr; Start the app again by duplicating the app's window.
(Right-click the tab and choose **Duplicate**.)
Instead of starting up blank,
it should contain the same name and appellation
as the first copy of the app displays.


#### Problems?

Check your code against the files in <io-location-string noclone="true" starterpath="/step6"></io-location-string>.

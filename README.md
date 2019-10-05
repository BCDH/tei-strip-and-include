# TEI Strip and Include Examples

TEI uses `<egXML>` in a separate namespace (`http://www.tei-c.org/ns/Examples`) for encoding XML fragments demonstrating the use of some XML, in which the egXML element functions as the root element.

I was driven on the brink of insanity by the fact that the content of `<egXML>` cannot include any other namespace including `http://www.tei-c.org/ns/1.0`. Because of this restriction, it is impossible to xi:include fragments from a separate TEI file.

Not being able to validate or reuse one's examples is a total pain in the neck, especially on longer documents.

You can use my quick and dirty workaround, as illustrated by this repo:

- open the oXygen project file `TEI-strinp-and-include-examples.xpr`
- use `examples. xml` to store and validate all your TEI examples
- associate this file with an XSLT transformation scenario (in the attached oXygen project `TEI-ns-stripper`) which uses the provided `tei-stripper.xsl`
- every time you add and validate some new TEI content in `examples.xml`, run the `TEI-ns-stripper` scenario. It will create a file called `examples-stripped.xml`, which will be exactly the same as your original `example.xml`, but it will put the TEI root element in the `http://www.tei-c.org/ns/Examples` namespace, making it legit for inclusion in `<egXML>`
- to include TEI fragments in a TEI document (in this repo, `call4examples.xml`), use `xi:include` to include from `examples-stripped.xml` as opposed to `examples.xml`, like this:
  ```xml
  <egXML xmlns="http://www.tei-c.org/ns/Examples">
    <xi:include href="examples-stripped.xml" xpointer="element(test/1)"/>
  </egXML>
  ```
- The included fragments will now be in the same namespace as `<egXML>` which means that your `call4examples.xml` file will not cause any validation errors, but your included (and potentially reusable) fragments will be the same as those that you validated im `examples.xml`.

Of course, if there is an easier way to do all this, I'd be thrilled to learn about it.

![screenshot](https://i.imgur.com/rbmr8qV.png)

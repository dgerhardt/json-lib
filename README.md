# JSON-lib

## About this fork

This is a fork of [JSON-lib](https://github.com/aalmiray/Json-lib) which fixes
handling of strings containing JSON-like syntax. The original JSON-lib parsed
JSON in strings if they contained _array_, _object_ or _function_ syntax or the
`null` keyword.

Please note that JSON-lib and this fork are not actively maintained. This fork
was solely created to fix the issues mentioned above for legacy code. If you are
already using JSON-lib feel free to use this fork. But if you are looking for a
modern JSON parser, I recommend you have a look at
[Jackson](https://github.com/FasterXML/jackson).


## Auto-expansion of properties

The XML serialization is extended to handle automatic expansion of properties
for arrays that contains objects.


### Source file to be serialized to and from JSON

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Document DOMVersion="8.0" Self="d">
    <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="Adobe XMP Core 5.3-c011 66.145661, 2012/02/06-14:56:27">
        <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
            <rdf:Description rdf:about=""
                             xmlns:dc="http://purl.org/dc/elements/1.1/">
                <dc:format>application/x-indesign</dc:format>
            </rdf:Description>
            <rdf:Description rdf:about=""
                             xmlns:xmp="http://ns.adobe.com/xap/1.0/">
                <xmp:CreatorTool>Adobe InDesign CS6 (Macintosh)</xmp:CreatorTool>
            </rdf:Description>
            <rdf:Description rdf:about=""
                             xmlns:xmpMM="http://ns.adobe.com/xap/1.0/mm/">
                <xmpMM:InstanceID>xmp.iid:D093CC710A2068118083AFA2F0AAE3ED</xmpMM:InstanceID>
            </rdf:Description>
        </rdf:RDF>
    </x:xmpmeta>
</Document>
```

file.xml


### Original JSON-lib

This code

```java
XMLSerializer serializer = new XMLSerializer();
serializer.setTypeHintsEnabled(false);

JSON jsonRepresentation = serializer.readFromFile( "file.xml" );

String xml = serializer.write( jsonRepresentation );
System.out.writeToReadme(xml)
```

will render:

```xml
<o DOMVersion="8.0" Self="d">
    <x:xmpmeta xmptk="Adobe XMP Core 5.3-c011 66.145661, 2012/02/06-14:56:27" xmlns:x="adobe:ns:meta/">
        <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
            <rdf:Description>
                <e about="" xmlns:dc="http://purl.org/dc/elements/1.1/">
                    <dc:format>application/x-indesign</dc:format>
                </e>
                <e about="" xmlns:xmp="http://ns.adobe.com/xap/1.0/">
                    <xmp:CreatorTool>Adobe InDesign CS6 (Macintosh)</xmp:CreatorTool>
                </e>
                <e about="" xmlns:xmpMM="http://ns.adobe.com/xap/1.0/mm/">
                    <xmpMM:InstanceID>xmp.iid:D093CC710A2068118083AFA2F0AAE3ED</xmpMM:InstanceID>
                </e>
            </rdf:Description>
        </rdf:RDF>
    </x:xmpmeta>
</o>
```


### AutoExpand

This code

```java
XMLSerializer serializer = new XMLSerializer();
serializer.setTypeHintsEnabled(false);
serializer.setPerformAutoExpansion(true);

JSON jsonRepresentation = serializer.readFromFile( "file.xml" );

String xml = serializer.write( jsonRepresentation );
System.out.writeToReadme(xml)
```

will render:

```xml
<o DOMVersion="8.0" Self="d">
<x:xmpmeta xmptk="Adobe XMP Core 5.3-c011 66.145661, 2012/02/06-14:56:27" xmlns:x="adobe:ns:meta/">
    <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
        <rdf:Description about="" xmlns:dc="http://purl.org/dc/elements/1.1/">
            <dc:format>application/x-indesign</dc:format>
        </rdf:Description>
        <rdf:Description about="" xmlns:xmp="http://ns.adobe.com/xap/1.0/">
            <xmp:CreatorTool>Adobe InDesign CS6 (Macintosh)</xmp:CreatorTool>
        </rdf:Description>
        <rdf:Description about="" xmlns:xmpMM="http://ns.adobe.com/xap/1.0/mm/">
            <xmpMM:InstanceID>xmp.iid:D093CC710A2068118083AFA2F0AAE3ED</xmpMM:InstanceID>
        </rdf:Description>
    </rdf:RDF>
</x:xmpmeta>
</o>
```

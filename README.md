# ActiveAnnotations

ActiveAnnotations is a gem for modeling simple [OpenAnnotation](http://www.openannotation.org/)-compatible annotations to Rails. It is ActiveRecord compatible, but is backed by an RDF graph that serializes (by default) to [JSON-LD](http://json-ld.org/).

* The `source_uri` and the serialized annotation are stored in ActiveRecord; other methods access the underlying graph.
* All attributes are optional.
* Target selectors are currently limited to time-based media fragments, but will be generalized in future iterations.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'active_annotations'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install active_annotations

## Usage

```ruby
note = ActiveAnnotations::Annotation.create
note.label = 'This is the annotation label'
note.annotated_by = 'https://github.com/mbklein/'
note.annotated_at = DateTime.now
note.content = 'The five boxing wizards jump quickly.'
note.source = thing # thing is expected to respond to :rdf_uri and :rdf_type
note.start_time = 3.5
note.end_time = 10.75
note.save

puts note.pretty_annotation
```
Output:
```json
{
  "@context": "http://www.w3.org/ns/oa-context-20130208.json",
  "@id": "urn:uuid:cac68351-a920-47d5-8c7c-9820e2f4d3bd",
  "@type": "oa:Annotation",
  "annotatedBy": {
    "@id": "https://github.com/mbklein/",
    "@type": "foaf:Person"
  },
  "hasBody": {
    "@id": "urn:uuid:55cb5e7e-19bf-406c-9e4c-cc0ff9fd0689",
    "@type": [
      "dctypes:Text",
      "cnt:ContentAsText"
    ],
    "chars": "The five boxing wizards jump quickly."
  },
  "hasTarget": {
    "@id": "urn:uuid:7a5adf37-7bf2-45d4-ab0d-5a6b9aa2c6ad",
    "@type": "oa:SpecificResource",
    "hasSelector": {
      "@id": "urn:uuid:21331f24-e065-4c06-8c7f-b42a95c53721",
      "@type": "oa:FragmentSelector",
      "conformsTo": "http://www.w3.org/TR/media-frags/",
      "value": "t=3.5,10.75"
    },
    "hasSource": {
      "@id": "http://www.example.edu/thing/being/annotated",
      "@type": "dctypes:MovingImage"
    }
  },
  "label": "This is the annotation label",
  "oa:annotatedAt": {
    "@value": "2016-04-22T12:59:31Z",
    "@type": "xsd:dateTime"
  }
}
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/active_annotations.


## License

The gem is available as open source under the terms of the [Apache License](http://www.apache.org/licenses/LICENSE-2.0).

# camel-atom

Atom feed consumption in Camel is provided by the [camel-atom](http://camel.apache.org/atom.html) component.

## Example Atom feed consumer Camel route

The following example configures an Atom endpoint to consume the recent activity GitHub feed of user 'wildflyext'. The raw content of each feed entry is then written out as a file within directory 'feed/entries'.

```java
CamelContext camelContext = new DefaultCamelContext();

camelContext.addRoutes(new RouteBuilder() {
    @Override
    public void configure() throws Exception {
        from("atom://https://github.com/wildflyext.atom?splitEntries=true")
        .process(new Processor() {
            @Override
            public void process(final Exchange exchange) throws Exception {
                Entry entry = exchange.getIn().getBody(Entry.class);
                exchange.getOut().setBody(entry.getContent());
            }
        })
        .to("file:///feed/entries/");
    }
});
```


##### ***What is XML & XXE?***
XML (Extensible Markup Language) is a markup language, it used to provide rules to define data for communicating with back-end systems. XXE (XML External Entity Injection) is a web vulnerability that an attacker uses to inject malicious XML into an XML request body, to interact or retrieve sensitive data from back-end systems.
##### ***Retrieving files using malicious XXE***
In this example, there is a stock check functionality in the vulnerable website that doesn't have any protections against arbitrary XML payloads. I was able to see the contents of `/etc/passwd` by using the following payload with burp's intercept functionality against a POST request to retrieve the stock of a product:
```
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
	<productId>
		&xxe;
	</productId>
</stockCheck>
```

This payload works and retrieves the contents of `/etc/passwd` because the XML parser encounters the `<!DOCTYPE>` definition and registers `xxe` as an external entity that points to `file:///etc/passwd`. When the parser encounters `&xxe;` within the `<productId>` tags, it replaces it with the content it retrieves from `file:///etc/passwd`.

Placing the `<!DOCTYPE>` declaration directly inside the `<productId>` tags would not work as intended because:
- XML Document Type Definitions (DTDs), including `<!DOCTYPE>` declarations, are typically only valid at the beginning of an XML document, immediately after the XML declaration. Placing it elsewhere in the document is generally not allowed by XML parsers and would likely result in a parsing error.
    
- Even if the parser did allow the `<!DOCTYPE>` inside `<productId>`, the scope of the entity definition would be limited to that specific element. This means that the entity would not be recognized or expanded elsewhere in the document.
    
- The `<!DOCTYPE>` declaration is meant to define the document's structure and entities at a global level, not within specific elements. Placing it within an element like `<productId>` would be semantically incorrect according to XML standards.



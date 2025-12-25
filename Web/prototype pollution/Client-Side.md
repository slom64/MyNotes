- You mainly add `__proto__.canary = "random"` and enumerate `Object` or other objects in DOM if they got `canary` property.
- **Prototype pollution via the constructor**: some times `__proto__` is heavily sanitized, so we need another way to trigger prototype pollution
	- `myObject.constructor.prototype`
	- `__pro__proto__to__.gadget=payload`

## Finding client-side prototype pollution sources manually

Finding [prototype pollution sources](https://portswigger.net/web-security/prototype-pollution#prototype-pollution-sources) manually is largely a case of trial and error. In short, you need to try different ways of adding an arbitrary property to `Object.prototype` until you find a source that works.

When testing for client-side vulnerabilities, this involves the following high-level steps:

1. Try to inject an arbitrary property via the query string, URL fragment, and any JSON input. For example:
    `vulnerable-website.com/?__proto__[foo]=bar`
2. In your browser console, inspect `Object.prototype` to see if you have successfully polluted it with your arbitrary property:
    `Object.prototype.foo // "bar" indicates that you have successfully polluted the prototype // undefined indicates that the attack was not successful`
3. If the property was not added to the prototype, try using different techniques, such as switching to dot notation rather than bracket notation, or vice versa:
    `vulnerable-website.com/?__proto__.foo=bar`
4. Repeat this process for each potential source.
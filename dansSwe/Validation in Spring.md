
On entities we can have the following validation:
```java
@NotEmpty(message="Must not be empty!")
@NotNull
@Null
@Size(max = ..., message="")
@Pattern(regexp="", message="")
```
Within these we can also set error messages and provide extra parameters for example for `@Pattern` we can provide the required regex

And in the controller you state what fields have to be validated and provide parameters to capture errors
```java
@PostMapping
public void postVenue(@Valid @ModelAttribute("venue") Venue venue, BindingResult errors); {
	if (errors.hasErrors()) {...}
}
```

goto: [[SWE 2 Home Page]]
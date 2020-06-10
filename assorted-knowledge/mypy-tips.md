# Mypy tips

## Learn a language that is actually typed.

#### \(If you have time\)

I recommend the elm language. It'st the first language with which I built impressive application. There is a great community on slack and discourse, very welcoming to beginners.

## Don't redefine a variable.

 Every time you change what the variable represents to you will get an error.

```text
a :str = "abc"
a = list(a)
a = {i:j for i,j in enumerate(a)}
```

While a contrived example above, you can see it is easy to reuse a variable and makes a lot of sense when writing. But this will throw 2 mypy errors even though its otherwise good code.


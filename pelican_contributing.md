## 12.23.20 Compound File Extensions

### How it works now

1. The user specifies in the `READERS = {}` dict in settings that they want to map certain extensions to certain reader classes. So to add a custom reader for the `foo` extension we'd add `READERS = {'foo': FooReader}`
2. The user writes some articles `article1.foo`, `article2.foo`, etc. and runs `pelican` to generate the html files.
3. Pelican gets a list of generators and with each of them runs `generate_context()` which looks in the article directory to build a list of files that all need to be read and then written. **Part of the problem is that `generate_context()` calls `get_files()` which calls `_include_path()` which checks to see if the extension is among the list of those allowed. At this stage something like `file.md.html` would be interpreted as an `html` file. This is the change I did make in my original pull request https://github.com/getpelican/pelican/pull/2816/commits/d44d27f0932c47f17bfcd181389a83a50997aec0**
4. On each file in the list it calls the `read_file()` method of the `Readers` class.  
   1. This class is a generic interface to all of the other Reader classes. It contains a mapping of file extensions to Reader classes, to know which Reader class must be used to read a file (based on its extension). 
   2. Specifically, each reader class contains a list of extensions that it reads for.  The generic `Reader` class iterates through each Reader class, looks at its list of extensions, and maps each one to the reader class in its own `reader_classes` dict.
5. This method takes the path to the file and strips the extension out. It then uses that to get the right reader class for the file so it can be read. From there it does some processing and ultimately returns an instance of a class that represents that file. **This is the other part of the problem that I ignored the first time. When it strips the extension out it only takes what's after the last dot, so once again `file.md.html` is mapped to the `html` reader and not the `md.html` one.**

### Concerns/Questions

* We can use `Pathlib`s `suffixes()` method to break any file name into a list of suffixes, and then join them to get the full suffix list. But if someone has file names with dots in it then it will break. So we need to either not allow dots in file names, or not allow compound file extensions, or find a way to deal with both.
* In terms of ways to deal with both, we could have a setting `ALLOW_COMPOUND_FILE_EXTENSIONS=False` with a big warning that if you set it equal to true you can't have file names with dots anywhere in your Article Path or it will break. And then in the code we can account for both scenarios.
* The other thing I was thinking was maintaining some sort of internal list of allowed compound file extensions and referencing against it somehow when we're reading and selecting files.

## 01.02.21 More on compound extensions

Talking with the other contributors online I'm seeing how this could work in general. The most naive and probably simplest implementation would be to check every possible combination of filename + extension for a reader match. So for `some.file.with.dots.md.html` we'd check 

```
some + .file.with.dots.md.html
some.file + .with.dots.md.html
some.file.with + .dots.md.html
some.file.with.dots + .md.html
some.file.with.dots.md + .html
```

until we find the first match. (Alternatively, we could reverse the order, as a compound file extension is likely more rare and in the average case would result in less checks, at the expense of code complexity.)

The better but most complicated solution is to build a "tree of readers" based on the extensions that we know we're able to match and then for each file, walk down the tree until we can no longer match. So if we're looking for files ending in `.rst`, `.html`, `.md.html`, `.baz`, `.foo.bar.baz` we'd build a tree like

```
readers = {
		'.rst': {
			'': RSTReader
		},
    '.html': {
        '': HTMLReader,
        '.md': MDHTMLReader,
    },
    '.baz': {
    		'': BAZReader,
    		'.bar': {
    				'.foo': FOOReader
    		}
    }
```

Of the two this is by far the most performant solution.

## 01.06 Getting closer on compound extensions

So I've now successfully built the reader tree and need to think about the next step in the process. For one, I need to traverse the tree and initialize a class object for each of these readers. Then, the function `read_file()` is called on each file in a directory. `read_file()` ultimately needs to be able to retrieve the already initialized class from the dict. This last part feels like the hard part.

We don't know a files extension ahead of time. Basically, we start with everything after the last dot. This is the classic idea of an extension. Look for it in the dict. If found, get the next set of letters until dot. Look for it in the next layer dict. If found, etc. If not found, check for `''` in that dict. If found, use that class. If not found, don't know how to parse.

Ideally this could be abstracted away to some level so we could just "pass in" the file and get a reader class in return. We also need to support people hardcoding in a `fmt` in the `read_file()` function.

Hmm. Do we nest a class in `Readers`? Or put a class in `utils.py`? That could be good as we could keep a lot of the messiness out of the `Reader` class. If I could make this class behave like a dict it could make things simpler on the `Reader` class end...

## 01.10 File names vs extensions

Looking around I'm realizing that there are many places in the code where we separate out the file name from the extension. A problem is that with compound file extension support, the file name vs the extension now changes based on the reader tree. 

* in `contents.py` the `Content` class creates a slug for each file that's based off of the filename. For a `.md.html` file the `.md` would be included in the filename and not the extension.
* The `SourceFileGenerator` class pulls the file name using `os.path.splitext` so same problem
* In `paginator.py` the `Page` class splits a file using `splitext`. I *think* this is ok as writers deal with `html` pages that have already been generated.
* In `urlwrappers.py` the `_from_settings` function returns URL information as defined in settings. It has a `get_page_name` flag that, when True, returns URL without anything after `{slug}`, so if `CATEGORY_URL="cat/{slug}.html"` the function returns `"cat/{slug}"`. This should be ok as well bc I this happens after generation.

I think my reader tree class needs a new method like `file_has_reader()` or something that returns T or F based on whether the files extension is in the reader tree. Since this change ties reader tree state to extension anyways. And most of the time when we're looking to retrieve the extension it makes that check.


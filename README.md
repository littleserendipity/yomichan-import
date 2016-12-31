# Yomichan-Import #

Yomichan Import allows users of the [Yomichan for Chrome](https://foosoft.net/projects/yomichan-chrome) extension to import custom dictionary
files. It currently supports the following formats:

*   [JMdict](http://www.edrdg.org/jmdict/edict_doc.html)
*   [JMnedict](http://www.edrdg.org/enamdict/enamdict_doc.html)
*   [KANJIDIC2](http://www.edrdg.org/kanjidic/kanjd2index.html)
*   [EPWING](https://ja.wikipedia.org/wiki/EPWING)
    *       [Daijirin](https://en.wikipedia.org/wiki/Daijirin) (三省堂　スーパー大辞林)

Yomichan Import is continuously being expanded to support other EPWING dictionaries based on user demand. This is a
mostly non-technical and (although laborious) process that requires writing regular expressions and creating font
tables; volunteer contributions are welcome.

## Installation ##

Yomichan Import is currently available for Windows and Linux; MacOS X executables will be released at a later date.

*   [yomichan-import_windows.zip](https://foosoft.net/projects/yomichan-import/dl/yomichan-import_windows.zip)
*   [yomichan-import_linux.tar.gz](https://foosoft.net/projects/yomichan-import/dl/yomichan-import_linux.tar.gz)

## Usage ##

Yomichan Import is a simple command line application. When invoked without any arguments (or executed with `--help`),
Yomichan Import will output usage instructions:

```
Usage: yomichan-import [options] input-path [output-dir]
https://foosoft.net/projects/yomichan-import/

Parameters:
  -format string
    	dictionary format [edict|enamdict|kanjidic|epwing]
  -port int
    	port to serve dictionary JSON on (default 9876)
  -pretty
    	output prettified dictionary JSON
  -serve
    	serve dictionary JSON for extension
  -stride int
    	dictionary bank stride (default 10000)
  -title string
    	dictionary title
```

In the vast majority of cases it is enough to simply provide the path to the dictionary resource you wish to process,
without explicitly specifying a format. Yomichan Import will attempt to automatically determine the format of the
dictionary based on the contents of the path:

| Format       | Resource                             |
| ------------ | ------------------------------------ |
| **edict**    | file named `JMDict_e.xml`            |
| **enamdict** | file named `JMNedict.xml`            |
| **kanjidic** | file named `kanjidic2.xml`           |
| **epwing**   | directory with file named `CATALOGS` |

For example, if you wanted to process an EPWING dictionary titled Daijirin, you could do so with the following command
(shown on Linux):

```
$ ./yomichan-import dict/Kokugo/Daijirin/
```

Yomichan Import will now begin the conversion process, which can take a couple of minutes to complete:

```
2016/12/29 17:12:12 converting 'dict/Kokugo/Daijirin/' to '/tmp/yomichan_tmp_825860502' in 'epwing' format...
```

After dictionary processing is complete, Yomichan Import will start a local HTTP server to enable the Yomichan for
Chrome extension to retrieve the dictionary data. Users of Windows will likely see a firewall nag dialog at this point;
access must be granted in order to make dictionary data accessible to the extension.

```
2016/12/29 17:12:20 starting dictionary server on port 9876...
```

As a final step, open the Yomichan for Chrome options dialog and choose the *Local dictionary* item in the dictionary
importer drop-down menu. When you see that `http://localhost:9876/index.json` displayed in the address text-box, you can
press the *Import* button to begin the import process. Once the imported dictionary is displayed on the options screen,
it is safe to terminate the Yomichan Import tool.

## License ##

MIT

# Outline

The `TMTOOLS` plug-in is a conglomeration of shell commands which provide the user with advanced functionality to interact with TextMate.<br><br>
It is based on code fragments of tm_dialog 1.x and 2.x. Furthermore I was inspired by: Allan Odgaard, Joachim M&aring;rtensson, Chris Thomas, Ciar&aacute;n Walsh, and H&aring;kan Waara.

<div style="border:1px solid red;padding:3mm;"><b><i>Important Note:</i></b><br/><br/>

The <code>TMTOOLS</code> plug-in calls undocumented TextMate functions without an appropriate API.<br><br>&nbsp;&nbsp;&nbsp;&nbsp;<font color=#DF0000> The usage of that plug-in is at your own risk!</font>
</div>

&nbsp;

# Usage

To use the `TMTOOLS` plug-in write a shell command:
<pre style="background-color:grey;"> "$TMTOOLS" verb command {options}
 ... | "$TMTOOLS" verb command -</pre>
 For help type the following into an empty line of a TextMate document and press CTRL+R
 
<pre style="background-color:grey;"> "$TMTOOLS" help me</pre>

`verb := [ get | set | move | insert | call | goto | show | close | reload | delete | scroll | complete | use | do | help ]`

The plug-in sets an environment variable "$TMTOOLS" automatically. "TMTOOLS" only interacts with TextMate if you invoke it from a document, meaning this plug-in doesn't work with the `Bundle Editor`, `Web Preview`, etc. in order to avoid unstableness.

`options` must be written as 'Property List'. If the property list will be piped to TMTOOLS one has to write a - (a hyphen) to tell TMTOOLS to read the options on STDIN.

Each value given in the Property List will be evaluated as **string** and converted internally if needed.

## version

In order to get <code>TMTOOLS</code>' version type
<pre style="background-color:grey;">"$TMTOOLS" -v</pre>

**Bash Example**

***It checks whether TMTOOLS is installed and if the current version is greater or equal 0906. If not it will download the plug-in.***

<pre style="background-color:grey;">			if [ -z &quot;$TMTOOLS&quot; ]; then
				open &quot;http://email.eva.mpg.de/~bibiko/downloads/textmate/TMTools.tmplugin.zip&quot;
				exit_discard
			fi
			
			VERSION=$(&quot;$TMTOOLS&quot; -v)

			if [ $VERSION -lt 906 ]; then
				open &quot;http://email.eva.mpg.de/~bibiko/downloads/textmate/TMTools.tmplugin.zip&quot;
				exit_discard
			fi</pre>

# Syntax

##get

In general `get` asks Textmate for something.

###`allBookmarks`

 * It will list all bookmarks in the current document as property list with `line` = `line number` and `name` = `line content`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: line number:line content
 * If no bookmarks are set it returns `"no bookmarks"`.
 * ***Hint*** This command manipulates the caret! After the command the caret will be set at the original position automatically.
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" get allBookmarks
			echo -n "{format=plain;}" | "$TMTOOLS" get allBookmarks -</pre>

###`allBundleItems`

 * It will list all **bundle items** stored in the bundle editor as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allBundleItems</pre>

###`allBundles`

 * It will list all available **bundles** stored in the bundle editor as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allBundles</pre>

###`allCommands`

 * It will list all available **commands** stored in the bundle editor as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allCommands</pre>

###`allEnvironmentVariables`

 * It will list all available environment variables as property list with `key` = `name` and `string` = `value`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB value
 * With the option <span  style="background-color:grey;">'{variables=(X,Y,Z);}'</span> it outputs only the values for the keys X, Y, and Z. If a key does not occur an empty line will be outputted. `variables` must be an array.
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" get allEnvironmentVariables '{format=plain;}'
			"$TMTOOLS" get allEnvironmentVariables '{variables=(TM_LINE_NUMBER, TM_SELECTED_TEXT, TM_SCOPE);}'</pre>

###`allGrammars`

 * It will list all available **grammars** stored in the bundle editor as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allGrammars</pre>

###`allPreferences`

 * It will list all available **Preferences** stored in the bundle editor as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allPreferences</pre>

###`allSnippets`

 * It will list all available **snippets** stored in the bundle editor as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allSnippets</pre>

###`allSymbols`

 * It will list all available **symbols** within the current document as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: line number:line content
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allSymbols</pre>

###`allTemplates`

 * It will list all available **templates** stored in the bundle editor as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get allTemplates</pre>

###`antiAliasEnabled`

 * Get the boolean value (0/1) for `antiAliasEnabled` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get antiAliasEnabled</pre>

###`autoIndent`

 * Get the boolean value (0/1) for `autoIndent` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get autoIndent</pre>

###`bundlePathsForUUID '{uuid="XXX"}'`

 * Get all path names of an installed bundle as property list which has the UUID = XXX.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list
 * With the option <span  style="background-color:grey;">'{shellEscape;}'</span> all SPACES are replaced by '\ '
 * With the option <span  style="background-color:grey;">'{urlEscape;}'</span> all file names are escaped for using them in URLs 
 * With the option <span  style="background-color:grey;">'{format=mate;}'</span> all file names are enclosed in single quotes and separeted by SPACES.  
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get bundlePathsForUUID '{uuid="0D42D5EE-5800-40A6-AD0E-29C3F56FAF16";}'</pre>

###`completionListFor '{prefix="XXX"}'`

 * It will list all available completion suggestions for the `prefix` = XXX as property list based on the current `WordChars`
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" get completionListFor '{prefix="Text"}'
			echo -n "prefix=$TM_CURRENT_WORD;format=plain;" | "$TMTOOLS" get completionListFor -</pre>

###`contentOfWindow '{pathContains="XXX"}'`

 * It returns the **current** content of that document window whose path name contains XXX.
 * If there are more than one windows opened containing XXX it will return the content of the first opened one.
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get contentOfWindow '{pathContains="MyDocument"}'</pre>

###`currentCharacter`

 * Get the character **left** of the caret 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentCharacter</pre>

###`currentFontName`

 * Get the current **font name** in the document 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentFontName</pre>

###`currentFontSize`

 * Get the current **font size** in the document 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentFontSize</pre>

###`currentGrammarUUID`

 * Get the current **UUID** for the active grammar used in the document 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentGrammarUUID</pre>

###`currentGrammar`

 * Get the current **grammar** used in the document 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentGrammar</pre>

###`currentSpellCheckerLanguage`

 * Get the current spell checker language 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentSpellCheckerLanguage</pre>

###`currentStyleSheet`

 * Get the **style sheet** specified in the current theme as property list
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentStyleSheet</pre>

###`currentSymbol`

 * Get the current **symbol** of the caret's position
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentSymbol</pre>

###`currentWordChars`

 * Get the current **WordChars** of TextMate's preferences pane
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get currentWordChars</pre>

###`currentWord`

 * Get the current **word** relative to the caret
 * Its behaviour is a bit different from the built-in variable `TM_CURRENT_WORD`
  * If no `delimiter` is specified it recognizes a ***word*** if it is enclosed by SPACES
  * With the option `delimiter` you can specify the **word boundaries**
  * With the option `only` = **head** it will output only these characters which are **before** the caret of the current word
  * With the option `only` = **tail** it will output only these characters which are **after** the caret of the current word
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" get currentWord '{delimiter=" ()";}'
			"$TMTOOLS" get currentWord '{only=head;}'
			"$TMTOOLS" get currentWord '{only=tail;delimiter=" ():.";}'</pre>

###`defaultBundle`

 * It returns the user's home bundle as property list with `name` and `UUID`.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list: name TAB UUID
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get defaultBundle</pre>

###`foldingsEnabled`

 * Get the boolean value (0/1) for `foldingsEnabled` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get foldingsEnabled</pre>

###`freehandMode`

 * Get the boolean value (0/1) for `freehandMode` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get freehandMode</pre>

###`frontMostDocument`

 * Get the file path or the display title (if untitled) of the **current** front most floating document.
 * With the option <span  style="background-color:grey;">'{urlEscape;}'</span> the file path will be escaped for using it in URLs  
 * ***Hint*** This command does not work inside of projects.
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get frontMostDocument</pre>

###`hardWrap`

 * Get the boolean value (0/1) for `hardWrap` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get hardWrap</pre>

###`hasSelection`

 * Get the boolean value (0/1) if there is something selected within the current document 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get hasSelection</pre>

###`indentedPaste`

 * Get the boolean value (0/1) for `indentedPaste` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get indentedPaste</pre>

###`isContinuousSpellCheckingEnabled`

 * Get the boolean value (0/1) for `isContinuousSpellCheckingEnabled` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get isContinuousSpellCheckingEnabled</pre>

###`isDocumentEdited`

 * Get the boolean value (0/1) if the current document is marked as `isDocumentEdited` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get isDocumentEdited</pre>

###`lineHead`

 * Get the content of the current line before the caret
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get lineHead</pre>

###`lineTail`

 * Get the content of the current line after the caret
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get lineTail</pre>

###`mainScreenSize`

 * Get the **size** of the screen as {x, y, width, height}
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get mainScreenSize</pre>

###`openDocumentFiles`

 * Get all accociated file names of all opened document windows as property list.
 * If a file was not saved yet its display title will outputted
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list
 * With the option <span  style="background-color:grey;">'{shellEscape;}'</span> all SPACES are replaced by '\ '
 * With the option <span  style="background-color:grey;">'{urlEscape;}'</span> all file names are escaped for using them in URLs 
 * With the option <span  style="background-color:grey;">'{format=mate;}'</span> all file names are enclosed in single quotes and separeted by SPACES. 
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" get openDocumentFiles
			"$TMTOOLS" get openDocumentFiles '{format=plain;shellEscape;}'
			eval mate `"$TMTOOLS" get openDocumentFiles '{format=mate;}'` untitled</pre>
 * ***Hint for third example*** This command forces TextMate to open all floating document windows in a new project even it is only one document (by adding 'untitled'). If one wants to close all floating windows in beforehand, one can do one of the following:
<pre style="background-color:grey;">			L=$("$TMTOOLS" get openDocumentFiles '{format=mate;}')
			"$TMTOOLS" do closeAllFloatingDocumentWindows
			eval mate "$L" untitled</pre>

<pre style="background-color:grey;">			FRONTMOSTFILE=$("$TMTOOLS" get frontMostDocument '{urlEscape;}')
			eval mate `"$TMTOOLS" get openDocumentFiles '{format=mate;}'` untitled
			open "txmt://open/?url=file://$FRONTMOSTFILE"
			"$TMTOOLS" close allFloatingDocumentWindows
			open "txmt://open/?url=file://$FRONTMOSTFILE"</pre>

###`openProjectFiles`

 * Get all file names which are opened in the **current project** as property list.
 * ***Hint*** This command does not work outside of a project.
 * With the option <span  style="background-color:grey;">'{format=plain;}'</span> one gets a plain list
 * With the option <span  style="background-color:grey;">'{shellEscape;}'</span> all SPACES are replaced by '\ '
 * With the option <span  style="background-color:grey;">'{urlEscape;}'</span> all file names are escaped for using them in URLs 
 * With the option <span  style="background-color:grey;">'{format=mate;}'</span> all file names are enclosed in single quotes and separeted by SPACES.  
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get openProjectFiles
			"$TMTOOLS" get openProjectFiles '{format=plain;urlEscape;}'</pre>

###`overwriteMode`

 * Get the boolean value (0/1) for `overwriteMode` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get overwriteMode</pre>

###`positionUnderCaret`

 * Get the graphical position under the caret as x,y 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get positionUnderCaret</pre>

###`selectionXML`

 * Get the current scope written as XML for the selection
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get selectionXML</pre>

###`showBookmarksInGutter`

 * Get the boolean value (0/1) for `showBookmarksInGutter` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get showBookmarksInGutter</pre>

###`showLineNumbersInGutter`

 * Get the boolean value (0/1) for `showLineNumbersInGutter` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get showLineNumbersInGutter</pre>

###`showSoftWrapInGutter`

 * Get the boolean value (0/1) for `showSoftWrapInGutter` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get showSoftWrapInGutter</pre>

###`smartTyping`

 * Get the boolean value (0/1) for `smartTyping` meaning the auto-pair status
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get smartTyping</pre>

###`softTabs`

 * Get the boolean value (0/1) for `softTabs` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get softTabs</pre>

###`softWrap`

 * Get the boolean value (0/1) for `softWrap` 
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get softWrap</pre>

###`standardUserDefaults '{key="XXX"}'`

 * Get the value for key in the current `standardUserDefaults`
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get standardUserDefaults '{key=OakTextViewNormalFontName;}'</pre>

###`suggestedExtensionForDocument`

 * Get the suggested extension for the current document based on the given language
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get suggestedExtensionForDocument</pre>

###`textString`

 * Get the **current** content of the entire document
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get textString</pre>

###`textXML`

 * Get the **current** content of the entire document with all scopes written as XML
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get textXML</pre>

###`uuidFor '{name="XXX"; kind="YYY";}'`

 * Get the UUID for the item's name XXX as YYY.
 * If XXX = "defaultBundle" and YYY = "bundle" it returns the UUID for the user's home bundle.
 * YYY := `bundle` | `command` | `snippet` | `language` | `template` | `preference` | `macro`
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" get uuidFor '{name="Lorem ipsum"; kind="snippet";}'
			"$TMTOOLS" get uuidFor '{name="defaultBundle"; kind="bundle";}'</pre>

###`windowOriginAndSize`

 * Get the origin and size of the current document' window as {x,y,width,height}
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get windowOriginAndSize</pre>

###`wordHead`

 * Get all characters of `TM_CURRENT_WORD` **before** the caret's position
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get wordHead</pre>

###`wordTail`

 * Get all characters of `TM_CURRENT_WORD` **after** the caret's position
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" get wordTail</pre>

##set

In general `set` sets something in the current document.

###`antiAliasEnabled '{to=X;}'`

 * X := `yes` or `no`
 * It sets `antiAliasEnabled` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set antiAliasEnabled '{to=no;}'</pre>

###`autoIndent '{to=X;}'`

 * X := `yes` or `no`
 * It sets `autoIndent` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set autoIndent '{to=no;}'</pre>

###`bookmarks '{to=(X,Y,...);}'`

 * It sets bookmarks for the lines X,Y,...
 * The key `to` must be an array of positive integers
 * ***Hint*** This command manipulates the caret! After the command the caret will be set at the original position automatically.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set bookmarks '{to=(5,10,15,20,25);}'</pre>

###`caretTo '{line=X;index=Y;}'`

 * Set the caret to line X and character index Y (***not column***)
 * If index is not set it places the caret to the beginning of the line X
 * If index = "end" it places the caret at the end of line X
 * If line is not set the current line number is used
 * See also [move caretBy]
	
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" set caretTo '{line=2;index=4;}'
			"$TMTOOLS" set caretTo '{index=end;}'
			"$TMTOOLS" set caretTo '{line=2;}'
			"$TMTOOLS" set caretTo '{index=4;}'</pre>

###`continuousSpellCheckingEnabled '{to=X;}'`

 * X := `yes` or `no`
 * It sets `continuousSpellCheckingEnabled` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set continuousSpellCheckingEnabled '{to=no;}'</pre>

###`expandSnippetsOnTab '{to=X;}'`

 * X := `yes` or `no`
 * It sets `expandSnippetsOnTab` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set expandSnippetsOnTab '{to=no;}'</pre>

###`foldingsEnabled '{to=X;}'`

 * X := `yes` or `no`
 * It sets `foldingsEnabled` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set foldingsEnabled '{to=yes;}'</pre>

###`fontName '{to="XXX";}'`

 * It sets the current font to XXX
 * ***Hint*** XXX must be a valid font name. Otherwise MAC OSX will set it to a system font.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set fontName '{to="Monaco";}'</pre>

###`fontSize '{to=X;}'`

 * It sets the current font size to X
 * X must be a valid integer greater than 1
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set fontSize '{to=14;}'</pre>

###`freehandedEdit '{to=X;}'`

 * X := `yes` or `no`
 * It sets `freehandedEdit` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set freehandedEdit '{to=yes;}'</pre>

###`frontMostDocument '{to="XXX";}'`

 * Activates that floating document window whose file path or display title (if untitled) is XXX.
 * With the option <span style="background-color:grey;">'{orderFront=no;}'</span> one activates that document but the displayed document order won't be changed. Its default is `yes`.
 * Relative file paths are omitted.
 * XXX is case-sensitive.
 * ***Hint*** This command does not work for projects. For projects one can use `get openProjectFile 'format=plain;urlEscape;'` and TextMate's URL scheme `open "txmt://open/?url=file://$FILENAME"`
 * **Fictive Example**

<pre style="background-color:grey;">			"$TMTOOLS" set frontMostDocument '{to="/Users/TextMate/index.html";}'</pre>

###`grammar '{to="XXX";}'`

 * It sets the current `grammar`, used for syntax highlighting, to XXX
 * XXX is a valid string for a grammar stored in the bundle editor; if not the command will be ignored
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set grammar '{to="Plain Text";}'</pre>

###`indentedPaste '{to=X;}'`

 * X := `yes` or `no`
 * It sets `indentedPaste` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set indentedPaste '{to=no;}'</pre>

###`lineNumbering '{to=X;}'`

 * X := `yes` or `no`
 * It sets the line numbering to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set lineNumbering '{to=no;}'</pre>

###`overwriteMode '{to=X;}'`

 * X := `yes` or `no`
 * It sets `overwriteMode` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set overwriteMode '{to=yes;}'</pre>

###`selectionTo '{line=X;column=Y;length=Z}'`

 * Set the selection to `line` X and `column` Y from the caret's position
 * The values of the keys `Line` and `column` could also be negative
 * If `length` is set it will select Z characters relative from the caret (could also be negative)
 * If `length` is set `line` and `column` will be ignored
 * If `line` is not set the current line number is used
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" set selectionTo '{line=2;column=3;}'
			"$TMTOOLS" set selectionTo '{length=-2;}'</pre>

###`selectionToCurrentScope`

 * It selects the current scope specified by the caret's position
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set selectionToCurrentScope</pre>

###`showBookmarksInGutter '{to=X;}'`

 * X := `yes` or `no`
 * It sets `showBookmarksInGutter` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set showBookmarksInGutter '{to=yes;}'</pre>

###`showInvisibles '{to=X;}'`

 * X := `yes` or `no`
 * It sets `showInvisibles` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set showInvisibles '{to=yes;}'</pre>

###`showSoftWrapInGutter '{to=X;}'`

 * X := `yes` or `no`
 * It sets `showSoftWrapInGutter` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set showSoftWrapInGutter '{to=yes;}'</pre>

###`smartTyping '{to=X;}'`

 * X := `yes` or `no`
 * It sets `smartTyping` &mdash; the auto-pair function &mdash; **temporally** for the **current** document to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set smartTyping '{to=no;}'</pre>
###`softTab '{to=X;}'`

 * X := `yes` or `no`
 * It sets `softTab` to `yes` or `no`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set softTab '{to=yes;}'</pre>

###`spellCheckerLanguage '{to=XXX;}'`

 * It sets the current spell checker language
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set spellCheckerLanguage '{to=en_GB;}'</pre>

###`tabWidth '{to=X;}'`

 * It sets the current used TAB size to X
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set tabWidth '{to=8;}'</pre>

###`windowOrigin '{x=X;y=Y;}'`

 * It sets the current window origin to X, Y (0, 0 := bottom left corner)
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set windowOrigin '{x=100;y=150;}'</pre>

###`windowSize '{width=X;height=Y;}'`

 * It sets the current window size to width X and height Y
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set windowSize '{width=100;height=200;}'</pre>

###`wordChars '{to="XXX";}'`

 * It sets `wordChars` used for `TM_CURRENT_WORD` to XXX
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" set wordChars '{to="_$";}'</pre>

##move

In general `move` moves something in the current document.

###`move caretBy '{line=X;index=Y;}'`

 * It moves the caret relative to the actual position
 * X and Y could also be negative
 * See also [set caretTo]
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" move caretBy '{index=-2;}'</pre>

###`move caretToCenter`

 * It moves the caret to the middle line of the current window
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" move caretToCenter</pre>

###`move selectionBy '{line=X;column=Y;}'`

 * It expands the selection relative to the actual caret position
 * X and Y could also be negative
 * See also [set selectionTo]
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" move selectionBy '{line=-2;column=-3;}'</pre>

##insert

In general `insert` inserts something in the current document.

###`snippet '{value="XXX";}'`

 * It inserts XXX as snippet
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" insert snippet '{value="\${1:Hello} - \${2:World}\${3:}";}'</pre>

###`text '{value="XXX";}'`

 * It inserts XXX as plain text
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" insert text '{value="Hello World";}'</pre>

##call

In general `call` executes an item which is stored as bundle item.

###`bundleItem '{uuid="XXX";}'`

 * It executes any bundle item identified via the internal UUID = XXX
 * ***Hint*** UUIDs depend on the individual TextMate installation and are not system independent
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" call bundleItem '{uuid="DA0A4E77-5F16-11D9-B9C3-000D93589AF6";}'</pre>

###`command '{name="XXX";}'`

 * It executes a **bundle command** with the name XXX
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" call command '{name="Add Line Numbers to Document / Selection";}'</pre>

###`command PLIST`

 * It executes that command which is specified in PLIST
 * **Example**

<pre style="background-color:grey;">			"$TMTOOLS" call command '
				&lt;dict&gt;
					&lt;key&gt;beforeRunningCommand&lt;/key&gt;
					&lt;string&gt;nop&lt;/string&gt;
					&lt;key&gt;command&lt;/key&gt;
					&lt;string&gt;
						A=$(cat)
						echo -e &quot;$A&quot;
						echo -en &quot;Hello World&quot;
					&lt;/string&gt;
					&lt;key&gt;input&lt;/key&gt;
					&lt;string&gt;selection&lt;/string&gt;
					&lt;key&gt;fallbackInput&lt;/key&gt;
					&lt;string&gt;word&lt;/string&gt;
					&lt;key&gt;output&lt;/key&gt;
					&lt;string&gt;showAsTooltip&lt;/string&gt;
				&lt;/dict&gt;
				'</pre>

###`macro '{name="XXX";}'`

 * It executes a **bundle macro** with the name XXX
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" call macro '{name="Delete Line";}'</pre>

###`macro PLIST`

 * It executes a dynamic generated macro stored as a property list PLIST
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" call macro '{
				commands = (
					{command = "moveWordRight:"; },
					{command = "moveWordLeftAndModifySelection:"; },
					{command = "moveWordLeftAndModifySelection:"; },
					{command = "moveWordLeftAndModifySelection:"; }
				);
			}'
			
			"$TMTOOLS" call macro '&lt;?xml version="1.0" encoding="UTF-8"?&gt;
			&lt;!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/#PropertyList-1.0.dtd"&gt;
			&lt;plist version="1.0"&gt;
			&lt;dict&gt;
				&lt;key&gt;commands&lt;/key&gt;
				&lt;array&gt;
					&lt;dict&gt;
						&lt;key&gt;argument&lt;/key&gt;
						&lt;dict&gt;
							&lt;key&gt;action&lt;/key&gt;
							&lt;string&gt;replaceAll&lt;/string&gt;
							&lt;key&gt;findInProjectIgnoreCase&lt;/key&gt;
							&lt;false/&gt;
							&lt;key&gt;findString&lt;/key&gt;
							&lt;string&gt;Textmate is good&lt;/string&gt;
							&lt;key&gt;ignoreCase&lt;/key&gt;
							&lt;true/&gt;
							&lt;key&gt;replaceAllScope&lt;/key&gt;
							&lt;string&gt;document&lt;/string&gt;
							&lt;key&gt;replaceString&lt;/key&gt;
							&lt;string&gt;TextMate is extraordinary&lt;/string&gt;
							&lt;key&gt;wrapAround&lt;/key&gt;
							&lt;true/&gt;
						&lt;/dict&gt;
						&lt;key&gt;command&lt;/key&gt;
						&lt;string&gt;findWithOptions:&lt;/string&gt;
					&lt;/dict&gt;
				&lt;/array&gt;
			&lt;/dict&gt;
			&lt;/plist&gt;
			'</pre>

###`snippet '{name="XXX";}'`

 * It executes a **bundle snippet** with the name XXX
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" call snippet '{name="Lorem ipsum";}'</pre>

###`template '{name="XXX";}'`

 * It executes a **bundle template** with the name XXX
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" call template '{name="From Clipboard";}'</pre>

##goto

In general `goto` goes to an item.

###`nextBookmark`

 * It goes to the next bookmark
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" goto nextBookmark</pre>

###`nextSnippetField`

 * It goes to the next snippet field
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" goto nextSnippetField</pre>

###`prevBookmark`

 * It goes to the previous bookmark
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" goto prevBookmark</pre>

###`prevSnippetField`

 * It goes to the previous snippet field
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" goto prevSnippetField</pre>

##show

In general `show` opens a panel or window.

###`alertSheet`

 * It opens a alert window as sheet bound to the current window.
 * The returned value is the index beginning with 0 of the pressed button - starting from the right.
 * Possible options:
  * <span style="background-color:grey;">informativeText="XXX";</span>
  * <span style="background-color:grey;">messageTitle="XXX";</span>
  * <span style="background-color:grey;">buttonTitles=("X","Y");</span>  
  * <span style="background-color:grey;">alertStyle="XXX";</span> whereby XXX can be: `informational`, `critical`, or `warning`
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show alertSheet '{informativeText="This is only a info";
				messageTitle="This is a message";buttonTitles=(OK,Cancel);alertStyle=critical;}'</pre>

###`bundleEditor`

 * It opens the Bundle Editor window
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show bundleEditor</pre>

###`bundleMenu`

 * It shows the Bundle Menu (the Gear Menu - CTRL+ESC)
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show bundleMenu</pre>

###`contextMenu`

 * It shows the Context Menu for the current document (right mouse click)
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show contextMenu</pre>

###`openPanel`

 * It opens the internal **Open** panel
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show webPreview</pre>

###`printPanel`

 * It opens the internal **Print** panel
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show printPanel</pre>

###`projectDrawer '{onEdge=X;}'`

 * It opens the internal project drawer on a specific edge X
 * X = 0 : on the left side
 * X = 1 : at the bottom
 * X = 2 : on the right side
 * X = 3 : at the top
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show projectDrawer '{onEdge=0;}'</pre>

###`saveAsPanel`

 * It opens the internal **Save As** panel
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show saveAsPanel</pre>

###`webPreview`

 * It opens the Web Preview window
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" show webPreview</pre>

##close

In general `close` closes windows.

###`allAuxiliaryWindows`

 * It closes all auxiliary windows which belong to TextMate like the Bundle Editor, Find Panel, HTML Output Windows, etc.
 * With the option <span style="background-color:grey;">'{except=(X,Y,Z);}'</span> one can specify which auxiliary windows won't be closed (***see example 2***).
 * **Examples**

<pre style="background-color:grey;">			 "$TMTOOLS" close allAuxiliaryWindows
			 "$TMTOOLS" close allAuxiliaryWindows '{except=(OakFindPanel, OakBundleEditorWindow);}'</pre>

###`allFloatingDocumentWindows`

 * It closes all open floating document windows and asks for saving if a document is unsaved. All project windows and auxiliary windows will be untouched.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" close allFloatingDocumentWindows</pre>

###`allWindows`

 * It closes all open windows and asks for saving if a document is unsaved.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" close allWindows</pre>

##reload

In general `reload` reloads something.

###`allBundles`

 * It reloads all bundles
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" reload allBundles</pre>

##delete

In general `delete` deletes something.

###`allBookmarks`

 * It deletes all bookmarks set in the current document.
 * ***Hint*** This command manipulates the caret! After the command the caret will be set at the original position automatically.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" delete allBookmarks</pre>

##scroll

In general `scroll` scrolls the window content of the current document.

###`columnLeft`

 * It scrolls the window content one column leftwards
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" scroll columnLeft</pre>

###`columnRight`

 * It scrolls the window content one column rightwards
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" scroll columnRight</pre>

###`lineDown`

 * It scrolls the window content one line downwards
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" scroll lineDown</pre>

###`lineUp`

 * It scrolls the window content one line upwards
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" scroll lineUp</pre>

###`windowToCaret`

 * It scrolls the window content to center the selection/caret
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" scroll windowToCaret</pre>

##complete

In general `complete` will show a pull-down menu of suggestions. While typing this list will be updated automatically. This code is mainly taken from Joachim M&aring;rtensson. Changes are only done for the event handling. This pull-down menu will not interfere with tm_dialog.

###`path`  

 * It allows the user to input a UNIX path or file name.
 * If one invokes this completion without a given absolute path at the caret it will start at `TM_DIRECTORY` resp. at root `/` if the document is not yet saved.
 * If one invokes this completion relatively **and** the parser finds `TM_BUNDLE_SUPPORT` or `TM_SUPPORT_PATH` as a valid path before the caret it will start at this position.
 * Allowed are also relative path names beginning with `.` or `..`, and `~` indicating the user's $HOME directory.
 * SPACES are escaped automatically.
 * If one deletes a SPACE it will also delete the according escape sign `\`.
 * If the completion finds a distinct path it will complete the path name automatically.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" complete path</pre>

###`usingDictionary`

 * It asks the cocoa's spell checker dictionary for suggestions for the current word.
 * As default the language is set to TextMate's spell checker settings.
 * To change the language one has to set the key `lang` accordingly:
  * `en_US, en_GB, en_AU, en_CA, de, fr, fr_CA, es, it, nl, sv, nb, da, fi, pt` are attested
  * `zh-Hans, zh-Hant, ko, jp` are not yet attested
 * With the option <span  style="background-color:grey;">'{showInternalListFirst=yes;}'</span> TextMate's internal completion list is displayed first. Its default is `no`.
 * With the option <span  style="background-color:grey;">'{autocomplete=yes;}'</span> the command inserts the longest common prefix of the suggestions automatically. Its default is `no`.
 * If no spell checker is found en_US will be used.
 * **Examples**

<pre style="background-color:grey;">			 "$TMTOOLS" complete usingDictionary
			 "$TMTOOLS" complete usingDictionary '{lang="de_DE";}'
			 "$TMTOOLS" complete usingDictionary '{lang="da";showInternalListFirst=yes;}'</pre>

###`usingDocument`

 * It asks TextMate for the suggestions based on the internal document based completion list.
 * The current word will not be displayed
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" complete usingDocument</pre>

##use

In general `use` is an API to cocoa's objects.

###`nsstring '{initWith="XXX"; toGet="YYY"}'`

 * This command will init a NSString with the string XXX and returns the method YYY.
 * If the method asks for an argument one can pass it by using <span  style="background-color:grey;">'with="ZZZ";'</span>
 * Up to now it supports only NSString methods which requests for none or one argument.
 * As output types are supported: NSString, NSArray, NSDictionary, NSRange, BOOL, unichar, and int.
 * Read more for NSString [here](http://developer.apple.com/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html)
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" use nsstring '{initWith="&ouml;"; toGet="decomposedStringWithCanonicalMapping";}'
			"$TMTOOLS" use nsstring '{initWith="ö"; toGet="decomposedStringWithCanonicalMapping";}'
			"$TMTOOLS" use nsstring '{initWith="hello"; toGet="hasPrefix:"; with="he"; }'
			"$TMTOOLS" use nsstring '{initWith="hello"; toGet="length"; }'
			"$TMTOOLS" use nsstring '{initWith="hello"; toGet="rangeOfString:"; with="el"; }'
			"$TMTOOLS" use nsstring '{initWith="hello"; toGet="smallestEncoding"; }'
			"$TMTOOLS" use nsstring '{initWith="A:B:C"; toGet="componentsSeparatedByString:"; with=":"; }'
			"$TMTOOLS" use nsstring '{initWith="hello"; toGet="compare:"; with="hel"; }'
			"$TMTOOLS" use nsstring '{initWith="&#x305F;&#x3066;&#x3082;&#x306E;"; toGet="characterAtIndex:"; with="2"; }'
			"$TMTOOLS" use nsstring '{initWith="&#x305F;&#x3066;&#x3082;&#x306E;"; toGet="canBeConvertedToEncoding:"; with="8"; }'
			"$TMTOOLS" use nsstring '{initWith="/Users/TextMate"; toGet="lastPathComponent";}'
			"$TMTOOLS" use nsstring '{initWith="&#x305F;&#x3066;&#x3082;&#x306E;"; toGet="stringByAddingPercentEscapesUsingEncoding:"; with="4"; }'
			"$TMTOOLS" use nsstring '{initWith="%E3%81%9F%E3%81%A6%E3%82%82%E3%81%AE";
							toGet="stringByReplacingPercentEscapesUsingEncoding:"; with="4"; }'
			"$TMTOOLS" use nsstring '{initWith="{z=({a='b';});}"; toGet="propertyList";  }'
			"$TMTOOLS" use nsstring '{initWith="&#x44E;&#x440;&#x438;"; toGet="caseInsensitiveCompare:"; with="&#x42E;&#x440;&#x438;"; }'</pre>

##do

In general `do` executes something or implements an event handler.

###`centerWindow`

 * It centers the current window on the screen.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do centerWindow</pre>

###`checkUUID '{uuid="XXX";}'`

 * It returns `1` if XXX is **not** used by TextMate.
 * It returns `0` if XXX is used by TextMate.
 * ***Hint*** One can generate a new UUID by using the shell command `uuidgen`.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do checkUUID '{uuid="67CBC425-6A5B-4621-97A1-147C8CEC221A";}'</pre>

###`miniaturizeAllWindows`

 * It miniaturizes all windows.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do miniaturizeAllWindows</pre>

###`onKeyDownAfterDelay '{delay=T; shell="XXX"; }' or PLIST`

 * <font color=red>! experimental !</font>
 * It will execute the shell script XXX or the command which is passed as PLIST after a delay of K seconds after a keyDown event once.
 * The key `stopkey` is the integer value of a key event (e.g. 53 = ESC) which interrupts that event handler loop.
 * If no `stopkey` is passed the shell command or the command passed via PLIST will be executed only **once**. After that the keyDown loop handler will interrupt.
 * If a `stopkey` is passed and the event handler loop will be interrupted an acoustic signal (Plop) will be played.
 * With the option <span  style="background-color:grey;">'passEvent=no;'</span> the keyDown event will not be sent to TextMate (***be carefully***). Its default is `yes`.
 * If the shell script should be execute repeatedly without a further keyDown event one has to specify <span  style="background-color:grey;">'repeat=yes;'</span>. Its default is `no`.
 * If the shell script should be executed after invoking that command without to wait for the first keyDown event one has to specify <span  style="background-color:grey;">'startAtCall=yes;'</span>. Its default is `no`.
 * It is possible to install more than one keyDown handle, but use it up to now with caution!
 * ***Hint*** Be aware of shell escaping.
 * **Examples**

<pre style="background-color:grey;">			"$TMTOOLS" do onKeyDownAfterDelay "{delay=2;
					shell='\"$TMTOOLS\" call command \'{name=\"ShowCurrentScopeToolTip\";}\''; stopkey=53; }"
			"$TMTOOLS" do onKeyDownAfterDelay "{delay="0.5";
					shell='\"$TMTOOLS\" complete usingDictionary \"{lang=de_DE;}\"'; stopkey=53; }"
			"$TMTOOLS" do onKeyDownAfterDelay "{delay="1.5"; repeat=yes; startAtCall=no;
					shell='\"$TMTOOLS\" complete usingDocument'; stopkey=53; }"
			"$TMTOOLS" do onKeyDownAfterDelay "{delay="5"; repeat=no; startAtCall=no;
					shell='\"$TMTOOLS\" get textString > /tmp/\"$TM_FILENAME\".txmtbkp;say \"document was saved\"'; stopkey=53; }"
					
			"$TMTOOLS" do onKeyDownAfterDelay '
				&lt;dict&gt;
					&lt;key&gt;delay&lt;/key&gt;
					&lt;string&gt;2&lt;/string&gt;
					&lt;key&gt;beforeRunningCommand&lt;/key&gt;
					&lt;string&gt;nop&lt;/string&gt;
					&lt;key&gt;command&lt;/key&gt;
					&lt;string&gt;
						A=$(cat)
						echo -e &quot;$A&quot;
						echo -en &quot;Hello World&quot;
					&lt;/string&gt;
					&lt;key&gt;input&lt;/key&gt;
					&lt;string&gt;selection&lt;/string&gt;
					&lt;key&gt;fallbackInput&lt;/key&gt;
					&lt;string&gt;word&lt;/string&gt;
					&lt;key&gt;output&lt;/key&gt;
					&lt;string&gt;showAsTooltip&lt;/string&gt;
				&lt;/dict&gt;
'</pre>

###`openFileInNewWindow`

 * It opens the current project file in a new window.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do openFileInNewWindow</pre>

###`openFileWithFinder`

 * It opens the current project file with its linked default application.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do openFileWithFinder</pre>

###`redo`

 * It calls the function `redo`.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do redo</pre>

###`revealFileInFinder`

 * It reveals the current project file in Finder.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do revealFileInFinder</pre>

###`saveCurrentDocument`

 * <font color=red>! Do use that command with care for 'untitled' documents !</font>
 * It will save the current document.
 * If the document wasn't saved before &mdash; it is an 'untitled' document &mdash; the 'Save As Panel' will be displayed.
 * For 'untitled' documents it is possible to prevent the 'Save As Panel' by using the option <span style="background-color:grey;">'{avoidPanel;}'</span>. Thus this command will return '0' and the document **wont'be saved**! If the document was saved successfully it will return '1'.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do saveCurrentDocument '{avoidPanel;}'</pre>

<div style="border:1px solid red;padding:5mm;"><font color=red>Attention</font>: If one uses that command without the option `avoidPanel` <b>and</b> the current document is an 'untitled' document within of a script &mdash;not as the <b>last</b> command&mdash;, all following commands will be ignored! This also could lead that TextMate freezes and/or some modifications won't be saved! An other side-effect could be that TextMate registers more than one instance of the same file!<br/>To avoid such a behaviour it is always the best to use the option <code>avaoidPanel</code> and react to the return value.</div>

###`saveWithFilenameAndReopen '{name="XXX";}'`

 * It will save the current **floating** document window as XXX.
 * If the document could be saved successfully, it will reopen XXX in TextMate.
 * If XXX already exists a message will be returned and XXX won't be overwritten. With the option <span style="background-color:grey;">'{overwriteMode;}'</span> one can force TMTOOLS to overwrite the existing file XXX.
 * If XXX is not a valid file path or an other error occured only a error message will be returned.
 * ***Hint*** This command won't work inside of a project.
 * **Examples**

<pre style="background-color:grey;">			 "$TMTOOLS" do saveWithFilenameAndReopen '{name="/Users/TextMate/test.txt";overwriteMode;}'</pre>
<pre style="background-color:grey;">			 FILENAME="$TM_SELECTED_TEXT"
			 EXT=$("$TMTOOLS" get suggestedExtensionForDocument)
			 filename="$(CocoaDialog filesave --with-directory `dirname "$TM_FILEPATH"` --with-file "$FILENAME.$EXT" )"
			 if [ -n "$filename" ]; then
				 echo "{name=\""$filename"\";overwriteMode;}" |"$TMTOOLS" do saveWithFilenameAndReopen -
			 fi</pre>

###`undo`

 * It calls the function `undo`.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do undo</pre>

###`zoomWindow`

 * It zooms the current window.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" do zoomWindow</pre>

##help

###`me`

 * It displays TMTOOLS' help.
 * **Example**

<pre style="background-color:grey;">			 "$TMTOOLS" help me</pre>

# Release Notes

## Release 0900

 * first release 08 Oct 2007

## Release 0901

 * 12 Oct 2007
 * fixed: `do onKeyDownAfterDelay` only works on a TM text document; not on auxiliary panels etc.
 * added: to `do onKeyDownAfterDelay` an option `passEvent` yes or no
 * added: to `do onKeyDownAfterDelay` the possibility not only to execute a shell command but also a command specified by a PLIST
 * changed: `do onKeyDownAfterDelay` if no stopkey is given the shell script or the command will only execute once; after that the loop handler does not exist
 * added: `get currentCharacter`
 * added: to `call command` the possibility not only to call a saved tmCommand via its name but also to pass a plist containing command options
 * added: `do closeAllWindows`

## Release 0902

 * 16 Oct 2007
 * added: to `get allEnvironmentVariables` an option `variables` to output only these values
 * added: `close allAuxiliaryWindows` 
 * added: `close allFloatingDocumentWindows`
 * changed: `do closeAllWindows` to `close allWindows`
 * added: `get openDocumentFiles`
 * added: `set/get frontMostDocument`
 * added: to `get openProjectFiles` the output option `format=mate;`
 * changed: `get openProjectFiles` output option `escape` is renamed to `shellEscape`
 * added: `do openFileInNewWindow`
 * added: `do revealFileInFinder`
 * added: `do openFileWithFinder`
 * added: `show alertSheet`
 
## Release 0903

 * 24 Oct 2007
 * added: `get contentOfWindow`
 * added: `get/set smartTyping`
 * added: `get allBundles`
 * added: `get uuidFor`
 * added: `get defaultBundle`
 * added: `get allBundleItems`
 * added: `do checkUUID` 
 * added: `get bundlePathsForUUID`  
 
## Release 0904

 * 26 Oct 2007
 * added: `do saveWithFilenameAndReopen`
 * added: `get suggestedExtensionForDocument`

## Release 0905

 * 26 Oct 2007
 * added: `do saveCurrentDocument`
 
## Release 0906

 * 13 Nov 2007
 * added: `show bundleMenu`
 
# Contact

If there are further suggestions or bugs don't hesitate to mail me.

**26 October 2007 - Hans-J&ouml;rg Bibiko <bibiko@eva.mpg.de>**
	
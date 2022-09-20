# ChromeRemoteObject
This library allows to use JavaScript objects as native objects and adds an 'eval' method to [Chrome.ahk](https://github.com/G33kDude/Chrome.ahk)

`result := Chrome.Page.eval(expr)`

#### Example
```ahk
if (chromes := Chrome.findInstances())
    browser := {base:Chrome, debugPort:chromes.minIndex()}
else
    browser := new Chrome(a_desktop "\ChromeProfile")

page := browser.getPage()

doc := page.eval("document")
doc.querySelector("body").innerHTML := "
(ltrim
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
)"
doc.querySelectorAll("li")[0].innerHTML := "<a href='https://autohotkey.com'>AutoHotkey</a>"
doc.querySelectorAll("li")[3].innerHTML := "<a href='https://google.com'>Google</a>"

for i, elem in (doc.querySelectorAll("a"), text := "")
    text .= elem.innerText " (" elem.getAttribute("href") ")`n"
MsgBox % text

func := page.eval("(function(a, b) { return a + b })")
MsgBox % func.call("", 3, 5)

arr := page.eval("['hello', 'world']")
for i, v in (arr, text := "")
    text .= i ": " v "`n"
MsgBox % text

js := page.eval("window")
js.myVar := "ahk is awsome"
MsgBox % page.eval("myVar")
```

#### Recommended Fixes
`Call` method of file 'Chrome.ahk' at line 245
```ahk
while !this.responses[ID]
    Sleep 0 ; set delay to 0 for fast response
```

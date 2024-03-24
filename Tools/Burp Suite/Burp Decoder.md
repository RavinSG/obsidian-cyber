
## Encoding/Decoding

The Decoder module of Burp Suite gives user data manipulation capabilities. As implied by its name, it not only *decodes data* intercepted during an attack but also provides the function to *encode our own data*, prepping it for transmission to the target. 

Decoder also allows us to *create hashsums* of data, as well as providing a Smart Decode feature, which *attempts to decode provided data recursively* until it is back to being plaintext.

![[Burp Decoder.png]]

Above, we have the interface of the decoder and it lays out a multitude of options.

1) This box serves as the workspace for entering or pasting data that requires encoding or decoding. Consistent with other modules of Burp Suite, data can be moved to this area from different parts of the framework via the Send to Decoder option upon right-clicking.
   
2) At the top of the list on the right, there's an option to treat the input as either text or hexadecimal byte values.
   
3) As we move down the list, dropdown menus are present to encode, decode, or hash the input.
   
4) The Smart Decode feature, located at the end, attempts to auto-decode the input.

### Available Encoding Types

- **Plain**: This refers to the raw text before any transformations are applied.

- **URL**: URL encoding is utilised to ensure the safe transfer of data in the URL of a web request. It involves substituting characters for their ASCII character code in hexadecimal format, preceded by a percentage symbol (`%`). This method is vital for any type of web application testing.

- **HTML**: HTML Entities encoding replaces special characters with an ampersand (&), followed by either a hexadecimal number or a reference to the character being escaped, and ending with a semicolon (;). This method ensures the safe rendering of special characters in HTML and helps prevent attacks such as XSS. The HTML option in Decoder allows any character to be encoded into its HTML escaped format or decode captured HTML entities. 

- **Base64**: Base64, a commonly used encoding method, converts any data into an ASCII-compatible format. 

- **ASCII Hex**: This option transitions data between ASCII and hexadecimal representations. For instance, the word "ASCII" can be converted into the hexadecimal number "4153434949". Each character is converted from its numeric ASCII representation into hexadecimal.

- **Hex, Octal, and Binary**: These encoding methods apply solely to numeric inputs, converting between decimal, hexadecimal, octal (base eight), and binary representations.

- **Gzip**: Gzip compresses data, reducing file and page sizes before browser transmission. Faster load times are highly desirable for developers looking to enhance their SEO score and avoid user inconvenience. 

### Hex Format

While inputting data in ASCII format is beneficial, there are times when byte-by-byte input editing is necessary. This is where "Hex View" proves useful, selectable above the decoding options:

![[Hex Format.png]]

This feature enables us to view and alter our data in hexadecimal byte format, a vital tool when working with binary files or other non-ASCII data.

## Hashing

Decoder allows us to create hashsums for data directly within Burp Suite; it operates similarly to the encoding/decoding options we discussed earlier. Specifically, we click on the Hash dropdown menu and select an algorithm from the list:

![[Available Hashes.png]]

This list is significantly longer than the encoding/decoding algorithms .

A hashing algorithm's output does not yield pure ASCII/Unicode text. Hence, it's customary to convert the algorithm's output into a hexadecimal string; this is the "hash" form you might be familiar with.

![[Hash in ASCII Hex.png]]
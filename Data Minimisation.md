Data minimisation techniques seek to reduce risk by *reducing the amount of sensitive information that we maintain on a regular basis*. The best way to achieve data minimisation is to simply **destroy data when it is no longer necessary** to meet our original business purpose.

If we can't completely remove data from a dataset, we can often transform it into a format where the original sensitive information is de-identified. The **de-identification** process removes the ability to link data back to an individual, reducing its sensitivity.

An *alternative to de-identifying* data is transforming it into a format where the original information can't be retrieved. This is a process called **data obfuscation**, and we have several tools at our disposal to assist with it:

- **Hashing** uses a hash function to transform a value in our dataset to a corresponding hash value. If we apply a strong hash function to a data element, we may replace the value in our file with the hashed value.

- **Tokenisation** replaces sensitive values with a unique identifier using a lookup table. For example, we might replace a widely known value, such as a student ID, with a randomly generated 10-digit number. We'd then maintain a lookup table that allows us to convert those back to student IDs if we need to determine someone's identity. 

- **Masking** partially redacts sensitive information by replacing some or all sensitive fields with blank characters. For example, we might replace all but the last four digits of a credit card number with X's or \*'s to render the card number unreadable.

Although it isn't possible to retrieve the original value directly from the hashed value, there is one major flaw to this approach. If someone has a list of possible values for a field, they can conduct something called a [[Rainbow Table]] attack.
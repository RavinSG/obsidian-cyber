
Sequencer allows us to *evaluate the entropy*, or randomness, of "**tokens**". Tokens are strings used to identify something and should ideally be generated in a cryptographically secure manner. 

These tokens could be session cookies or Cross-Site Request Forgery ([[Cross-Site Request Forgery|CSRF]]) tokens used to protect form submissions. If these tokens aren't generated securely, then, in theory, we could predict upcoming token values. The implications could be substantial, for instance, if the token in question is used for password resets.

![[Burp Sequencer.png]]

There are two main ways to perform token analysis with Sequencer:

- **Live Capture**: This is the more common method and is the default sub-tab for Sequencer. Live capture lets us *pass a request that will generate a token* to Sequencer for analysis. For instance, we might want to pass a POST request to a login endpoint to Sequencer, knowing that the server will respond with a cookie. With the request passed in, we can instruct Sequencer to start a live capture. It will then automatically make the same request thousands of times, storing the generated token samples for analysis. After collecting enough samples, we stop the Sequencer and allow it to analyse the captured tokens.

- **Manual Load**: This allows us to *load a list of pre-generated token samples* directly into Sequencer for analysis. Using Manual Load means we don't need to make thousands of requests to our target, which can be noisy and resource-intensive. However, it does require that we have a large list of pre-generated tokens.

## Live Capture

First, capture a request to `http://10.10.234.252/admin/login/`in the Proxy. Right-click on the request and select Send to Sequencer.

In the "**Token Location Within Response**" section, we can select between Cookie, Form field, and Custom location. Since we're testing the loginToken in this case, select the "Form field" radio button and choose the loginToken from the dropdown menu:

![[Live Capture.png]]

A new window will pop up indicating that a live capture is in progress and displaying the number of tokens captured so far. Wait until a sufficient number of tokens are captured (approximately 10,000 should suffice); the more tokens we have, the more precise our analysis will be.

Once around 10,000 tokens are captured, click on Pause and then select the Analyze now button.
### Entropy Analysis

The generated entropy analysis report is split into four primary sections. The first of these is the Summary of the results. The summary gives us the following:

![[Entropy Report.png]]
![[Entropy Report 2.png]]

- **Overall result**: This gives a broad assessment of the security of the token generation mechanism. In this case, the level of entropy indicates that the tokens are likely securely generated.

- **Effective entropy**: This measures the randomness of the tokens. The effective entropy of 113 bits is relatively high, indicating that the tokens are sufficiently random and, therefore, secure against prediction or brute force attacks.

- **Reliability**: The significance level of 1% implies that there is 99% confidence in the accuracy of the results. This level of confidence is quite high, providing assurance in the accuracy of the effective entropy estimation.

- **Sample**: This provides details about the token samples analysed during the entropy testing process, including the number of tokens and their characteristics.

While the summary report often provides enough information to assess the security of the token generation process, it's important to remember that further investigation may be necessary in some cases. The *character-level and bit-level analysis can provide more detailed insights* into the randomness of the tokens, especially when the summary results raise potential concerns.

While the entropy report can provide a strong indicator of the security of the token generation mechanism, there needs to be more definitive proof. Other factors could also impact the security of the tokens, and the nature of probability and statistics means there's always a degree of uncertainty. 
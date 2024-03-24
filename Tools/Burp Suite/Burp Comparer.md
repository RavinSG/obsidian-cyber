
Comparer, as the name implies, lets us compare two pieces of data, either by ASCII words or by bytes.

![[Burp Comparer.png]]

The interface can be divided into three main sections:

1) On the left, we see the items to be compared. When we load data into Comparer, it appears as rows in these tables. We select two datasets to compare.
   
2) On the upper right, we have options for pasting data from the clipboard (Paste), loading data from a file (Load), removing the current row (Remove), and clearing all datasets (Clear).
   
3) Lastly, on the lower right, we can choose to compare our datasets by either words or bytes. It doesn't matter which of these buttons you select initially because this can be changed later. These are the buttons we click when we're ready to compare the selected data.

Just like most Burp Suite modules, we can also load data into Comparer from other modules by right-clicking and choosing Send to Comparer.

Once we've added at least 2 datasets to compare and press on either Words or Bytes, a pop-up window shows us the comparison:

![[Comparing Two Strings.png]]

This window also has three distinct sections:

1) The compared data occupies most of the window; it can be viewed in either text or hex format. The initial format depends on whether we chose to compare by words or bytes in the previous window, but this can be overridden by using the buttons above the comparison boxes. 
2) The comparison key is at the bottom left, showing which colours represent modified, deleted, and added data between the two datasets.
3) The Sync views checkbox is at the bottom right of the window. When selected, it ensures that both sets of data will sync formats. In other words, if you change one of them into Hex view, the other will adjust to match.

We can use this *alongside the Repeater* to check the difference between the responses we get when we send correct vs incorrect credentials to a login page.


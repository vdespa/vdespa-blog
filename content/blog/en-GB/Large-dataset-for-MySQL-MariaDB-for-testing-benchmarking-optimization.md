# Large or huge datasets for MariaDB / MySQL testing, benchmarking or optimization

Looking for a large MySQL sample database? 

## What size does a huge database have? ##

Actually it's important to define what large actually is. 

MySQL does actually offer for download some [sample databases](http://dev.mysql.com/doc/index-other.html) and while they are actually good for learning and small tests, they are, at least for my concept of large database, unfortunately way to small.

On the other hand, defining what large is, becomes rather a subjective issue. Some would consider that 200 MB is a large database. If you ask me I would consider a large database anything that is larger than 1 GB, other would go for 1 TB.

The annoying thing when looking for such a database or a dataset, whatever your idea of large is, there are quite some datasets out there and I had to try them one by one to get an idea on how large is that most of the downloads offered don't actually state how many records or an approximate size they actually are. If you are looking for a 10GB dataset, you probably wasted lot of time downloading 200 MB sets.

## Top MySQL datasets, by size ##

Last updated: 10.07.2014. If you know a free large dataset, drop me a comment.

Below you will find some datasets which you can easily import in MySQL. Don't get fouled by the size of the archive you are downloading. Some datasets are quite good compressed. Shown is the size of the database in MySQL.

http://dev.mysql.com/doc/index-other.html

Stack Exchange Creative Commons data hosted by the Internet Archive - https://archive.org/details/stackexchange - in MySQL ~50GB.



## Viewing that dataset ##

Now that you have downloaded the "thing", next question is how to import it. To answer that question, it would be great if you could just see a bit of the data to observe if it's formated as a CSV, XML, etc.

Your normal text editor won't help you. It will try to open it all by copying it's contents to the memory. Of course, unless you have huge amounts of RAM. this operation is deemed to fail.

One of the first tools I have tried that actually worked, was Large Text File Viewer (LTFViewr5u), available for free (I think - it does not actually come with a license or something).


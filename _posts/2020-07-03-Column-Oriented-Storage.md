---
layout: post
title:  "Column-Oriented Storage"
date:   2020-07-03 20:22:00+0800
categories: jekyll update
---
What is Column-Oriented Storage? In Contrast to Row-Oriented Storage, each column of data is stored in 
a seperate file, and all the column files store rows in the same order.

# Column Compression

Values in a column often look quite repetitive, so they can be compressed. 

## Bitmap Encoding

we can take a column with n distinct values and turn it into n separate bitmaps: one bitmap for each distinct value, with one bit for each row. The bit is 1 if the row has that value, and 0 if not.

## Run-length encoding

In run-length encoding, a sequence of "0, 4, 12, 2" stands for 0 zeros, 4 ones, 12 zeros, 2 ones.

Using bitmap encoding, bitwise operation can be use to speed up queries.

Column families are row-oriented: within each column family, they store all columns from a row 
together, along with a row key, and they do not use column compression.

# Memory bandwitdth and vectorized processing
Column compression allows more rows from a column to fit in the same amount of L1 cache. Operators, such as bitwise AND and OR, can be designed to operate on such chunks of compressed column data directly. 

# Sort Order in Column Storage

We can choose to impose an order on rows, and use that as an indexing mechanism. A second column can determine the sort order of any rows that have the same value in the first column. Another advantage of sorted 
order is that it can help with compression of columns. 
## Several different sort orders

Different queries benefit from different sort orders, so why not store the same data sorted in several different ways? Data needs to be replicated to multiple machines anyway.

# Writing to Column-Oriented Storage
Write data to an in-memory store first and then merge the in-memory store with the column files  on disk 
and written to new files in bulk.
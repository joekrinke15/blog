---
layout: post
title: Creating a Spotify Database
image: https://developer.spotify.com/assets/branding-guidelines/icon3@2x.png
---


# Post under development!

The first step in getting this data into a database is normalizing it. Normalization makes the database more efficient and flexible by reducing the amount of redundant data. The database is already in first normal form, as there are no columns that contain multiple data elements; the next step is to determine how to get it into third normal form. Third normal form is when every non-prime attribute is non-transitively dependent on every primary key. This sounds a bit jargony, I know, but an example may help clarify things.

A primary key is a value that is unique for each record in the database. In the current scenario, we have a unique track id for every track in our dataset, so we could potentially use this as one of our primary keys. If the database was in third normal form all the other columns in the table would only be dependent on the track id AND would not be transitively dependent on another column. Transitive dependence is when you know what the value of a column will be based on a column that is not the primary key of the table. This database is not currently in third normal form, as the present columns are dependent on non-primary-key values in the database. 



For example, the track album name is transitively dependent on the track album id; knowing the id of the track means you *KNOW* the value of the track album id which, in turn, means you *KNOW* the name of the album. The goal is to have the only column in a table that allows you to *KNOW* another column's value to be the primary key. We need to break the table up into smaller tables to fix this issue. 

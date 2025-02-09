# The Cutting Edge School YT report 

# Unguided project

![Image](https://github.com/user-attachments/assets/7e88f043-9eca-4ee9-8a2f-646c5515414b)
# 

I created an analytics dashboard for Ansh Mehra's youtube channel in PowerBI. Below is a breakdown of how I got the data and cleaned the data 

# How I got the data

Used the Youtbe API to make a request to the channel internally to scrape the publicly avaiable data. To access and use the API steps involved were

1.) Making a developers account on google for developers.

2.) Choose to scrape the data by its individual video ID's but this API follows a structure in which it keeps its video id which is 
channel_id -> playlist_id -> video_id

So even if a video is not necessarly in a specific playlis the API lists all the videos in  a master playlist.

3.) The code to extract the playlist_id

![Image](https://github.com/user-attachments/assets/820a37dd-108f-4879-8b08-b34503176c6c)

4.) The code to extract the video_id

![Image](https://github.com/user-attachments/assets/b47853ce-8179-4380-9a04-de72df041554)

Finally to extract details of the individual video I wrote this code

![Image](https://github.com/user-attachments/assets/e4f5407e-b501-4eed-b4a2-16e29edb6040)

5.) Final data on exporting to excel looked like this

![Image](https://github.com/user-attachments/assets/a77b5aca-8261-4be0-b2d2-92ea675d0eef)


# Cleaning the data

1.) Removing unnecessary columns like caption and video quality since it had the same values for every row i.e. HD 

2.) In the previous image in the tags section you can see that all the tags of a video are in a list like [‘Ansh Mehra’, ‘ui’ ,’ux’, ‘freelance’]

This makes the data harder to analyze hence I loaded the data to power bi and grouped all the tags by the number of times they appeared in the whole dataset and connected that table to the original table.

3.) Changed format of duration column in power bi to make it more readable and added a column classifying the video as short or long form

# Dax formulas used for data preprocessing

I had applied a group by to the tags section hence there were multiple rows for the same video since there were multiple tags to those videos and every row of data can only represent one row of data. Due to this the amount of views, likes and comments were repeated causing a wrong value for the total amount of views/likes/comments.

To tackle this I applied the following formulas

1.)Total Views
    
        =SUMX(SUMMARIZE(
        org,
        org[id],  
        "Distinct Views", MAX(org[View_Count])
    ),
    [Distinct Views])

Here the summarize function creates a new table "Distinct Views" which has column max of view count. Hence now for every id there is only one view count.
After making the new table we can easily use the sumx function.

The same formula was followed for like and comment count

2.) Amount Of Videos
    
    =COUNTX(SUMMARIZE(
        org,org[id],
        "distinct views",MAX(org[ID]))
        ,[distinct views])

Again the same issue I encountered which was that for the same id their were many rows of data but the same amount of views. If I would have applied a simple count then it would amount to a wrong number due to the many duplicates.

Hence I created a temporay table using summarize which has column max of the id thus ignoring duplicates and counting only the distinct ones.

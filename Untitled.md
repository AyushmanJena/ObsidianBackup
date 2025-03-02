# Youtube Comments Analyzer
Ever watched a 1 hour long educational video or tutorial just to learn nothing? Sometimes it's not you, not all youtube videos are perfect. However you can always check the comments under the video to see what others are saying. But what if there is a whole playlist or you are just lazy. 

Youtube Comments Analyzer got you covered. :)

Get comments from a video or each video in a playlist. Then get the comments summarized by Google Gemini with a rating as well. And decide for yourself if the video is even worth watching.

## How to use :
1. Go to "src/main/resources"
2. Create a file named "api-keys.properties"
3. Add the following code to the file : 

`yt.api.key = <yt key>`
`gemini.api.key = <your gemini API key>`

replace the `<yt key>` with your generated google cloud API key 
and `<gemini key>` with your generated API key

Tutorials to generate the keys : 
how to generate google cloud api key : https://www.youtube.com/watch?v=brCkpzAD0gc
how to generate google gemini api key : https://www.youtube.com/watch?v=o8iyrtQyrZM

4. Run the application 
5. go to http://localhost:9090/
6. Select Video/Playlist Paste the videoId/playListId in the text field and click analyze


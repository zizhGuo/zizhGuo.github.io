---
layout: post
title:  "Mashup Web Application consuming SOAP&REST Web Services"
date:   2020-02-16 23:40:12
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---


### 1. Web Application Overall
#### 1.1 The Web Application introduction
This mashup apps is a funny web application using four public web services (3 REST web services + 1 SOAP web service). It has one single buttle on the index page marked as "get me a word and a track". Once the user pushes the buttons, the backend application would first invoke a web service to generate a random English word, which would be used as a search key to invoke another web service in charge of track (music) search. After then, the music search API would return the first searching result containing a track title and an artist name as two strings to invoke the third Lyrics API in order to get its lyrics. Finally, all results including random word, track's title, artist's name, and lyrics would be print to another dynamically generated web page. Once it jumped to the new page, there still has a button to generate a new random English word to trigger the same procedure again. The users will always do it by pressing the button "give me another word and a track" to request a new page containing the new information. Last, the SOAP API will generate a key composed of random English characters and numbers to provide an extra function for users to use.

#### 1.2 The goal of Web Application:
- Designed to minimize the UI complexity for lightweight product experience
- Provided only single button on index page without any input so that to trigger users' repeatable anticipation
- To help learn new English word with a track that could help fortify the situational memorization

#### 1.3 Installation
**Tools:**
- Netbeans IDE 11.2
- Maven 3.1 (plugin version)
- JAVA 1.8
- Glassfish 5.0x (local server)



**Step 1:** Using Netbeans 11.2 import the project.

**Step 2:** Check the "Services" tab on the left-hand side of IDE.

**Step 3:** Make sure it has a "Glashfish" service attached to the project

![glassfish_check](https://i.imgur.com/1SydJLS.png)

**Step 4:** Build the project to make sure dependencies are all installed.
you can always open POM.xml for dependencies check.
**Step 5:** Run the project. It will take a while to launch the local server if it is first launched.
**Step 6:** Wait for the default browser pop out or type in http://localhost:8080/webprojecttest/ in the URL section to visit.


#### 1.3 App Demonstration
![front_page](https://i.imgur.com/0FM5iLp.png)

Image 1: Front Index Page

![response_page_02](https://i.imgur.com/RCwQsqB.png)

Image 2: Response Page

![response_page_04](https://i.imgur.com/VDVJHWK.png)

Image 3: Another Response Page with a longer lyrics

### Readme file for installation


### 2. Web Services

#### 2.1 WS Descriptions
**Apiseeds Music:** (REST)
[**[Link]**](https://apiseeds.com/documentation/music)
    This service provides a collection of random words scraped from websites. It is a RESTful service that allows the user to invoke through URL. The server would return a JSON format object that contains only a random English word. The word would be used for later Music Search were the place to get the title, artist name, and its lyrics.

**Apiseeds Lyrics:** (REST)
[**[Link]**](https://apiseeds.com/documentation/lyrics)  
    Apisseds is an API provider that provides multiple RESTful web services. This music service allows users to search music tracks based on a **search words**. The information returned includes the track's title, artist, album withs its IDs for more professional usages. Therefore, based on **ID** to invoke is not allowed. However, my assignment only used the free service that requests the **title** and **artist** stored in Strings data type by a given search word. These two data type instances will be passed later for the Lyrics request service.
The API key for Apiseeds Lyrics and Apiseeds Music:
```
API key: yVl2CQoJHboJc6QLKym44Tm1fxoDA8UfyARdX9XklkxlwgH0RSS1miDsMGc5CFoe
```
**Random Word:** (REST)
[**[Link]**]( https://random-word-api.herokuapp.com)
    This API is provided by the same provider of Apisseds Music, the database it has allows users to discover millions of lyrics from all artist all around the world. By given the **track title** and **artist name**, the services will return a block of information in JSON format that includes the lyrics of the tracks and language it was composed. The **lyrics** JSON will be parsed to published on the JSP webpages.

**Random Fonts Web Service** (SOAP)
[**[Link]**](http://ws.webxml.com.cn/WebServices/RandomFontsWebService.asmx)
[**[WSDL]**](http://ws.webxml.com.cn/WebServices/RandomFontsWebService.asmx?WSDL)  
    This API provides functions to generate random characters in English and Numbers. It is a soap AIP that has a WSDL file for a quick set up. The return value type is String.




#### 2.2 Certificate Issue might happens:
The Glassfish Server requires all HTTP request remote servers has the certificate to pass the validation based on the records stored in its own trust store, in which the directory is 
```sh
.\GlassFish_Server\glassfish\domains\domain1\config
```
If the there is **runtime error** reported:
```sh
"PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target"
```
This means it has SSL issues with invalid certificates from the servers you requested.
The **solution** is to **cd** to the directory and open cmd console, and type this:
```sh
keytool -importcert -keystore .\cacerts.jks -trustcacerts -alias "keyquery" -file .\keyquery.cer
```
to import the certificate to Glassfish trust list.


#### 2.3 How to add certificate file to Glassfish trust store.

```
keytool -importcert -keystore .\cacerts.jks -trustcacerts -alias "randomword" -file .\randomword.cer

keytool -importcert -keystore .\cacerts.jks -trustcacerts -alias "lyrics" -file .\lyrics.cer
```

### 3 Implementation

#### 3.1 Files
- **com.mycompany.webprojecttest.Model**
    This file overall implements the application's business logic. It contains three instances of the other three API invocations class that implements the program workflow.

    ```java
        private Lyrics lyrics_instance;
        private RandomWord randomWord_instance;
        private Tracks tracks_instance;

        public Model()throws MalformedURLException, IOException{
            // get random word
            this.randomWord_instance = new RandomWord();

            // get tracks info
            tracks_instance = new Tracks(randomWord_instance.getRandomWordString());

            // get lyrics
            this.lyrics_instance = new Lyrics(tracks_instance.getTrackTitle(), tracks_instance.getArtist());            
    }
    ```
    All other methods are available for JSP page to instantiate and get return values to present.
    ```java
        public String getLyrics(){
            if (lyrics_instance.getLyricsString() == "") return "Sorry, this track does not have lyrics";
            return lyrics_instance.getLyricsString();
        }
        public String getRandomWord(){ return randomWord_instance.getRandomWordString();}
        public String getTrackTitle(){ return tracks_instance.getTrackTitle();}
        public String getAritistName(){ return tracks_instance.getArtist();}
        public String getRandomKey(){ return getRandomChar(); }
    
    ```

- **package com.mycompany.webprojecttest.RandomWord**
This Class works for invoking Random Word API through HTTP request to get a random word.
It has two one methods to return the word to **Model** instance.
    ```java {.line-numbers}
        // The URL is for getting dynamic token key from the API.
        final String url_key = "https://random-word-api.herokuapp.com/key?";
        final String url = "https://random-word-api.herokuapp.com/word?key=GNK11SCE&number=1";
        
        private String random_word_string = "";  // random words
        private String apikey = "";
    
    ```
    ```java {.line-numbers}
        public String getAPIkey(){return apikey;} 
    ```
    ```java {.line-numbers}
        public String getRandomWordString(){return random_word_string;}
    ```

- **package com.mycompany.webprojecttest.Tracks**
This class works for getting the random word and invoke Lyrics API through HTTP request to get track information and return the string of Artist and Track Title to Lyrics instance.

- **package com.mycompany.webprojecttest.Lyrics**
This class works for getting the string of Artist and Track Title and invoke Lyrics API through HTTP requests to get lyrics.

#### 3.2 HTTP request
```java
        String str = "";
        URL url2 = new URL(url);
        HttpURLConnection connection =(HttpURLConnection) url2.openConnection();

        // Imiate the browsers by adding the "User-Agent" header
        connection.addRequestProperty("User-Agent", "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:25.0) Gecko/20100101 Firefox/25.0");
        connection.setRequestMethod("GET");
        connection.connect();
        int responseCode = connection.getResponseCode();
        System.out.println(responseCode);
        if (responseCode == HttpURLConnection.HTTP_OK){
            BufferedReader bufferReader = new BufferedReader(new InputStreamReader (connection.getInputStream()));
            StringBuffer stringBuffer = new StringBuffer();
            while ((str = bufferReader.readLine()) != null) {
                stringBuffer.append(str);
                stringBuffer.append("\n");
                System.out.println(stringBuffer.toString());
            }           
            bufferReader.close();
            str = stringBuffer.toString();
```
#### 3.3 JSON parse
```java 
        try{
            JSONParser parser = new JSONParser();
            JSONObject obj = (JSONObject)parser.parse(stringBuffer.toString());
            JSONObject obj2 = (JSONObject)obj.get("result");
            JSONObject obj3 = (JSONObject)obj2.get("track");    
            String msg = obj3.get("text").toString();
            System.out.println("song length : "+ msg.length());
            lyrics_string = msg;
        }
```
---
Zizhun Guo@All rights reserved

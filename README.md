# Botpress basics
## Botpress documentation for customizing the bot.

### 1. In-order to make the bot auto-greet when the user opens the website we must use this html code snippet:

~~~javascript
<script>
      window.botpressWebChat.onEvent(
        function (event) {
          if (event.type === 'LIFECYCLE.LOADED') {
            window.botpressWebChat.sendEvent({ type: 'show' })
          }
        },
        ['LIFECYCLE.LOADED']
      )
</script>

~~~

Paste the above code snippet in your HTML file for the website.

#### NOTE:
Make sure to add a ```trigger``` node at the start of your botpress flowchart.

## The above method is no longer needed. You can simply use the ```trigger``` node at the start of your flowchart to make it work.

### 2. To connect your bot to zapier in-order to send queries use the following code snippet :

~~~javascript
workflow.zapierSuccess = false
const leadData = {name: workflow.name,
                  email: workflow.email,
                  number: workflow.phone,
                  class: workflow.classtype,
                  day: workflow.day,
                  time: workflow.time}
try {
    const response = await axios.post('---', leadData)
    console.log(response.data)
    workflow.zapierSuccess = true
}  catch(error) {
  console.error(error)
}
~~~
##### STEPS:
 1. Paste the above in your execute code block.
 2. Replace the ```---``` with your zapier.com webhook URL.
 3. The variables that you can see inside the leadData, are the ones being sent through the zapier webhook. Change them according to your needs. 
 4. Remember that the variables in botpress are structured as ```workflow.<variable-name>```, change the ```<variable-name>``` accordingly.


### 3. To connect stack-ai to your botpress bot, use the following code snippet :

~~~javascript
const endpoint='---'
const headers = {
    Authorization : 'Bearer ...',
    'Content-type': 'application/json'
}
const data = {
    'in-0': workflow.question,
}
try{
    const response = await axios.post(endpoint, data ,{headers})
    workflow.apiResponse = response.data['out-0']
 } catch (error) {
    throw new Error('stack-error: ${error}')
 }
~~~

##### STEPS:
1. Paste the above in your execute code block.

2. Replace the ```---``` with the actual endpoint link on your stack-ai account.

3. Replace the ```...``` with your unique bearer key in your stack-ai account.

4. In this code snippet, the variable that receives the stack-ai answer is "workflow.apiResponse", change it accordingly.

### 4. To lets users start chatting with the Chatbot without needing to click any buttons or menus.

~~~javascript
<script>
window.botpressWebChat.onEvent(
        function () {
            window.botpressWebChat.sendEvent({ type: "show" });
        },
        ["LIFECYCLE.LOADED"]
    );
</script>    
~~~

Paste the above code into your HTML file for your website.

### 5. Full screen chatbot.

~~~javascript

<script src="https://cdn.botpress.cloud/webchat/v0/inject.js"></script>
<script>
    window.botpressWebChat.init({
        
        "botId": "<your-bot-id>",
        "clientId": "<your-client-id>",
 
        
        "hostUrl": "https://cdn.botpress.cloud/webchat/v0",
        "messagingUrl": "https://messaging.botpress.cloud",
 
        
        "botName": "Test",
 
        
        "containerWidth": "100%25",
        "layoutWidth": "100%25",
 
        
        "hideWidget": true,
        "disableAnimations": true,
    });

    window.botpressWebChat.onEvent(
        function () {
            window.botpressWebChat.sendEvent({ type: "show" });
        },
        ["LIFECYCLE.LOADED"]
    );
</script>
~~~
 The above code can be pasted in your HTML file.

 ##### STEPS:
 1. Once you have pasted the above code in your HTML file, replace the ```<your-bot-id>``` with your bot-ID and the ```<your-client-id>``` with your client-ID. Both of which can be found in your configurable code in your chatbot's dashboard.


 ### 6. Connecting Vectara DB to your chatbot.

 ~~~javascript
 const endpoint = '<URL>';

 const headers = {
    'Content-type': 'application/json',
    'Accept': 'application/json',
    'customer-id': '<CUSTOMER_ID>'
    'x-api-key': '<API_KEY>'
 };

 const payload ={
    'query': [{
        'query': workflow.apiQuery,
        'start': 0,
        'numResults': 5,
        'contextConfig': {
            'charsBefore': 10,
            'charsAfter': 10,
            'sentencesBefore': 2,
            'sentencesAfter': 2,
            'startTag': "<b>",
            'endTag': "</b>"
        },
        'corpusKey': [{
            'customerId': '<CUSTOMER_ID>',
            'corpusId': '<CORPUS_ID>' //knowledge-base
        }],
        'summary': [{
            'summarizePromptName': 'string',
            'responseLang': 'string',
            'maxSummarizedResults': 0
        }],
   }]
 }

 const result = await axios.post(endpoint, payload, {headers});

 const response = result.data.responseSet[0].response;

 response.forEach(info) => {
    const title = info.metadata.find(meta => meta.name === 'title').value;
    const paragraph = info.text;
    formattedParagraph += 'Title: ' + title + '\n' + 'Info: ' + paragraph +'\n';
 };

 worflow.apiResponse = formattedParagraph;
 ~~~
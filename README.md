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

Paste the above code snippet in your html file for the website.

#### NOTE:
Make sure to add a ```trigger``` node at the start of your botpress flowchart.

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
 2. Replace the "---" with your zapier.com webhook URL.

### NOTE:
This is just a sample for leadData. You must change the variables to the names you have created in your workflow.


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

2. Replace the "---" with the actual endpoint link on your stack-ai account.

3. Replace the "..." with your unique bearer key in your stack-ai account.

4. In this code snippet, the variable that receives the stack-ai answer is "workflow.apiResponse", change it accordingly.





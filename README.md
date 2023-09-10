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

### 2. To connect your bot to zapier in-order to send queries use th efollowing code snippet :

~~~javascript
workflow.zapierSuccess = false
const leadData = {name: workflow.name,
                  email: workflow.email,
                  number: workflow.phone,
                  class: workflow.classtype,
                  day: workflow.day,
                  time: workflow.time}
try {
    const response = await axios.post('https://hooks.zapier.com/hooks/catch/16444759/3r2jxau/', leadData)
    console.log(response.data)
    workflow.zapierSuccess = true
}  catch(error) {
  console.error(error)
}
~~~
Paste the above in your execute code block.

### NOTE:
This is just a sample for leadData. You must change the variables to the names you have created in your workflow.







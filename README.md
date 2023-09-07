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






# Botpress basics
## Botpress documentation for customizing the bot.

In-order to make the bot auto-greet when the user opens the website we must use this html code snippet:

'''javascript
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

'''

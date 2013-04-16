SimManager-Touch
================

SimManager port (Extjs to touch 2) to simulate ajax calls
<br>
Extjs version can be found in the Extjs SDK examples>ux>ajax created by [@dongryphon](https://twitter.com/dongryphon)

---

This singleton manages simulated Ajax responses. This allows application logic to be
written unaware that its Ajax calls are being handled by simulations ("simlets"). This
is currently done by hooking **Ext.data.Connection** methods, so all users of that
class (and **Ext.Ajax** since it is a derived class) qualify for simulation.

      Ext.onReady(function () {
          initAjaxSim();

          // normal stuff
      });

      function initAjaxSim () {
          Ext.ux.ajax.SimManager.init({
              delay: 300
          }).register({
              '/app/data/url': {
                  stype: 'json',  // use JsonSimlet (stype is like xtype for components)
                  data: [
                      { foo: 42, bar: 'abc' },
                      ...
                  ]
              }
          });
      }
      
 As many URL's as desired can be registered and associated with a **Ext.ux.ajax.Simlet**. To make
 non-simulated Ajax requests once this singleton is initialized, add a `nosim:true` option
 to the Ajax options:

      Ext.Ajax.request({
          url: 'page.php',
          nosim: true, // ignored by normal Ajax request
          params: {
              id: 1
          },
          success: function(response){
              var text = response.responseText;
              // process server response here
          }
      });

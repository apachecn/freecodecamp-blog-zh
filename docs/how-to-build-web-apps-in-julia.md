# å¦ä½ä½¿ç¨ Genie.jl ð§ââï¸å¨ Julia ä¸­æå»ºæ¨çç¬¬ä¸ä¸ª Web åºç¨ç¨åº

> åæï¼<https://www.freecodecamp.org/news/how-to-build-web-apps-in-julia/>

Julia æ¯ä¸ç§é«çº§çãå¨æçãå¼æºçç¼ç¨è¯­è¨ãå®æ¨å¨å Python ä¸æ ·æäºä½¿ç¨ï¼åæ¶ä¿æ C æ C++ä¸æ ·çæ§è½ã

Julia çè®¸å¤æ©æç¨ä¾æ¯å¨ç§å­¦é¢åï¼å¨è¿äºé¢åä¸­ï¼è¿å»åç°å¨é½éè¦å¤§éçè®¡ç®å¤çãä½æ¯éçè¯­è¨çä¸æ­åå±ï¼è¶æ¥è¶å¤çç¨ä¾è·å¾äºå¨å(æç¤º:web å¼å)ã

å¦æä½ å¯¹ Julia å®å¨éçï¼å¹¶ä¸æ³å¨å¼å§åå»ºä½ çç¬¬ä¸ä¸ª web åºç¨ç¨åºä¹åææ¡ä¸ä¸è¯­æ³ï¼[çç freeCodeCamp ä¸çè¿ç¯æç« ](https://www.freecodecamp.org/news/learn-julia-programming-language/)ã

å®è®²è¿°äºåºç¡ç¥è¯ï¼å¦ä½å®è£ Juliaï¼å®è£åçæ­¥éª¤ï¼ç­ç­ï¼

æ¬æç¨å°éç¹ä»ç»å¨ Julia ä¸­ä»å¤´æå»ºç¬¬ä¸ä¸ª web åºç¨ç¨åºçææå¿è¦æ­¥éª¤ãå æ­¤ï¼è®©æä»¬ä»æ¥ç Genie ç½ç«å¼å§:[https://genieframework.com](https://genieframework.com)ã

## Genie.jl æ¯ä»ä¹ï¼ð§

Genie æ¯ä¸ä¸ªç¨ Julia ç¼åçç°ä»£é«æç web æ¡æ¶ãç¨é¡¹ç®èªå·±çè¯è¯´:

> Genie æ¯ä¸ä¸ªå¨æ  web æ¡æ¶ï¼å®ä¸ºå¼åç°ä»£ web åºç¨ç¨åºæä¾äºä¸ä¸ªç®åä¸é«æçå·¥ä½æµãå®å»ºç«å¨ Julia çä¼å¿ä¹ä¸(é«çº§ãé«æ§è½ãå¨æãJIT ç¼è¯)ï¼ä¸ºé«æç web å¼åæä¾äºä¸°å¯ç API åå¼ºå¤§çå·¥å·éã

Genie ä¸ Django é¡¹ç®éå¸¸ç¸ä¼¼ï¼å ä¸º Genie ä¸ä»ä»æ¯ä¸ä¸ªæ¡æ¶ãç¸åï¼å®æ¯ä¸ä¸ªå®æ´ççæç³»ç»ï¼ææ©å±ä¹ç±»çä¸è¥¿ã

ä½æ¯æä»¬ä¸ºä»ä¹éè¦ç²¾çµå¢ï¼ç®åçç­æ¡æ¯ï¼éç Julia è¶æ¥è¶åæ¬¢è¿ï¼è¶æ¥è¶å¤çå¼åäººåå¸æå¨ä»ä»¬çæ´ä¸ªå æ ä¸­å©ç¨ JuliaãGenie æä¾äºå¨æå¡å¨ç«¯è¿è¡ Julia ä»£ç æ¥é¨ç½²ç½ç«çè½åï¼å æ­¤æ¨å¯ä»¥å°é¨ç½²æºå¨å­¦ä¹ æ¨¡åä½ä¸º Genie åºç¨ç¨åºçä¸é¨åã

å¨æä»¬å¼å§ä½¿ç¨ Genie ä¹åï¼ä½ å¯è½æ³ççä¸ä¸ªå®æ¶é¨ç½²ç Genie åºç¨ç¨åºï¼æåä¸ä¸ä»ä¹æ¯å¯è½ç:[https://pkgs.genieframework.com](https://pkgs.genieframework.com)ã

è¿ä¸ªé¡¹ç®æ¯ä¸ä¸ªç¤¾åºèµæºï¼æ¨å¯ä»¥å¨å¶ä¸­æ¥è¯¢ç¹å®æ¶é´æ®µåç¹å®åçä¸è½½æ¬¡æ°ãé®å¥âgenieâæ¥çæ¯æ¥ä¸è½½æ¬¡æ°ã

æ¨å¯è½ä¹æå´è¶£å­¦ä¹ æ´å¤å³äº Julia ä¸­å¶ä» GUI å web å¼åæ¡æ¶çç¥è¯ãè¦æ´å¹¿æ³å°äºè§£è¿ä¸ªçæç³»ç»ï¼[è¯·æ¥çè¿ç¯æç« ]( https://towardsdatascience.com/6-julia-frameworks-to-create-desktop-guis-and-web-apps-9ae1a941f115)ã

## å¦ä½å®è£ç²¾çµâ¤µï¸

è¦å®è£ Genieï¼æä»¬éè¦åçå°±æ¯æå¼æ±èå¨Â·REPL å¹¶é®å¥`] add Genie`ãè¿ä¼æ»¡è¶³ä½ çä¸åéæ±ãå¦æä¸åæ­£å¸¸ï¼æ¨åºè¯¥è½å¤:

```
julia> using Genie 
```

æ²¡æä»»ä½é®é¢ãç°å¨ï¼æ¨å¯ä»¥å¼å§è¯ç¨ Genie äºã

## å¦ä½å° URL æ å°å° Julia å½æ°ðº

Genie æ¡æ¶çæ ¸å¿é¨åæ¯è·¯ç±å¨çæ¦å¿µãè·¯ç±å¨æ¥åç¨æ·è®¿é®ç¹å® URL çæä½ï¼å¹¶å°å¶ä¸è¢«è°ç¨ç Julia å½æ°ç¸å³èã

è®©æä»¬çä¸ä¸ªç®åçä¾å­ãå¨ REPL ä¸­ï¼é®å¥ä»¥ä¸åå®¹:

```
julia> using Genie, Genie.Router

julia> route("/hello") do
           "Hello freeCodeCamp"
       end
[GET] /hello => #5 | :get_hello
```

å¨è¿ä¸ªä¾å­ä¸­ï¼æä»¬å®ä¹äºâ/HelloâURL æ¥è¿åææ¬âHello freeCodeCampâãæä»¬å¯ä»¥éè¿å¯å¨æå¡å¨æ¥éªè¯è¿ä¸ç¹:

```
julia> up() # start server
â Info: 
â Web Server starting at http://127.0.0.1:8000 
Genie.AppServer.ServersCollection(Task (runnable) @0x000000011c5c5bb0, nothing)
```

ç°å¨æå¡å¨å·²ç»å¯å¨å¹¶è¿è¡ï¼æä»¬å¯ä»¥å¨æµè§å¨ä¸­è®¿é® [`http://127.0.0.1:8000`](http://127.0.0.1:8000) ãæ¨ä¼æ³¨æå°æä»¬å¾å°äºä¸ä¸ª 404 é¡µé¢ï¼è¿æ¯ææä¹ä¸­çï¼å ä¸ºæä»¬å®ä¹çå¯ä¸è·¯ç±æ¯â/helloâãå æ­¤ï¼è®©æä»¬å°å®æ·»å å° URL ä¸­ï¼ççæä»¬ä¼å¾å°ä»ä¹:

![Browser window showing nothing but the text "Hello freeCodeCamp"](img/182c04e04ec7c3a6abbe5510dabcef2e.png)

æä»¬èµ°å§ï¼æä»¬æå»ºä¸ä¸ªå¨åè½ web åºç¨ç¨åºçç¬¬ä¸æ­¥å·²ç»å®æãæä»¬è¿å¯ä»¥éè¿æ£æ¥æ¾ç¤ºä»¥ä¸åå®¹ç REPL æ¥ç¡®è®¤é¡µé¢æ¯å¦æ­£ç¡®å è½½:

```
julia> â Error: GET / 404
â @ Genie.Router ~/.julia/packages/Genie/UxbVJ/src/Router.jl:163
â Error: GET /favicon.ico 404
â @ Genie.Router ~/.julia/packages/Genie/UxbVJ/src/Router.jl:163
[ Info: GET /hello 200
```

æä»¬çå°ç¬¬ä¸æ¬¡å°è¯çç»ææ¯ 404ï¼ç¬¬äºæ¬¡å°è¯æåå°è·å¾äºååº(200 æ¶æ¯è¡¨ç¤ºä¸åæ­£å¸¸)ã

ç°å¨æä»¬æäºä¸ä¸ªåºæ¬çå·¥ä½ç¤ºä¾ï¼è®©æä»¬è¯çå¨æ­¤åºç¡ä¸æ´æ·±å¥å°æå»ºã

ä¸ºæ­¤ï¼æä»¬å°åå»ºä¸ä¸ªæ°æä»¶ãæå°ä½¿ç¨ VS ä»£ç ï¼ä½æ¯æ¬¢è¿ä½ ä½¿ç¨ä»»ä½ä½ è®¤ä¸ºæç¨ç IDEãå¨æä»¬æ¥çä¸ä¸æ®µä»£ç ä¹åï¼æä»¬éè¦ç¡®ä¿éè¿å¨ REPL ä¸­é®å¥`down()`å³é­äºæå¡å¨ã

å¥½äºï¼æ¥ä¸æ¥çä¾å­:

```
using Genie, Genie.Router
using Genie.Renderer, Genie.Renderer.Html, Genie.Renderer.Json

route("/") do
    html("Hey freeCodeCamp")
end

route("/hello.html") do
  html("Hello freeCodeCamp (in html)")
end

route("/hello.json") do
  json("Hi freeCodeCamp (in json)")
end

route("/hello.txt") do
   respond("Hiya freeCodeCamp (in txt format)", :text)
end

# Launch the server on a specific port, 8002
# Run the task asynchronously
up(8002, async = true)
```

è¿ä¸ªä¾å­ä¸­åçäºå¾å¤äºæï¼æä»¥è®©æä»¬æ¥çä¸ä¸åçäºä»ä¹ã

æä»¬ä»è£å¥æä»¬æ³è¦çåå¼å§ãç¶åï¼æä»¬å®ä¹ 4 æ¡ä¸åçè·¯çº¿ãç¬¬ä¸æ¡æ¯ç´¢å¼è·¯çº¿ãæä»¥å½ç¨æ·è®¿é® [`http://127.0.0.1:8002`](http://127.0.0.1:8002) æ¶ï¼ä»ä»¬ä¼çå°âå¿ freeCodeCampâãç´¢å¼åçè·¯çº¿çªåºæ¾ç¤ºæ¯æ¡è·¯çº¿å¯ä»¥ç»åºä¸ä¸ªèªå®ä¹è¾åºãå¨æäºæåµä¸ï¼å®å¯ä»¥æ¯ HTMLï¼å¨å¶ä»æåµä¸ï¼å®å¯ä»¥æ¯ JSON æçº¯ææ¬ã

è¿ä¸ªä¾å­çæåä¸è¡å±ç¤ºäºæå¡å¨å¯å¨ä»£ç ãå¦æ³¨éæè¿°ï¼æä»¬å¯ä»¥è®¾ç½®ç¹å®çç«¯å£å·ï¼å¹¶éæ©æ¯å¦å¸æè·¯ç±å¼æ­¥è¿è¡ãæä»¬ç°å¨å·²ç»æåå°åå»ºäºæä»¬çç¬¬ä¸ä¸ª[ç²¾çµèæ¬](https://genieframework.com/docs/tutorials/Getting-Started.html#developingasimplegeniescript)ï¼

## å¦ä½åå»ºä¸ä¸ªåºæ¬ç Web æå¡ð¸

æ¢ç¶æä»¬å·²ç»ææ¡äºåºç¡ç¥è¯ï¼ç°å¨æä»¬å°å¼å§æå»ºä¸ä¸ªæçç web åºç¨ç¨åºã

å¨æä»¬å¼å§ä¹åï¼æä»¬å°è¿åºç¬¬ä¸æ­¥ï¼åå»ºä¸ä¸ªåºæ¬ç web æå¡ãä¸ºæ­¤ï¼æä»¬å°è¿å¥ REPLï¼å°å½åç®å½åæ¢å°ä¸ä¸ªæäºè®¿é®çç®å½ãå¨æ¬æç¨ä¸­ï¼æå°ä½¿ç¨æçæ¡é¢:

```
shell> cd Desktop
/Users/logankilpatrick/Desktop
```

è¦è¿å¥ä¸é¢æ¾ç¤ºç shell æ¨¡å¼ï¼åªéé®å¥âï¼âè¿å¥ REPLãå¨æçä¾å­ä¸­ï¼ç°å¨æä»¬å·²ç»å°æ´»å¨ç®å½è®¾ç½®ä¸ºæ¡é¢ï¼æä»¬å°ä½¿ç¨æ¹ä¾¿ççæå¨å½æ°æ¥åå»ºæå¡:

```
julia> Genie.newapp_webservice("freeCodeCampApp")

[ Info: Done! New app created at /Users/logankilpatrick/Desktop/freeCodeCampApp
[ Info: Changing active directory to /Users/logankilpatrick/Desktop/freeCodeCampApp
    /var/folders/tc/519vfm453fj_x5bmd8pwx9480000gn/T/jl_bO1R8h/FreeCodeCampApp/Project.toml
[ Info: Project.toml has been generated
[ Info: Installing app dependencies
...
```

`newapp_webservice`æ¯ä¸ä¸ªéå¸¸æç¨çå½æ°ï¼å®èªå¨åå»ºæä»¬ç¬¬ä¸ä¸ª web æå¡æéçææçæ®µãç°å¨æä»¬å·²ç»åå»ºäºä¸ä¸ªé¡¹ç®ï¼æä»¬éè¦å¨ IDE ä¸­æå¼å®(å¨æçä¾å­ä¸­ï¼æ¯ VS ä»£ç )ãå¦ææå¼æ­£ç¡®çæä»¶å¤¹ï¼æ¨åºè¯¥ä¼çå°ä»¥ä¸åå®¹:

![Screen-Shot-2022-01-30-at-7.39.23-PM](img/b20f24890817c0cd0a38be9317ba5880.png)

æå¾å¤æä»¶æ¯èªå¨ä¸ºæä»¬åå»ºçãæä»¬è¦ççä¸»è¦ä¸ä¸ªæ¯`routes.jl`ï¼å®ç¨äºåå»ºè·¯çº¿ï¼å°±åæä»¬å¨ä¸ä¸èæåçé£æ ·ã

æä»¬è°ç¨ççæè¿äºæä»¶å¤¹çå½æ°ä¼èªå¨å¯å¨æå¡å¨ï¼æä»¥è®©æä»¬éè¿è®¿é® [http://127.0.0.1:8000](http://127.0.0.1:8000) æ¥å¿«éæµè§ä¸ä¸ç°æçç»å½é¡µé¢:

![Screen-Shot-2022-01-30-at-7.51.16-PM](img/5fa9139347294e1e6f418031ed051f53.png)

æ­£å¦ä½ å¯è½æ³¨æå°çï¼æçé¡µé¢çèµ·æ¥ä¸ä½ çç¥æä¸åï¼å ä¸ºæè¿å¥å¹¶ç¼è¾äºå¬å±æä»¶å¤¹ä¸­ç`welcome.html`é¡µé¢ã

æ­£å¦æ¨å¨`routes.jl`ä¸­çå°çï¼å½ç¨æ·è®¿é®ä¸» URL `/`æ¶ï¼æä»¬ä¼å°ä»ä»¬è·¯ç±å°æ¬¢è¿é¡µé¢ãæä»¬å¯ä»¥æ·»å é¢å¤çè·¯ç±ï¼å°±åæä»¬å¨ä¸ä¸èæåçé£æ ·ï¼å¹¶æ©å±å®ãæ¬¢è¿ä½ å¨è¿éåä¸æ¥ç©ãæä»¬å·²ç»æä¸ä¸ªç¸å½å¼ºå¤§çç½ç«è®¾ç½®ã

å¦æä½ çä¸ä¸å¶ä»ä¸äºæä»¶å¤¹ï¼æ¯å¦`config/env`ï¼ä½ ä¼çå°å³äºè®¾ç½®ç«¯å£ãä¸»æº URL åå¶ä»ç¸å³åæ°çç»èãåæ ·ï¼ä½ å¯ä»¥éæå¨é£éç©ï¼ä½æä»¬ä¸ä¼å¨æ¬æç¨ä¸­æ·±å¥è¿äºæä»¶çææç»èã

å¨æä»¬è¿å¥ä¸ä¸ä¸ªä¸»é¢ä¹åï¼è®©æä»¬åçå ä¸ªä¸ºæä»¬çåºæ¬ web æå¡çæçæä»¶:

*   å¬å±æä»¶å¤¹åå«ææçåç«¯æä»¶(HTML å CSS)
*   `src`æä»¶å¤¹æ web æå¡çå¥å£ç¹(å¨æçä¾å­ä¸­æ¯`freeCodeCampApp.jl`)
*   bin åå«ä¸äºé¢å¤çä¾èµé¡¹ï¼æä»¬å°åæ¬¡å¿½ç¥
*   Manifest.toml å Project.toml æ¯å³é®ç Julia æä»¶ï¼åè®¸æä»¬ç»´æ¤æä»¬ç Julia ä¾èµå³ç³»ãå½æ¨åå»º web æå¡æ¶ï¼èæ¬èªå¨æ¿æ´»æ¨å½åçé¡¹ç®ç¯å¢(è¿æ¯æä»¬åååå»ºçåºç¨ç¨åº)ãæ¨å¯ä»¥éè¿å¨ REPL ä¸­é®å¥â]âæ¥éªè¯è¿ä¸ç¹ï¼å®å°ä»¥èè²æ¾ç¤ºæ´»å¨ç©ºé´:

![Screen-Shot-2022-01-30-at-7.59.49-PM](img/df482b22e7b57be3281d9dd0243163be.png)

è¿ä»ä»æå³çï¼å¦ææä»¬è¯å¾æ·»å ä¸ä¸ªåï¼å®å°æå®æ·»å å°é¡¹ç®åè¿ä¸ªé¡¹ç®çæ¸åæä»¶ä¸­ï¼èä¸æ¯å¨å±å±äº«çã

## å¦ä½ç¨æ°æ®åºåå»ºä¸ä¸ªå¨åè½ç Web åºç¨ç¨åºð½

æ¢ç¶æä»¬å·²ç»æ¢ç´¢äºåºç¡ç¥è¯ï¼æä»¬å°è¿å¥ä¸ä¸ªå®æ´ç web åºç¨ç¨åºãåæ ·ï¼Genie æä¾äºä¸äºå¾å¥½çå½æ°æ¥å¸®å©æä»¬å¼å§ãå¨åå»ºå®ä¹åï¼æä»¬éè¦å¯¼èªåæ¡é¢:

```
shell> pwd
/Users/logankilpatrick/Desktop/freeCodeCampApp

shell> cd ..
/Users/logankilpatrick/Desktop

shell> 
```

è¯·è®°ä½ï¼æ¨å¯ä»¥é®å¥`;`è¿å¥ shell æ¨¡å¼ï¼å¹¶æ backspace éåº shell æ¨¡å¼ãç°å¨ï¼è®©æä»¬åå»ºåºç¨ç¨åº:

```
julia> Genie.newapp_mvc(Genie.newapp_mvc("freeCodeCampMVC"))
   Resolving package versions...
   ...
```

ç³»ç»å°æç¤ºæ¨éæ©ä¸ä¸ªæ°æ®åºåç«¯ãå¯¹äºæ¬ä¾ï¼æä»¬å°ä½¿ç¨ SQLite:

![Screen-Shot-2022-01-30-at-8.08.31-PM](img/e9b5defdac428fbf790ae75c4b3aaa29.png)

å¦ææ¨æ³ä½¿ç¨ä¸åçæ°æ®åºåç«¯ï¼ä¹å¯ä»¥è¿æ ·åãä½æ¯è¯·æ³¨æï¼æ¨éè¦èªå¨åå»ºæ°æ®åºæä»¶ãGenie åªä¸ºä½ åå»ºä¸ä¸ª SQLite æä»¶ã

æä»¬ç°å¨å·²ç»åå»ºäºä¸ä¸ª MVC åºç¨ç¨åºãä½æ¯ä½ å¯è½ä¼é®èªå·±ï¼ä»ä¹æ¯ MVCï¼

æ¨¡å-è§å¾-æ§å¶å¨èå¼å¨åºç¨ç¨åºå¼åä¸­éå¸¸æ®éãä¸ºäºä¸é·å¥å¶ä¸­ï¼æå°[åä½ æ¨èè¿ç¯æç« ](https://www.freecodecamp.org/news/mvc-architecture-what-is-a-model-view-controller-framework/)ï¼å¨é£éä½ å¯ä»¥è¯»å°ç»èãä»æä»¬ä½ä¸ºå¼åèçè§åº¦æ¥çï¼å½±åä¸å¤§ã

å°±åæä»¬åå»ºä¸ä¸ä¸ªé¡¹ç®æ¶æåçä¸æ ·ï¼æä»¬éè¦å¨ IDE ä¸­åæ¬¡æå¼å®:

![Screen-Shot-2022-02-01-at-6.44.21-AM](img/9219f3441cf4d2e32c09df51551ac6d7.png)

åæ ·ï¼æä»¬å°ä¼çå°å¾å¤åä»¥åä¸æ ·çä¸è¥¿ï¼æ°å¢å ç`app`æä»¶å¤¹å°ä¼åå«å¾å¤å³é®ä»£ç ãéè¿é®å¥ä»¥ä¸å½ä»¤ï¼æä»¬å¯ä»¥çå°æ°é¡¹ç®çæ ·å­:

```
julia> loadapp()

julia> up()
```

ç¶åå¯¼èªä¹: [http://127.0.0.1:8000](http://127.0.0.1:8000) ã

æ¥ä¸æ¥ï¼æä»¬éè¦å°æ°æ®åºè¿æ¥å°æä»¬åå»ºç web åºç¨ç¨åºãä¸ºæ­¤ï¼è½¬è³`db/connection.yml`å¹¶ç¼è¾ä»¥ä¸é¨å:

```
env: ENV["GENIE_ENV"]

dev:
  adapter: SQLite
  database: db/freeCodeCamp_courses.sqlite
```

æ¨å¯ä»¥ææ¶å°å¶ä½å­æ®µçç©ºãç¶åï¼æä»¬éè¦è¿è¡:

```
julia> include(joinpath("config", "initializers", "searchlight.jl"))
```

è¿å°å è½½æ°æ®åºéç½®ãæ¥ä¸æ¥ï¼æä»¬å°ç»§ç»­éç½®æ°æ®åºï¼ä»¥ä¾¿æä»¬å¯ä»¥å°åºç¨ç¨åºä¸­çæ°æ®ä¿å­å°æ°¸ä¹å­å¨ä¸­ã

æä»¬ä»åå»ºä¸ä¸ªæ°èµæºå¼å§è¿ä¸ªè¿ç¨:

```
julia> Genie.newresource("course")
```

ä¸æ¦æä»¬å®ä¹äºèµæºï¼ä¸ä¸æ­¥å°±æ¯å»ç¼è¾æ°æ®åºè¿ç§»è¡¨ï¼å¨æçä¾å­ä¸­ï¼è¿ä¸ªè¡¨å¯ä»¥å¨`db/migrations/2022020115190055_create_table_courses.jl`æ¾å°ã

é»è®¤æåµä¸ï¼è¯¥è¡¨å·²ç»æ ¹æ®æä»¬è¿è¡çæåå ä¸ªå½ä»¤å¡«åäºä¸äºå ä½ç¬¦ææ¬ãå®åºè¯¥æ¯è¿æ ·ç:

![Screen-Shot-2022-02-01-at-7.22.35-AM](img/fde68b6f065952b78a6f9414d0d17222.png)

æä»¬å°ç¼è¾æä»¶ä»¥å¹éæä»¬æ³è¦çç¹å®æ¹æ¡ãè¿å°å®å¨åå³äºåºç¨ç¨åºæ¬èº«ãç±äºææ­£å¨æ¬ç½ç«ä¸å¶ä½è¯¾ç¨ï¼æå°è¾å¥ææè¯¾ç¨çè¯¦ç»ä¿¡æ¯å¦ä¸:

```
module CreateTableCourses

import SearchLight.Migrations: create_table, column, columns, pk, add_index, drop_table, add_indices

function up()
  create_table(:courses) do
    [
      pk()
      column(:title, :string, limit = 200)
      column(:authors, :string, limit = 250)
      column(:year, :integer, limit = 4)
      column(:rating, :string, limit = 10)
      column(:categories, :string, limit = 100)
      column(:description, :string, limit = 1_000)
      column(:cost, :float, limit = 1000)
    ]
  end

  add_index(:courses, :title)
  add_index(:courses, :authors)
  add_index(:courses, :categories)
  add_index(:courses, :description)

end

function down()
  drop_table(:courses)
end

end
```

åæ ·ï¼è¿äºæ¯ä»»æçï¼å¯ä»¥æ¯ä½ æ³è¦çä»»ä½ä¸è¥¿ã

å¼å¾æ³¨æçæ¯ï¼æ·»å ç´¢å¼æ¯å¯éçãä¹æä»¥è¦æ·»å å®ï¼æ¯å ä¸ºå®å¯ä»¥å å¿«æ¥è¯¢éåº¦ï¼ä½æ¯è¿æå¶ä»çæè¡¡ï¼èä¸æ¨å®éä¸ä¸è½å°ææçåé½ä½ä¸ºç´¢å¼æ¥å è½½ãä½ å¯ä»¥å¨è¿éåè¿ééè¯»æ´å¤å³äºè¿äºæè¡¡çåå®¹ã

ç°å¨æä»¬å·²ç»æ´æ°äºæ°æ®åºè¡¨ï¼æä»¬éè¦ä¼ æ­è¿äºæ´æ°ãä¸ºæ­¤ï¼æä»¬å°ä½¿ç¨`SearchLight.jl`ä½ä¸ºæä»¬åºç¨çè¿ç§»ç³»ç»:

```
julia> using SearchLight

julia> SearchLight.Migration.create_migrations_table()
â Info: 2022-02-01 07:37:11 CREATE TABLE `schema_migrations` (
â       `version` varchar(30) NOT NULL DEFAULT '',
â       PRIMARY KEY (`version`)
â     )
[ Info: 2022-02-01 07:37:11 Created table schema_migrations

julia> SearchLight.Migration.status()
[ Info: 2022-02-01 07:37:20 SELECT version FROM schema_migrations ORDER BY version DESC
|   | Module name & status                     |
|   | File name                                |
|---|------------------------------------------|
|   |                 CreateTableCourses: DOWN |
| 1 | 2022020115190055_create_table_courses.jl |

julia> SearchLight.Migration.last_up()
[ Info: 2022-02-01 07:37:29 SELECT version FROM schema_migrations ORDER BY version DESC
[ Info: 2022-02-01 07:37:29 CREATE TABLE courses (id INTEGER PRIMARY KEY , title TEXT  , authors TEXT  , year INTEGER (4) , rating TEXT  , categories TEXT  , description TEXT  , cost FLOAT (1000) )
[ Info: 2022-02-01 07:37:29 CREATE  INDEX courses__idx_title ON courses (title)
[ Info: 2022-02-01 07:37:29 CREATE  INDEX courses__idx_authors ON courses (authors)
[ Info: 2022-02-01 07:37:29 CREATE  INDEX courses__idx_categories ON courses (categories)
[ Info: 2022-02-01 07:37:29 CREATE  INDEX courses__idx_description ON courses (description)
[ Info: 2022-02-01 07:37:29 INSERT INTO schema_migrations VALUES ('2022020115190055')
[ Info: 2022-02-01 07:37:29 Executed migration CreateTableCourses up
```

æä»¬ç°å¨å·²ç»æåå®æäºè¿ç§»ãå¦æè¦å¯¹æ¨¡å¼è¿è¡æ´æ¹ï¼æ¨éè¦éæ°è¿è¡ä¸é¢çå½ä»¤ï¼ä»¥ä½¿è¿äºæ°æ®åºæ´æ¹çæã

è¿ä¸ªè¿ç¨çæåä¸æ­¥æ¯å®ä¹æä»¬çæ¨¡åãè¿å°åè®¸æä»¬ç¨ Julia ä»£ç åå»ºå¯¹è±¡ï¼ç¶åå°å®ä»¬ä¿å­å°æä»¬ååå®ä¹çæ°æ®åºä¸­ãæä»¬éè¦å¯¼èªå°`app/resources/courses/Courses.jl`æç­æè·¯å¾æ¥è¿è¡è¿äºæç»æ´æ°:

```
module Courses

import SearchLight: AbstractModel, DbId
import Base: @kwdef

export Course

@kwdef mutable struct Course <: AbstractModel
  id::DbId = DbId()
  title::String = ""
  authors::String = ""
  year::Int = 0
  rating::String = ""
  categories::String = ""
  description::String = ""
  cost::Float64 = 0.0
end

end
```

åæ ·ï¼è¿åºè¯¥ä¸æ¨ä¹åå®ä¹çåå®¹ç¸åãä¸ºäºç¡®ä¿è¿ä¸ç¹ï¼æä»¬å¯ä»¥:

```
julia> using Courses
[ Info: 2022-02-01 07:43:51 Precompiling Courses [top-level]
```

ç¶åå°è¯éè¿ä»¥ä¸æ¹å¼åå»ºè¯¾ç¨:

```
 julia> c = Course(title = "Web dev with Genie.jl", authors="Logan Kilpatrick")
Course
| KEY                 | VALUE                 |
|---------------------|-----------------------|
| authors::String     | Logan Kilpatrick      |
| categories::String  |                       |
| cost::Float64       | 0.0                   |
| description::String |                       |
| id::DbId            | NULL                  |
| rating::String      |                       |
| title::String       | Web dev with Genie.jl |
| year::Int64         | 0                     |
```

æä»¬å·²ç»æååå»ºäºæä»¬çç¬¬ä¸ä¸ªå¯¹è±¡ï¼ä½æ¯å®ä¸ä¼ç«å³ä¿å­å°æ°æ®åºä¸­ãæä»¬å¯ä»¥éè¿ä»¥ä¸æ¹å¼éªè¯è¿ä¸ç¹:

```
julia> ispersisted(c)
false
```

æä»¥æä»¬éè¦è¿è¡:

```
julia> save(c)
[ Info: 2022-02-01 07:47:04 INSERT  INTO courses ("title", "authors", "year", "rating", "categories", "description", "cost") VALUES ('Web dev with Genie.jl', 'Logan Kilpatrick', 0, '', '', '', 0.0) 
[ Info: 2022-02-01 07:47:04 ; SELECT CASE WHEN last_insert_rowid() = 0 THEN -1 ELSE last_insert_rowid() END AS LAST_INSERT_ID
true 
```

ç°å¨è¯¾ç¨ææäºï¼ä½è¦çæ­£æµè¯è¿ä¸ç¹ï¼æä»¬éè¦ç¨æ·è½å¤åå»ºä¸ä¸ªè¯¾ç¨ãè®©æä»¬åå°`routes.jl`å¹¶å¯ç¨å®:

```
using Genie, Genie.Router, Genie.Renderer.Html, Genie.Requests
using Courses

form = """
<form action="/" method="POST" enctype="multipart/form-data">
  <input type="text" name="name" value="" placeholder="What's the course name?" />
  <input type="text" name="author" value="" placeholder="Who is the course author?" />

  <input type="submit" value="Submit" />
</form>
"""

route("/") do
  html(form)
end

route("/", method = POST) do
  c = Course(title=postpayload(:name, "Placeholder"), authors=postpayload(:author, "Placeholder"))
  save(c)
  "Course titled $(c.title) created successfully!"
end
```

æä»¬ä»å®ä¹ä¸ä¸ªç®åç HTML è¡¨åå¼å§(è¿éæ²¡æä»ä¹æ°çæä»¤äººå´å¥ç)ï¼ç¶åï¼æä»¬è®©é»è®¤ç route `/`åç° HTML è¡¨åãæåï¼æä»¬ä¸º`/` URL åå»ºå¦ä¸ä¸ªè·¯ç±ï¼ä½æ¯ä¸é¨éå¯¹ POST æ¹æ³ãå¨è¿æ¡è·¯çº¿ä¸­ï¼æä»¬éè¿`postpayload`ä»ææè½½è·çè¡¨åä¸­æåæä»¬æ³è¦çä¿¡æ¯ï¼ä»èåå»ºä¸æ¡æ°çè·¯çº¿ã

æ¨å¯ä»¥éè¿å¯¼èªå: [http://127.0.0.1:8000](http://127.0.0.1:8000) æ¥å°è¯

![Screen-Shot-2022-02-01-at-8.11.38-AM](img/6485d3e9f7fb03651d55489ac03573c3.png)

æ¨å¯ä»¥å°è¯è¾å¥ä¸äºè¯¦ç»ä¿¡æ¯ï¼ç¶åææäº¤ãè¦ç¡®ä¿æäº¤ææï¼æ¨å¯ä»¥:

```
julia> all(Course)
[ Info: 2022-02-01 08:10:19 SELECT "courses"."id" AS "courses_id", "courses"."title" AS "courses_title", "courses"."authors" AS "courses_authors", "courses"."year" AS "courses_year", "courses"."rating" AS "courses_rating", "courses"."categories" AS "courses_categories", "courses"."description" AS "courses_description", "courses"."cost" AS "courses_cost" FROM "courses" ORDER BY courses.id ASC
â Warning: 2022-02-01 08:10:19 Unsupported SQLite declared type INTEGER (4), falling back to Int64 type
â @ SQLite ~/.julia/packages/SQLite/aDggE/src/SQLite.jl:416
â Warning: 2022-02-01 08:10:19 Unsupported SQLite declared type FLOAT (1000), falling back to Float64 type
â @ SQLite ~/.julia/packages/SQLite/aDggE/src/SQLite.jl:416
3-element Vector{Course}:
 Course
| KEY                 | VALUE                 |
|---------------------|-----------------------|
| authors::String     | Logan Kilpatrick      |
| categories::String  |                       |
| cost::Float64       | 0.0                   |
| description::String |                       |
| id::DbId            | 1                     |
| rating::String      |                       |
| title::String       | Web dev with Genie.jl |
| year::Int64         | 0                     |

 Course
| KEY                 | VALUE       |
|---------------------|-------------|
| authors::String     | Logan K     |
| categories::String  |             |
| cost::Float64       | 0.0         |
| description::String |             |
| id::DbId            | 2           |
| rating::String      |             |
| title::String       | Test course |
| year::Int64         | 0           |
```

è¿åºè¯¥æ¾ç¤ºæ¡ç®å·²ä¿å­å¨æ°æ®åºä¸­ã

## åæð

åï¼å¤ªå¤äºãæä»¬å¨è¿ä¸ªæç¨ä¸­æ¶åäºå¤§éçåå®¹ã

ä¹å°±æ¯è¯´ï¼å³äº Genie è¿ææ´å¤éè¦å­¦ä¹ ãæå¼ºçå»ºè®®å¨è¿éæ¥çç[ææ¡£ï¼å¶ä¸­ææ´å¤å³äº REST APIãè®¤è¯ç­ä¸»é¢çæç¨ã](https://genieframework.com/docs/tutorials/Overview.html)

## è·å¾å³äº Genie.jl çå¸®å©ð¨

å¦æä½ å¨æ¬æç¨æä½¿ç¨ Genie æ¶éå°é®é¢ï¼è¯·ç¨`genie.jl`å`julia`æ ç­¾æå¨ [Julia Discourse](https://discourse.julialang.org) ä¸æåºé®é¢ãä¹åï¼ä½ å¯ä»¥æé®é¢çé¾æ¥åå°æ¨ç¹ä¸ï¼æä¼å°½åå¸®å©ä½ :ãhttps://twitter.com/OfficialLoganKã
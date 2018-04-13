# JavaScript

c'est un langage de script interprété par le navigateur (ou le serveur pour node.js)
ECMAScript 6 (7 en préparation)
il est asynchrone, orienté objet

Les script sont placé ou chargé en bas du fichier HTML avant la balise </html>


	<script type="text/javascript" src="monscript.js"></script>

	<script type="text/javascript">
	  // Mon code Javascript
	  ...
	</script>


## let vs var :
let: local, dans la fonction, ou le bloc

var: global

toujours utiliser let


# DOM
Pour **D**ocument **O**bject **M**odel

C'est un "arbre" représentant l'arboressance d'un document **xml**, comme le **html**

## a quoi ça sert ?
Le DOM peut être modifié par javascript

## Avant ...
	document.getElementById(id)
	document.getElementsByTagName(name)
	document.getElementsByClassName(name)

## Maintenant:

	document.querySelector("div#coucou")
recupère la **div** d'**id** coucou

	document.querySelectorAll("div")
recupère toute les **div**

	document.querySelectorAll("div")[0]
recupère le premiere élément **div**

	document.querySelectorAll("div>span")
recupère le **span** directement dans une **div**

	document.querySelector("nav>button").onclick = menu;
ajoute la fonction menu au click

# jQuery
C'est une **bibliothèque javascript**, qui permet de simplifier le **DOM**  
on l'appel parr la commande **$** ou **jQuery**

	jQuery()
	jQuery("li").addClass("starred");

## jQery vs JavaScript

	document.querySelectorAll("a").foreach( function(){a.onmouseover=function()} );
	jQuery("a").on("mouseover", function(){} );


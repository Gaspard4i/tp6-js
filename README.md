<img src="images/readme/header.jpg">

## Objectifs <!-- omit in toc -->
- Refaire un tour d'horizon de ce qui a été vu lors des TPs précédents
- Préparer le CTP de JS 😱

## Sommaire <!-- omit in toc -->
- [A. Préparatifs](#a-préparatifs)
- [B. Liste de Pokémons](#b-liste-de-pokémons)
- [C. Filtrage de la liste](#c-filtrage-de-la-liste)
- [D. Détail d'un Pokémon](#d-détail-dun-pokémon)
- [E. Carousel](#e-carousel)


**Comme le but de ce TP est de vous entrainer à réaliser seul une SPA, vous allez être beaucoup moins dirigés que lors des précédents TPs.**

N'hésitez pas à vous référer aux pdf des différents cours sur moodle, à revoir votre code des précédents TPs, ou à demander à votre encadrant.e de TP !


## A. Préparatifs
1. **Faites un fork de ce TP sur https://gitlab.univ-lille.fr/js/tp6/-/forks/new**

	- Pour le `namespace` choisissez de placer le fork dans votre profil utilisateur.\
	- Pour `Visibility Level` sélectionnez le **mode "private"**\
	- ajoutez en **"reporter"** votre encadrant.e de TP (`@patricia.everaere-caillier`, `@catherine.verbrugge` ou `@thomas.fritsch`)

2. **Clonez votre fork et ouvrez le dans VSCode :**
	```bash
	mkdir ~/tps-js
	git clone https://gitlab.univ-lille.fr/<votre-username>/tp6.git ~/tps-js/tp6
	code ~/tps-js/tp6
	```

	Vous constaterez que **la vie est belle** : la configuration de babel, webpack et du debug dans vscode sont déjà faites ! Vous avez aussi un fichier `index.html` et des css fournies, vous allez pouvoir vous concentrer sur le JS !

3. **Lancez votre serveur de développement `webpack-dev-server` dans un terminal intégré de VSCode** (<kbd>CTRL/Cmd</kbd>+<kbd>J</kbd>) :
	```bash
	npm i
	npm start
	```

	> 📖 **Rappel de cours :**
	>
	> _Pour rappel `npm i` c'est un "raccourci" pour la commande `npm install` : cette commande permet de télécharger, dans un dossier `/node_modules` à la racine du projet, TOUS les paquets dont on a besoin (les "dépendances" du projet). Pour savoir quels sont les paquets à télécharger, npm va lire le contenu du fichier `package.json` (plus d'explications dans le [TP2 - A.4. Le fichier `package.json`](https://gitlab.univ-lille.fr/js/tp2/-/blob/main/A-preparatifs.md?ref_type=heads#a4-le-fichier-packagejson) )_
	>
	> _Si vous regardez le contenu du dossier `/node_modules`, vous y trouverez des dossiers pour `@babel`, `prettier`, `webpack` etc._
	>
	> _D'ailleurs en parlant de webpack, on a dit que `npm start` lançait `webpack-dev-server` mais vous vous souvenez de ce que c'est ? Non ? Alors allez voir un peu dans le TP3, ici : https://gitlab.univ-lille.fr/js/tp3/-/blob/main/C-modules.md#c6-webpack--live-reload_

4. **Lancez votre site en mode "debug dans vscode"** : tapez <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> puis sélectionnez `"Debug: Select and start debugging"` ou appuyez simplement sur la touche <kbd>F5</kbd> (_<kbd>F5</kbd> lance en principe le dernier navigateur que vous aviez lancé dans les précédents TP. Vérifiez dans le panneau "Run & Debug" avec <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>d</kbd> quel est le navigateur dans la liste déroulante tout en haut_).

5. **Vérifiez que le rendu dans le navigateur est bien le suivant**, et si oui, vous allez pouvoir passer à la suite. \
	En cas de problème, harcelez votre encadrant.e de TP (_il ne faut pas perdre de temps sur cette étape_) !

	<img src="images/readme/screen-00.png" />

## B. Liste de Pokémons

Dans ce TP vous allez :
- récupérer une liste de pokemons (_en AJAX_) qu'on affichera à gauche de l'écran
- au clic sur un élément de la liste, vous allez lancer un 2e appel AJAX pour récupérer le détail du pokémon cliqué et l'afficher dans la partie de droite

Commençons par nous intéresser à la liste :
1. Tout d'abord ajoutez le fichier `build/main.bundle.js` dans la page `index.html` (_actuellement il n'est pas chargé mais pour rappel vous avez fait ça au [TP1 dans la partie B.2. Inclure le JS dans la page](https://gitlab.univ-lille.fr/js/tp1/-/blob/main/B-integration.md?ref_type=heads#b2-inclure-le-js-dans-la-page) !!!_)

	> <details><summary>📖 <em>C'est quoi déjà ce fichier <code>main.bundle.js</code> ?</em></summary>
	>
	> _Il s'agit du bundle de votre application. C'est quoi un bundle ? Qui le génère ? Rendez-vous dans le [TP3 partie C.4. Webpack : Utiliser un bundler](https://gitlab.univ-lille.fr/js/tp3/-/blob/main/C-modules.md#c4-webpack--utiliser-un-bundler)._
	> </details>

2. En JS, masquez :
	- le formulaire de filtre,
	- la barre de progression de droite
	- la "card" de détail à droite

	Vous devez en principe aboutir à cet affichage :

	<img src="images/readme/init.png"/>

3. Déclenchez au chargement de votre page, un appel AJAX vers l'api https://pokeapi.co/api/v2/pokemon?limit=-1

	> ℹ️ _**La documentation** de cette API se trouve ici : https://pokeapi.co/docs/v2 (cliquez sur `Pokémon` dans le menu de gauche puis sur `Pokemon` - sans accent cette fois)_

	> ℹ️ _Vous remarquerez qu'on passe dans l'URL de l'API **un paramètre `?limit=-1`**. Il permet de récupérer la liste complète de tous les pokemons de la base._
	>
	> _C'est un peu "bourrin" mais c'est ce qui nous permettra plus tard de filtrer les résultats. L'idéal aurait été d'avoir un endoint qui permette de faire la recherche via l'API (comme dans l'API rawg.io) mais ça n'est [pas prévu](https://github.com/PokeAPI/pokeapi/issues/660) dans la version REST de pokeapi._

4. A la fin de l'appel AJAX, masquez la progress bar, ré-affichez le formulaire de filtre, et injectez les résultats retournés par l'API dans la div de classe `results`. Pour chaque pokémon, affichez le code HTML suivant :
	```html
	<a href="https://pokeapi.co/api/v2/pokemon/1/" class="list-group-item list-group-item-action">
		bulbasaur
	</a>
	```
	En remplaçant bien sûr le nom et l'URL du Pokémon !

	<img src="images/readme/list.png">
5. Vous constaterez qu'afficher 1279 résultats n'est pas très... pratique pour la personne qui visite votre app. **N'affichez donc que les 20 premiers résultats !**

## C. Filtrage de la liste

Maintenant que vous avez récupéré les pokémons de la bdd, faites en sorte que lorsque l'on tape quelque chose dans le champ de recherche ([événement `'input'` _(mdn)_](https://developer.mozilla.org/fr/docs/Web/API/HTMLElement/input_event)), on affiche en dessous les 20 premiers pokémons dont le nom contient la chaîne recherchée (_s'il y en a moins de 20 qui correspondent, on les affiche tous_) :

<img src="images/readme/filtre.png" />

## D. Détail d'un Pokémon

Faites en sorte maintenant que quand on clique sur un pokémon de la liste, le détail s'affiche ! Pour cela :

1. Ajoutez la classe "active" sur le lien qui a été cliqué (_et enlevez-la du précédent lien actif_)

	<img src="images/readme/liste-active.png" />

2. Affichez la progress bar du détail :

	<img src="images/readme/detail-loader.png" />

3. Déclenchez un appel AJAX vers l'URL du pokemon cliqué (_fournie dans les résultat du premier appel AJAX_). Par exemple, si l'on clique sur `bulbasaur`, l'appel AJAX qu'on fera sera vers https://pokeapi.co/api/v2/pokemon/1/ .

	Une fois le résultat obtenu, masquez la progress bar, affichez le détail et injectez :
	- dans `<div class="carousel-inner"></div>`, une balise :
		```html
		<div class="carousel-item active">
			<img src="..." class="d-block w-100" alt="">
		</div>
		```
		où le src correspond à une des images du champ `sprites` retourné par l'API.
	- dans `<h2 class="card-title"></h2>`, le nom du pokemon.
	- dans `<h6 class="card-subtitle mb-2 text-muted"></h6>`, ses `types` séparés par `' / '`
	- dans `<p class="badgesContainer card-text"></p>`, ses `abilities`, avec pour chaque ability le code suivant :
		```html
		<span class="badge text-bg-secondary">....</span>
		```
	- dans les deux `<li>`, la taille et le poids du pokémon :
		```html
		<ul class="list-group list-group-flush">
			<li class="list-group-item">taille : XX</li>
			<li class="list-group-item">poids : YY</li>
		</ul>
		```

Si tout est OK, le rendu doit être :

<img src="images/readme/detail-complet.png" />

## E. Carousel

**Pour terminer, vous remarquerez que l'image de la carte de détail dispose de flèches vers la gauche et la droite. Sur la capture d'écran précédente, vous voyez aussi que j'ai une sorte de "pagination" en bas de l'image.**

Si vous inspectez aussi ce que vous retourne l'API de détail, vous remarquerez que dans le champ `sprites` on a aussi beaucoup d'images différentes. On va donc s'en servir pour afficher un diaporama.

**Codez donc maintenant une classe `Carousel` :**
- dans le constructeur passez-lui l'élément `<div class="carousel card-img-top slide bg-light">`
- ajoutez une méthode `setImages(images)` qui va :
	- ajouter autant de balises identiques à celle qu'on a déjà dans le `carousel-inner` (avec la classe `active` uniquement sur la première image)
	- ajouter dans la balise `<div class="carousel-indicators"></div>` des liens de pagination (1 par image) :
		```html
		<button type="button" data-bs-target="#" data-bs-slide-to="X"></button>
		```
		où `X` est l'index du bouton dans la liste.
- faites ensuite en sorte que les boutons de pagination permettent de :
	+ changer l'image ayant la classe `active`
	+ ajouter la classe `active` sur le lien de pagination courant
- enfin faites fonctionner les boutons `carousel-control-prev` et `carousel-control-next`

<img src="images/readme/carousel.png" />
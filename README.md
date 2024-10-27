# back

- product-controller.ts
  - Controller "prod" => porte à confusion versus environnement de production.
  - ProductService instancié à la main => utiliser le système de DI de Nest.
  - @Post() => path du endpoint pas clair ?
  
- ProductRepository.ts
  - Nommage du fichier pas cohérent avec le controlleur ?
  - Les données peuvent être récupérées en une seule requête (problème du N+1) ?
  - Je ne connais pas Nest mais j'imagine qu'il y a moyen d'éviter de passer PrismaClient en paramètre ?

- calculate-price.ts
  - le code est éclaté, s'il fonctionne quand même, ajouter des TU et repartir de 0 ?
  - Peut-être qqch dans ce style (j'ai pas vérifié si ça marche, il faut des TU)  :
  ```ts
	export class CalculatePriceProduct {

	  public calculatePriceFromProduct(product: Product): number {
		return product.items.reduce((acc, current) => acc + this.getProductPrice(current), 0);
	  }

	  private getProductPrice(product: ProductItem) {
		if (product.quantiy === 0) {
		  return 0;
		}
		return product.materials.reduce((acc, current) => {
		  let materialPrice = 0;
		  if (current.dimension > 0 && current.weight > 0) {
			materialPrice = (current.cost * current.interest)
		  }
		  return acc + materialPrice;
		}, 0)
	  }
	}
  ```
  - On peut aussi mettre ce bout de code ailleurs, dans Product.ts par exemple en mode BDD ?

# front

- product.component.ts
  - Les 2 variables de classes sont typées en any => eslint.
  - Le redirect devrait plutôt être fait un niveau des guards du router.
  - On souscrit au changement d'id dans l'url, mais il ne se passe rien à part la variable ID qui est mise à jour. 
    - Dans le callback du subscribe, il faut aussi récupérer le nouveau produit.
	- Ou alors essayer d'utiliser le hook ngOnChange.
	

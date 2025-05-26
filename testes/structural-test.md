**26-05-2025**

# Teste cobertura por linha

Jacoco é uma ferramenta de cobertura de código para Java que analisa quais linhas de código foram executadas durante os testes. Ele gera relatórios detalhados que ajudam a identificar áreas do código que precisam de mais testes.


# teste Por branch

Garantir que todas as tomadas de decisão sejam testadas, try e cach não entram. não validar as condições testa somente o bloco.

focar pricipalmente para os fluxos

``` java

public boolean completa(){
	if(this.valor > 0 && this.valor < 100){
		return true; //deve haver um teste para esssa branch
	} else if(this.valor == 0){
		return false; //deve haver um teste para essa branch
	} else {
		// deve haver um teste para essa branch
		throw new IllegalArgumentException("Valor inválido");
	}
}
```


# Teste por condicional

tomar cuidado quando se testa somente condicionl pode ser que você não consigar testar todas as branches

``` java

public boolean completa(){
	if(this.valor > 0 // deve testar a condição
		 && this.valor < 100 // deve testar a condição
		 ){
		return true; //deve haver um teste para esssa branch
	} else if(this.valor == 0){
		return false; //deve haver um teste para essa branch
	} else {
		// deve haver um teste para essa branch
		throw new IllegalArgumentException("Valor inválido");
	}
}
```

# Cobertura por condicional e por branch

A cobertura por condicional e por branch é uma técnica de teste que garante que todas as condições lógicas e os caminhos de execução do código sejam testados. Isso é importante para identificar falhas e garantir que o código funcione corretamente em diferentes cenários.



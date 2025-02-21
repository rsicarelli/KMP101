## Explorando a Modularização no KMP

No último artigo, entramos em detalhes e aprendemos sobre as peculiaridades do código exportado nos headers do Objective-C, assim como as boas práticas no que exportar.

Nesse artigo, vamos entender melhor sobre o comportamento da modularização em projetos KMP, e como isso pode ser feito de forma eficiente e organizada.

## O que é modularização?

Não irei me alongar muito nesse tópico, pois já abordamos esse assunto no [Android Plataforma - Parte 1: Modularização](https://dev.to/rsicarelli/android-plataforma-parte-1-modularizacao-2016). Se não tem certeza do que é modularização, recomendo uma pausa para a leitura do artigo.

Em resumo, modularização é a prática de dividir um projeto em módulos menores e independentes, que podem ser desenvolvidos, testados e mantidos separadamente.

Essa prática é crucial para escalar projetos KMP, já que a modularização impacta diretamente na autonomia e independência dos times internos, evitando que um time dependa do outro para realizar suas tarefas.

## Modularização no KMP

No KMP, a modularização é feita através de módulos compartilhados, que são responsáveis por compartilhar código entre as plataformas.

Vamos elaborar uma estrutura de módulos que respeite a separação de responsabilidades e possibilite a reutilização de código de forma eficiente entre módulos. Nosso contexto aqui é pensando em uma aplicação que irá escalar, no sentido de mais features e mais plataformas:

<img src="https://github.com/rsicarelli/KMP-101/blob/main/posts/assets/kmp-modularization-pt1.png?raw=true" />

Essa estrutura segue algumas ideias do Domain Driven Design (DDD), onde cada módulo representa um domínio independente e isolado da aplicação. Não irei entrar em muitos detalhes sobre o DDD, mas recomendo a leitura do livro [Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.amazon.com.br/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1?dib=eyJ2IjoiMSJ9.Lo7-Md3VvIV38Rzn-ytmnX1FyJz_hHxG_c3ocyge7LEEkMf9J0QQUC_vNRqM-bly1FEW6JDWiQjxRiR4Ip4uOSi5BDadwwQLRq-qGmgXmoG36NnUp66mVBVEOL-xFpHChmTWdyWDB5EZGboxu2dOIVTrzRS54KI4S6rDRsLLLoSAkU9bCl81j0cePEicQvqB.QPWgwg7lUfTottKjOov5grb2CciIICVV12MWxs8bueA&dib_tag=se&keywords=Domain-Driven-Design-Tackling-Complexity-Software&qid=1739362218&sr=8-1&ufe=app_do%3Aamzn1.fos.4bddec23-2dcf-4403-8597-e1a02442043d) para entender melhor sobre o assunto.

Com essa estrutura, conseguimos:
- Escalar de forma eficiente sem duplicação de código. Ao criar uma nova feature, basta criar um novo módulo e adicionar as dependências necessárias.
- Ter uma granulalidade no que será exportado para as outras plataformas, especialmente para o XCFramework.
- Ter a independencia de domínio para times específicos, evitando conflitos de código e responsabilidades. Por exemplo, times podem criar um `CODEOWNER` para um módulo específico, e serem responsáveis por manter e evoluir esse módulo.

## Pavimentando flexibilidade de UI
Uma dos super poderes do KMP é compartilhar muito, ou pouco código. Essa habilidade implica que podemos  escolher qual UI iremos utilizar em cada plataforma. Dependendo da sua estratégia de construção de UI, você irá precisar de uma abordagem específica de módulos para criar essa flexibilidade.

Para isso, eu gosto de separar cada tela em um "frontend" e "backend". Seguindo o padrão de arquitetura MVVM, o "frontend" seria a nossa UI (Compose, SwiftUI) e o "backend" seria a nossa lógica de negócio (ViewModel/UiModel + Domain + Data). Ou seja, partes da camada de apresentação pode ser compartilhada, mas damos a liberdade para cada plataforma de escolher a sua UI.

Com isso em mente, uma abordagem é a seguinte separação de módulos:



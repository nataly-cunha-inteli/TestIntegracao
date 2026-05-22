# Testes com .NET — DevOps

## Discente: Nataly Cunha | G06

Esta atividade funciona como uma prática guiada de testes unitários, de integração e de esteira, conectando os exemplos do repositório a um aprendizado aplicado de testes para o presente módulo de esteira CI/CD automatizada.

## Evidências de processo

Os testes foram trabalhados em forks criados a partir dos repositórios originais. Primeiramente, vale ressaltar que, no início da execução dos três projetos, houve erros de desatualização da versão do dotnet, sendo necessário trocar, nos arquivos de `csproj`, as menções de versão 5.0 para 9.0.

Abaixo seguem prints do tipo de erro ocorrido e commits de ajuste.

<img width="1035" height="621" alt="image" src="https://github.com/user-attachments/assets/2ec444dc-3823-410e-b103-27c1ae050873" />

<img width="374" height="119" alt="image" src="https://github.com/user-attachments/assets/16675c00-633e-4e1c-bc4e-332970f75840" />

## Teste 1 - Unitário

Este repositório contém uma função simples que converte Fahrenheit para Celsius e testes automáticos que verificam se, de forma individual, a função de conversão está correta; os testes são rápidos de rodar e servem para garantir, de forma simples, que mudanças futuras não quebrem o comportamento esperado, portanto, trata-se de um teste unitário (Pittet, c2026).

https://github.com/nataly-cunha-inteli/DotNet5-xUnit

<img width="830" height="473" alt="image" src="https://github.com/user-attachments/assets/12ef3b19-503a-421c-95ac-bb056fb547e3" />

Este repositório contém dois projetos:

- [Temperatura/Temperatura.csproj](Temperatura/Temperatura.csproj): biblioteca com a regra de negócio.
- [Temperatura.Testes/Temperatura.Testes.csproj](Temperatura.Testes/Temperatura.Testes.csproj): projeto de testes unitários.

A regra principal converte Fahrenheit para Celsius com arredondamento em 2 casas decimais:

- [Temperatura/ConversorTemperatura.cs](Temperatura/ConversorTemperatura.cs)

Os testes validam vários cenários de entrada/saída usando [Theory] e [InlineData]:

- [Temperatura.Testes/TestesConversorTemperatura.cs](Temperatura.Testes/TestesConversorTemperatura.cs)

### Como os testes funcionam

1. O método FahrenheitParaCelsius recebe um valor em Fahrenheit.
2. O cálculo é feito com a fórmula:

   C = (F - 32) / 1.8

3. O resultado é arredondado para 2 casas decimais.
4. O xUnit executa o método de teste uma vez para cada [InlineData].
5. Cada execução compara valor esperado e valor calculado com `Assert.Equal`.

Por fim, como nos testes foi feita a conferência de eficácia dessa função específica com diferentes valores, trata-se de um teste unitário. Da mesma maneira, diferentes testes poderiam cobrir a saída de dados de mais funções distintas.

## Teste 2 - Unitários com Moq

https://github.com/nataly-cunha-inteli/DotNet5-Moq-xUnit-FluentAssertions

<img width="935" height="367" alt="image" src="https://github.com/user-attachments/assets/eae5e221-84fe-4e11-a3f5-a0abfe0a806b" />

Este repositório demonstra como testar uma regra de negócio em .NET usando:

- xUnit para os testes automatizados
- Moq para simular a dependência externa
- FluentAssertions para deixar as asserções mais legíveis

O foco do exemplo é validar a classe `AnaliseCredito`, que decide o status de uma consulta de crédito a partir do retorno de um serviço externo.

A aplicação está dividida em dois projetos:

- `ConsultaCredito`: contém a lógica de negócio e os tipos usados pela regra.
- `ConsultaCredito.Testes`: contém os testes unitários da lógica de negócio.

O teste não acessa nenhum serviço real. Em vez disso, ele usa um mock de `IServicoConsultaCredito` para controlar os cenários e verificar se `AnaliseCredito` retorna o status correto.

Comportamento esperado:

- Se o serviço retornar `null`, o status é `ParametroEnvioInvalido`.
- Se o serviço retornar uma lista vazia, o status é `SemPendencias`.
- Se o serviço retornar uma lista com itens, o status é `Inadimplente`.
- Se ocorrer qualquer exceção, o status é `ErroComunicacao`.

### Detalhes dos arquivos

#### [ConsultaCredito/IServicoConsultaCredito.cs](ConsultaCredito/IServicoConsultaCredito.cs)
Interface que representa a dependência externa usada pela regra de negócio. Ela define o método `ConsultarPendenciasPorCPF(string cpf)`.

#### [ConsultaCredito/OutrasEstruturas.cs](ConsultaCredito/OutrasEstruturas.cs)
Contém os tipos de apoio do domínio:

- `StatusConsultaCredito`: enum com os possíveis retornos da análise.
- `Pendencia`: classe com os dados de uma pendência financeira.

#### [ConsultaCredito.Testes/TestesAnaliseCredito.cs](ConsultaCredito.Testes/TestesAnaliseCredito.cs)
Arquivo com a classe de testes `TestesAnaliseCredito`.

Ele cria um `Mock<IServicoConsultaCredito>` e configura quatro cenários:

- CPF inválido: o mock retorna `null`.
- Erro de comunicação: o mock dispara exceção.
- CPF sem pendências: o mock retorna uma lista vazia.
- CPF inadimplente: o mock retorna uma lista com uma pendência.

Cada teste chama `AnaliseCredito.ConsultarSituacaoCPF` e verifica se o status retornado é o esperado.

### Como os testes funcionam

O objetivo dos testes é validar a regra de negócio sem depender de uma integração real.

O fluxo é este:

1. O teste cria um mock de `IServicoConsultaCredito`.
2. O mock é configurado com resultados diferentes para CPFs específicos.
3. O teste instancia `AnaliseCredito` passando o mock no construtor.
4. O método `ConsultarSituacaoCPF` é executado.
5. O resultado é comparado com o status esperado usando FluentAssertions.

Isso garante isolamento, repetibilidade e rapidez na execução.

## Teste 3 - API

https://github.com/nataly-cunha-inteli/ASPNETCore5-REST_API-xUnit-SpecFlow-Swagger-Docker_JurosCompostos

<img width="963" height="617" alt="image" src="https://github.com/user-attachments/assets/dfeb665e-fb44-4537-a1c9-5db1917fd211" />

Este repositório contém uma API ASP.NET Core para cálculo de juros compostos, com testes automatizados em xUnit e SpecFlow.

## O que os testes cobrem

O projeto de especificações em [APIFinancas.Especificacoes](APIFinancas.Especificacoes) valida o comportamento do cálculo de juros compostos com 7 cenários de entrada diferentes.

Os cenários conferem:

- valor do empréstimo
- número de meses
- taxa de juros mensal
- valor final esperado ao término do período

Os testes garantem que a função central de cálculo produza o mesmo valor esperado pelos cenários de negócio, incluindo a formatação de valores monetários com 2 casas decimais.

### Como os testes funcionam

1. O projeto de especificações executa cenários com entradas diferentes.
2. Cada cenário informa o valor do empréstimo, o número de meses e a taxa de juros mensal.
3. A aplicação calcula o valor final de juros compostos.
4. O resultado é comparado com o valor esperado do cenário.
5. Os testes validam a formatação monetária com 2 casas decimais.

Isso garante que a API entregue um comportamento consistente para as regras de negócio previstas.


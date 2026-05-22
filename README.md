https://github.com/nataly-cunha-inteli/DotNet5-xUnit

<img width="1035" height="621" alt="image" src="https://github.com/user-attachments/assets/2ec444dc-3823-410e-b103-27c1ae050873" />

<img width="374" height="119" alt="image" src="https://github.com/user-attachments/assets/16675c00-633e-4e1c-bc4e-332970f75840" />

<img width="830" height="473" alt="image" src="https://github.com/user-attachments/assets/12ef3b19-503a-421c-95ac-bb056fb547e3" />


# DotNet5-xUnit

Projeto de exemplo com conversao de temperatura em C# e testes unitarios com xUnit.

## Visao Geral

Este repositorio contem dois projetos:

- [Temperatura/Temperatura.csproj](Temperatura/Temperatura.csproj): biblioteca com a regra de negocio.
- [Temperatura.Testes/Temperatura.Testes.csproj](Temperatura.Testes/Temperatura.Testes.csproj): projeto de testes unitarios.

A regra principal converte Fahrenheit para Celsius com arredondamento em 2 casas decimais:

- [Temperatura/ConversorTemperatura.cs](Temperatura/ConversorTemperatura.cs)

Os testes validam varios cenarios de entrada/saida usando [Theory] e [InlineData]:

- [Temperatura.Testes/TestesConversorTemperatura.cs](Temperatura.Testes/TestesConversorTemperatura.cs)

## Estrutura

```text
DotNet5-xUnit/
  Temperatura/
    ConversorTemperatura.cs
    Temperatura.csproj
  Temperatura.Testes/
    TestesConversorTemperatura.cs
    Temperatura.Testes.csproj
    Temperatura.Testes.sln
```

## Tecnologias e Dependencias

- .NET 9 (target framework net9.0)
- xUnit
- Microsoft.NET.Test.Sdk
- xunit.runner.visualstudio
- coverlet.collector

Dependencias de teste configuradas em:

- [Temperatura.Testes/Temperatura.Testes.csproj](Temperatura.Testes/Temperatura.Testes.csproj#L9)

## Como os testes funcionam

1. O metodo FahrenheitParaCelsius recebe um valor em Fahrenheit.
2. O calculo e feito com a formula:

   C = (F - 32) / 1.8

3. O resultado e arredondado para 2 casas decimais.
4. O xUnit executa o metodo de teste uma vez para cada [InlineData].
5. Cada execucao compara valor esperado e valor calculado com Assert.Equal.

Implementacao testada:

- [Temperatura/ConversorTemperatura.cs](Temperatura/ConversorTemperatura.cs#L7)

Teste principal:

- [Temperatura.Testes/TestesConversorTemperatura.cs](Temperatura.Testes/TestesConversorTemperatura.cs#L13)

## Pre-requisitos

- SDK .NET 9 instalado.
- Ambiente com comando dotnet disponivel no PATH.

Para conferir SDKs instalados:

```bash
dotnet --list-sdks
dotnet --list-runtimes
```

## Passo a passo para executar testes

1. Entrar na pasta do projeto de testes:

```bash
cd Temperatura.Testes
```

2. Restaurar dependencias:

```bash
dotnet restore
```

3. Executar os testes:

```bash
dotnet test
```

Resultado esperado:

- Build concluido com sucesso.
- Descoberta e execucao dos testes xUnit.
- Todos os testes passando.

## Executar testes com cobertura

Para coletar cobertura de codigo com o coletor configurado:

```bash
cd Temperatura.Testes
dotnet test --collect:"XPlat Code Coverage"
```

Os artefatos de cobertura sao gerados em uma pasta TestResults dentro do projeto de testes.

## Como adicionar novos casos de teste

1. Abrir [Temperatura.Testes/TestesConversorTemperatura.cs](Temperatura.Testes/TestesConversorTemperatura.cs).
2. Adicionar uma nova linha [InlineData] no metodo de teste.
3. Rodar novamente:

```bash
dotnet test
```

Exemplo de novo caso:

```csharp
[InlineData(50, 10)]
```

## Solucao de problemas

Erro comum:

- You must install or update .NET to run this application.

Causa:

- Incompatibilidade entre TargetFramework do projeto e runtime disponivel na maquina.

Correcao aplicada neste repositorio:

- Migracao de net5.0 para net9.0 em:
- [Temperatura/Temperatura.csproj](Temperatura/Temperatura.csproj#L4)
- [Temperatura.Testes/Temperatura.Testes.csproj](Temperatura.Testes/Temperatura.Testes.csproj#L4)

## Comandos rapidos

```bash
# executar todos os testes
cd Temperatura.Testes && dotnet test

# executar com detalhes
cd Temperatura.Testes && dotnet test -v normal

# coletar cobertura
cd Temperatura.Testes && dotnet test --collect:"XPlat Code Coverage"
```

1. Deu o mesmo erro de versionamento do .NET

<img width="935" height="367" alt="image" src="https://github.com/user-attachments/assets/eae5e221-84fe-4e11-a3f5-a0abfe0a806b" />

# DotNet5-Moq-xUnit-FluentAssertions

Este repositorio demonstra como testar uma regra de negocio em .NET usando:

- xUnit para os testes automatizados
- Moq para simular a dependencia externa
- FluentAssertions para deixar as assercoes mais legiveis

O foco do exemplo e validar a classe `AnaliseCredito`, que decide o status de uma consulta de credito a partir do retorno de um servico externo.

## Visao geral

A aplicacao esta dividida em dois projetos:

- `ConsultaCredito`: contem a logica de negocio e os tipos usados pela regra.
- `ConsultaCredito.Testes`: contem os testes unitarios da logica de negocio.

O teste nao acessa nenhum servico real. Em vez disso, ele usa um mock de `IServicoConsultaCredito` para controlar os cenarios e verificar se `AnaliseCredito` retorna o status correto.

## Estrutura do repositorio

### Projeto principal

#### [ConsultaCredito/AnaliseCredito.cs](ConsultaCredito/AnaliseCredito.cs)
Classe principal da regra de negocio. Recebe uma implementacao de `IServicoConsultaCredito` no construtor e expoe o metodo `ConsultarSituacaoCPF`.

Comportamento esperado:

- Se o servico retornar `null`, o status e `ParametroEnvioInvalido`.
- Se o servico retornar uma lista vazia, o status e `SemPendencias`.
- Se o servico retornar uma lista com itens, o status e `Inadimplente`.
- Se ocorrer qualquer excecao, o status e `ErroComunicacao`.

#### [ConsultaCredito/IServicoConsultaCredito.cs](ConsultaCredito/IServicoConsultaCredito.cs)
Interface que representa a dependencia externa usada pela regra de negocio. Ela define o metodo `ConsultarPendenciasPorCPF(string cpf)`.

#### [ConsultaCredito/OutrasEstruturas.cs](ConsultaCredito/OutrasEstruturas.cs)
Contem os tipos de apoio do dominio:

- `StatusConsultaCredito`: enum com os possiveis retornos da analise.
- `Pendencia`: classe com os dados de uma pendencia financeira.

#### [ConsultaCredito/ConsultaCredito.csproj](ConsultaCredito/ConsultaCredito.csproj)
Arquivo de projeto da biblioteca principal. No estado atual, o alvo configurado esta em `net9.0` para ser compativel com o runtime disponivel no ambiente.

### Projeto de testes

#### [ConsultaCredito.Testes/TestesAnaliseCredito.cs](ConsultaCredito.Testes/TestesAnaliseCredito.cs)
Arquivo com a classe de testes `TestesAnaliseCredito`.

Ele cria um `Mock<IServicoConsultaCredito>` e configura quatro cenarios:

- CPF invalido: o mock retorna `null`.
- Erro de comunicacao: o mock dispara excecao.
- CPF sem pendencias: o mock retorna uma lista vazia.
- CPF inadimplente: o mock retorna uma lista com uma pendencia.

Cada teste chama `AnaliseCredito.ConsultarSituacaoCPF` e verifica se o status retornado e o esperado.

#### [ConsultaCredito.Testes/ConsultaCredito.Testes.csproj](ConsultaCredito.Testes/ConsultaCredito.Testes.csproj)
Arquivo de projeto do conjunto de testes. Inclui as dependencias:

- `xunit`
- `xunit.runner.visualstudio`
- `Moq`
- `FluentAssertions`
- `Microsoft.NET.Test.Sdk`
- `coverlet.collector`

#### [ConsultaCredito.Testes/ConsultaCredito.Testes.sln](ConsultaCredito.Testes/ConsultaCredito.Testes.sln)
Solucao usada para organizar o projeto de testes.

## Como os testes funcionam

O objetivo dos testes e validar a regra de negocio sem depender de uma integracao real.

O fluxo e este:

1. O teste cria um mock de `IServicoConsultaCredito`.
2. O mock e configurado com resultados diferentes para CPFs especificos.
3. O teste instancia `AnaliseCredito` passando o mock no construtor.
4. O metodo `ConsultarSituacaoCPF` e executado.
5. O resultado e comparado com o status esperado usando FluentAssertions.

Isso garante isolamento, repetibilidade e rapidez na execucao.

## Cenarios cobertos

Os quatro testes da classe `TestesAnaliseCredito` cobrem os principais caminhos da regra:

- CPF invalido
- Falha de comunicacao com o servico
- CPF sem pendencias
- CPF inadimplente

Esses cenarios exercitam tanto o fluxo feliz quanto os retornos de erro previstos pela implementacao.

## Passo a passo para executar

### 1. Entrar na pasta do projeto de testes

```bash
cd ConsultaCredito.Testes
```

### 2. Restaurar dependencias

O `dotnet test` ja faz a restauracao automaticamente, mas voce tambem pode executar manualmente:

```bash
dotnet restore
```

### 3. Executar os testes

```bash
dotnet test
```

### 4. Interpretar o resultado

Se tudo estiver correto, voce deve ver algo parecido com:

- build concluido com sucesso
- 4 testes executados
- 0 falhas

## Resultado esperado

Ao final da execucao, os testes devem confirmar que a classe `AnaliseCredito` retorna:

- `ParametroEnvioInvalido` quando o servico retorna `null`
- `ErroComunicacao` quando o servico dispara excecao
- `SemPendencias` quando nao ha pendencias
- `Inadimplente` quando existe ao menos uma pendencia

## Observacao sobre a versao do .NET

Os projetos estao configurados atualmente para `net9.0`. Isso foi necessario para rodar os testes no ambiente disponivel, que ja possui runtime .NET 9 instalado.

Se voce quiser voltar a `net5.0`, sera necessario instalar o runtime correspondente na maquina de execucao.

## Resumo rapido

- A logica principal esta em `ConsultaCredito`.
- Os testes estao em `ConsultaCredito.Testes`.
- O mock com Moq permite simular respostas diferentes do servico.
- FluentAssertions deixa as validacoes mais legiveis.
- `dotnet test` e o comando principal para validar o projeto.



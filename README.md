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

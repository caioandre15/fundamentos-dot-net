# Fundamentos-dot-net
Fundamentos de .NET na prática.

## Injeção de depêndencia
Injeção de dependência (DI) é um padrão de design de codificação que faz parte dos princípios SOLID.  
A ideia é obter a IoC (Inversão de controle) para simplificar as responsabilidades de uma classe.  
O ASP.NET Core dá suporte a injeção de dependência de forma nativa.  

## SOLID  

SRP - Principio da responsabilidade única. (O mais simples e mais importante sem ele não é possível aplicar os demais).  
"Uma classe deve ter um, e apenas um motivo para ser modificada."  
  
OCP - Princípio do aberto e fechado.  (Classes fechadas e abertas para extensão, evita quebrar o que já está funcionando)  
"Entidades de software (classes, módulos, funções, etc) devem estar abertas para extensão, mas fechadas para modificação."  

LSP - Princípio de Liskov. 
"Classes derivas devem poder serem substituidas pela suas classes bases"  

ISP - Princípio de segregação de interfaces.  (Muitas interfaces especificas são melhores do que uma interface única.)
"Classes não devem ser forçados a depender de métodos que não usam."  

DIP - Princípio da injeção de dependência (Depender da abstração e não da implementação).
"Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações; 
Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações."

### DI - Ciclos de de vida

- Transiente:
  Obtém uma nova instância do objeto a cada solicitação

- Scoped:
  Reutiliza a mesma instância do objeto durante todo o request (Utilizado para padrões web)

- Singleton:
  Utiliza a mesma instância para toda a aplicação (cuidado)
  
## Entity Framework (EF)

O que é?
O meio para realizar acesso ao banco de dados em .NET.
ORM - Object Relational Mapper (Mapeia o objeto (Classe) para o mundo relacional).

Como instalar?
Package Manager Console
> install-package Microsoft.EntityFrameworkCore  
> install-package Microsoft.EntityFrameworkCore.SQLServer

Como configurar o contexto?
Criar uma classe que extenda a a classe DbContext, adicionando as options na clase base. Que será a classe principal para realizar o mapeamento.  
Adicionar este serviço na classe Program passando a connection string.  
Adicionar a connecion string no arquivo appsettings.  

Classe extende a DbContext:

````
public DataBaseContext(DbContextOptions options)
        : base(options)
        {

        }
````
Classe Program
````
builder.Services.AddDbContext<DataBaseContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection"));
});
````

Appsettings
````
"ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=bd-locator-dev;Trusted_Connection=True;MultipleActiveResultsSets=true"
  }
````



Migrations - Para criar e alterar os banco de dados.



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
Criar uma classe que extenda a a classe DbContext, adicionando as options na classe base. Que será a classe principal para realizar o mapeamento.  
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

### Criando Models e inserindo no contexto

Criar a Classe Entity e herda-las nas classes que serão criadas como model. Desta forma o Id sempre será um Guid, não sendo necessário adicionar o Id em cada classe.

````
public class Entity
    {
        protected Entity()
        {
            Id = Guid.NewGuid();
        }
        public Guid Id { get; set; }
    }
````

#### Inserindo Models na classe de contexto  

> public DbSet<Car> Cars { get; set; }
  
#### Inserindo o contexto na controller, por injeção de dependência.  
````
private readonly DataBaseContext _contexto;  

public CarsController(DataBaseContext contexto)
{
	_contexto = contexto
}
````
Adicionar  
````
_contexto.Cars.Add(car);  
_contexto.SaveChanges();  
````
Buscar  
````
_context.Cars.Find(car.Id);  
_context.Cars.FirstOrDefault(a => a.Plate == "BKT-1588");  
_context.Cars.Where(a => a.Name == "Corsa");    
````  
Atualizar  
````
_context.Cars.Update(car);  
_contexto.SaveChanges();  
````
Deletar  
````
_context.Cars.Remove(car);  
_context.SaveChanges();  
````
#### Migrations - Para criar e alterar os banco de dados.

A ideia do migration é verificar se o que está no DbContex está no banco de dados, caso não esteja é criado um script para realizar está sincronização.
Comando para adicionar uma nova Migration:  
````
add-migration "Nome da Migration" -Verbose
````
Atributos que podem ser utilizados no comando:  
-Context (Passar o nome do contexto caso tenha mais de um)  
-Verbose (Para ter mais detalhes de logs do que está ocorrendo) 
Comando para listar as Migrations existentes:  
````
get-migration	
````
Comando para deletar uma Migration:  
````
remove-migration
````
Comando para criar o banco e suas tabelas conforme a Migrations:
````
update-database -Verbose
````
#### Criando DTOs realizando o De/Para com Automapper  
Criar uma Pasta Dtos e adicionar classes obtendo os campos que devem ser expostos seguindo as regras de negócio.  

Comando para instalar o AutoMapper  
````
install-package automapper.extensions.microsoft.dependencyinjection
````
Adicionar serviço na classe Program:   
````
builder.Services.AddAutoMapper(typeof(Program));
````
Criar uma pasta para o AutoMapper e uma classe de configuração (AutoMapperConfig).  
Esta classe herdará da classe Profile, pois seu objetivo é ser uma classe de perfil de mapeamento.  
Dentro da classe criar um construtor com os mapeamentos, conforme exemplo:  
````
public class AutoMapperConfig : Profile
    {
        public AutoMapperConfig()
        {
            CreateMap<Car, CarViewModel>().ReverseMap();
        }
    }
````
No código o ReverseMap() faz o caminho inverso da conversão sem que haja a necessidade de criar um mapeamento para cada sentido.  







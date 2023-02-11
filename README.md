# fundamentos-dot-net
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



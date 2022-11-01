# DIO - Trilha .NET - API e Entity Framework
www.dio.me

## Desafio de projeto
Para este desafio, você precisará usar seus conhecimentos adquiridos no módulo de API e Entity Framework, da trilha .NET da DIO.

## Contexto
Você precisa construir um sistema gerenciador de tarefas, onde você poderá cadastrar uma lista de tarefas que permitirá organizar melhor a sua rotina.

Essa lista de tarefas precisa ter um CRUD, ou seja, deverá permitir a você obter os registros, criar, salvar e deletar esses registros.

A sua aplicação deverá ser do tipo Web API ou MVC, fique a vontade para implementar a solução que achar mais adequado.

A sua classe principal, a classe de tarefa, deve ser a seguinte:

![Diagrama da classe Tarefa](diagrama.png)

Não se esqueça de gerar a sua migration para atualização no banco de dados.

## Métodos esperados
É esperado que você crie o seus métodos conforme a seguir:


**Swagger**


![Métodos Swagger](swagger.png)


**Endpoints**


| Verbo  | Endpoint                | Parâmetro | Body          |
|--------|-------------------------|-----------|---------------|
| GET    | /Tarefa/{id}            | id        | N/A           |
| PUT    | /Tarefa/{id}            | id        | Schema Tarefa |
| DELETE | /Tarefa/{id}            | id        | N/A           |
| GET    | /Tarefa/ObterTodos      | N/A       | N/A           |
| GET    | /Tarefa/ObterPorTitulo  | titulo    | N/A           |
| GET    | /Tarefa/ObterPorData    | data      | N/A           |
| GET    | /Tarefa/ObterPorStatus  | status    | N/A           |
| POST   | /Tarefa                 | N/A       | Schema Tarefa |

Esse é o schema (model) de Tarefa, utilizado para passar para os métodos que exigirem

```json
{
  "id": 0,
  "titulo": "string",
  "descricao": "string",
  "data": "2022-06-08T01:31:07.056Z",
  "status": "Pendente"
}
```


## Solução
O código está pela metade, e você deverá dar continuidade obedecendo as regras descritas acima, para que no final, tenhamos um programa funcional. Procure pela palavra comentada "TODO" no código, em seguida, implemente conforme as regras acima.

Buscar o Id no banco utilizando o EF
´´´csharp
  var tarefa = _context.Tarefas.Find(id);
´´´

Validar o tipo de retorno. Se não encontrar a tarefa, retornar NotFound,
caso contrário retornar OK com a tarefa encontrada
´´´csharp
  if (tarefa == null)
      return NotFound();

  return Ok(tarefa);
´´´

Buscar todas as tarefas no banco utilizando o EF
´´´csharp
  return Ok(_context.Tarefas.Select(x => x).ToList());
´´´

Buscar  as tarefas no banco utilizando o EF, que contenha o titulo recebido por parâmetro
Dica: Usar como exemplo o endpoint ObterPorData
´´´csharp
  var tarefa = _context.Tarefas.Where(x => x.Titulo == titulo);
  return Ok(tarefa);
´´´

Buscar  as tarefas no banco utilizando o EF, que contenha o status recebido por parâmetro
Dica: Usar como exemplo o endpoint ObterPorData
´´´csharp
  var tarefa = _context.Tarefas.Where(x => x.Status == status);
  return Ok(tarefa);
´´´

Adicionar a tarefa recebida no EF e salvar as mudanças (save changes)
´´´csharp
  _context.Tarefas.Add(tarefa);
  _context.SaveChanges();
´´´

Atualizar as informações da variável tarefaBanco com a tarefa recebida via parâmetro
´´´csharp
  tarefaBanco.Data = tarefa.Data;
  tarefaBanco.Descricao = tarefa.Descricao;
  tarefaBanco.Status = tarefa.Status;
  tarefaBanco.Titulo = tarefa.Titulo;
´´´

Atualizar a variável tarefaBanco no EF e salvar as mudanças (save changes)
´´´csharp
  _context.Tarefas.Update(tarefaBanco);
  _context.SaveChanges();
  return Ok(tarefa);
´´´

Remover a tarefa encontrada através do EF e salvar as mudanças (save changes)
´´´csharp
  _context.Tarefas.Remove(tarefaBanco);
  _context.SaveChanges();
´´´
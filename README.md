
----------
# Utilizando GitHub Copilot no IntelliJ

## O que é o GitHub Copilot?

### Introdução

-   Ferramenta de assistência ao código baseada em IA, desenvolvida pela OpenAI e GitHub.
-   Gera sugestões de código em tempo real com base no contexto.

### Benefícios

-   Acelera o desenvolvimento.
-   Reduz erros comuns.
-   Auxilia na escrita de código em linguagens menos familiares.

----------

## O que vamos fazer?

Nesta demonstração, iremos adicionar uma nova funcionalidade em um projeto de lista de tarefas.

Resumo dos passos a serem seguidos:

1.  Clonar o repositório.
2.  Instalar e habilitar o plugin do GitHub Copilot no IntelliJ.
3.  Realizar login com sua conta do GitHub no plugin.
4.  Obter o código das funcionalidades a partir de prompts.

### Ferramentas Necessárias

-   IntelliJ IDEA (Community ou Ultimate).
-   Plugin GitHub Copilot instalado.
-   JDK 17 ou superior.
-   Clonar o repositório de exemplo: [https://github.com/JonasHenriqueDev/tododolist-dcext6.git](https://github.com/JonasHenriqueDev/tododolist-dcext6.git)

----------

## Desenvolvendo a funcionalidade

### Lista de Tarefas

1.  No IntelliJ, pressione a seguinte sequência de teclas para abrir o chat: **Ctrl + Shift + C**.
    
2.  Para este exemplo, definimos o seguinte prompt. Sinta-se livre para pedir qualquer outra coisa!
    

> Adicione uma nova feature para marcar uma tarefa como concluída.

Resposta:

```java
// src/main/java/br/upe/todolist/service/TaskService.java  

public Task completeTask(Long id) {  
    Optional<Task> taskOptional = taskRepository.findById(id);  
    if (taskOptional.isPresent()) {  
        Task task = taskOptional.get();  
        task.setFinished(true);  
        return taskRepository.save(task);  
    }  
    return null;  
}

```

```java
// src/main/java/br/upe/todolist/service/TaskController.java  

@PatchMapping("/{id}/complete")  
public ResponseEntity<Task> completeTask(@PathVariable Long id) {  
    Task completedTask = taskService.completeTask(id);  
    if (completedTask != null) {  
        return ResponseEntity.ok(completedTask);  
    }  
    return ResponseEntity.notFound().build();  
}

```

3.  Execute o projeto para garantir que está funcionando.

### BÔNUS! Gerando testes com o GitHub Copilot

Desenvolver testes unitários pode ser uma tarefa que demanda bastante tempo do desenvolvedor, mas, com a chegada das IAs, tais tarefas podem ser facilitadas.

1.  No IntelliJ, pressione a seguinte sequência de teclas para abrir o chat: **Ctrl + Shift + C**.
    
2.  Para gerar um teste de alguma classe, você pode escrever algo como este prompt:
    

> /tests TaskController

Resposta:

```java
package br.upe.todolist.controller;  

import br.upe.todolist.model.Task;  
import br.upe.todolist.service.TaskService;  
import org.junit.jupiter.api.Test;  
import org.mockito.InjectMocks;  
import org.mockito.Mock;  
import org.mockito.MockitoAnnotations;  
import org.springframework.http.ResponseEntity;  

import static org.junit.jupiter.api.Assertions.*;  
import static org.mockito.Mockito.*;  

class TaskControllerTest {  

    @Mock  
    private TaskService taskService;  

    @InjectMocks  
    private TaskController taskController;  

    public TaskControllerTest() {  
        MockitoAnnotations.openMocks(this);  
    }  

    @Test  
    void completeTaskSuccessfully() {  
        Long taskId = 1L;  
        Task task = new Task();  
        task.setId(taskId);  
        task.setFinished(true);  

        when(taskService.completeTask(taskId)).thenReturn(task);  

        ResponseEntity<Task> response = taskController.completeTask(taskId);  

        assertNotNull(response);  
        assertEquals(200, response.getStatusCodeValue());  
        assertNotNull(response.getBody());  
        assertTrue(response.getBody().isFinished());  
        verify(taskService, times(1)).completeTask(taskId);  
    }  

    @Test  
    void completeTaskNotFound() {  
        Long taskId = 1L;  

        when(taskService.completeTask(taskId)).thenReturn(null);  

        ResponseEntity<Task> response = taskController.completeTask(taskId);  

        assertNotNull(response);  
        assertEquals(404, response.getStatusCodeValue());  
        assertNull(response.getBody());  
        verify(taskService, times(1)).completeTask(taskId);  
    }  
}

```

3.  Execute o projeto para garantir que está funcionando.

## Conclusão

### Benefícios Observados

-   Aceleração no desenvolvimento.
-   Redução de erros comuns.
-   Maior produtividade ao focar em regras de negócio.

### Dicas para Uso do GitHub Copilot

-   Combine sugestões da IA com revisões manuais.
-   Use para aprender padrões e boas práticas.

----------

## Recursos Adicionais

-   [Documentação do GitHub Copilot](https://docs.github.com/en/copilot)
-   [Spring Boot](https://spring.io/projects/spring-boot)
-   [IntelliJ IDEA](https://www.jetbrains.com/idea/)

----------

# Desenvolvimento de Aplicação com Azure OpenAI e Semantic Kernel 🤖💻

Este repositório foi criado com o objetivo de apresentar um resumo contendo o desenvolvimento de uma aplicação prática utilizando o **Azure OpenAI** e a integração com o **Semantic Kernel**. A aplicação envolve o uso da API do Azure OpenAI para realizar tarefas de processamento de linguagem natural e utiliza o Semantic Kernel para orquestrar e estruturar o fluxo de tarefas de IA. Com base no conteudo apresentado no curso da DIO.

## Sumário

1. [Introdução](#introdução)
2. [Passos para Configuração e Desenvolvimento](#passos-para-configuração-e-desenvolvimento)
    - [Criação de Conta e Instância no Azure OpenAI](#criação-de-conta-e-instância-no-azure-openai)
    - [Fazendo uma Chamada à API do Azure OpenAI](#fazendo-uma-chamada-à-api-do-azure-openai)
    - [Integrando com a Aplicação](#integrando-com-a-aplicação)
3. [Introdução ao Semantic Kernel](#introdução-ao-semantic-kernel)
4. [Integração do Semantic Kernel com Azure OpenAI](#integração-do-semantic-kernel-com-azure-openai)
5. [Prompt Engineering no Azure OpenAI](#prompt-engineering-no-azure-openai)
6. [Conclusão](#conclusão)

## Introdução

O **Azure OpenAI** oferece acesso a poderosos modelos de IA, como GPT-3, GPT-4, e DALL·E, que podem ser integrados em suas aplicações para realizar tarefas como geração de texto, análise de sentimentos, tradução, etc. O **Semantic Kernel** é uma ferramenta desenvolvida pela Microsoft que permite orquestrar e combinar modelos de linguagem de forma eficiente, facilitando a criação de aplicações complexas com IA.

## Passos para Configuração e Desenvolvimento

### Criação de Conta e Instância no Azure OpenAI

1. Crie uma conta no **Azure** caso não tenha uma: [Portal Azure](https://azure.microsoft.com).
2. No portal do Azure, crie um novo **recurso do Azure OpenAI**. Você precisará escolher uma região e uma assinatura.
3. Após a criação do recurso, você terá acesso às credenciais (chave de API e endpoint) necessárias para interagir com a API do OpenAI.

### Fazendo uma Chamada à API do Azure OpenAI

Aqui está um exemplo de como fazer uma chamada à API do Azure OpenAI usando **Java** com a biblioteca **HttpURLConnection**:

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.io.OutputStream;

public class AzureOpenAIExample {
    public static void main(String[] args) {
        try {
            String apiKey = "sua-chave-de-api";
            String endpoint = "seu-endpoint-do-azure";
            URL url = new URL(endpoint);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();

            // Configurações da requisição
            connection.setRequestMethod("POST");
            connection.setRequestProperty("Authorization", "Bearer " + apiKey);
            connection.setRequestProperty("Content-Type", "application/json");
            connection.setDoOutput(true);

            // Corpo da requisição
            String jsonInputString = "{\n" +
                    "  \"model\": \"gpt-4\",\n" +
                    "  \"messages\": [{\"role\": \"user\", \"content\": \"Olá, como você está?\"}]\n" +
                    "}";

            // Enviar a requisição
            try (OutputStream os = connection.getOutputStream()) {
                byte[] input = jsonInputString.getBytes("utf-8");
                os.write(input, 0, input.length);
            }

            // Ler a resposta
            int responseCode = connection.getResponseCode();
            BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String inputLine;
            StringBuffer response = new StringBuffer();
            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();

            // Exibe o código de resposta e o conteúdo
            System.out.println("Response Code: " + responseCode);
            System.out.println("Response: " + response.toString());

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Este código envia uma mensagem para o modelo GPT-4 e imprime a resposta gerada.

### Integrando com a Aplicação

Uma vez que a API do Azure OpenAI esteja configurada e funcionando, você pode integrar esse código em uma aplicação maior, como um chatbot, assistente virtual ou qualquer outra aplicação que necessite de processamento de linguagem natural.

## Introdução ao Semantic Kernel

O **Semantic Kernel** é uma biblioteca de código aberto da Microsoft que facilita a criação de sistemas complexos de IA, permitindo a integração de múltiplos modelos de linguagem e a orquestração de fluxos de tarefas. Ele foi projetado para ser flexível, escalável e de fácil utilização, tornando mais simples a criação de aplicações que utilizam **Azure OpenAI** e outras APIs de IA.

### Funcionalidades Principais do Semantic Kernel:

- **Encadeamento de Tarefas (Task Chaining):** Permite combinar múltiplos modelos de IA em um único fluxo de trabalho.
- **Memória Contextual:** Mantém o contexto ao longo de uma sessão para tarefas como chatbots ou assistentes virtuais.
- **Habilidades Reutilizáveis (Skills):** Permite criar blocos de funcionalidades reutilizáveis, como geração de texto ou sumarização.

## Integração do Semantic Kernel com Azure OpenAI

Infelizmente, atualmente o **Semantic Kernel** não tem uma versão oficial em Java. No entanto, você pode usar Java para interagir com a API do Azure OpenAI e implementar a lógica de orquestração por conta própria.

## Prompt Engineering no Azure OpenAI

**Prompt Engineering** refere-se à prática de criar e otimizar os prompts (ou instruções) fornecidos aos modelos de IA para obter os melhores resultados possíveis. No contexto do **Azure OpenAI**, um bom prompt é essencial para garantir que o modelo compreenda corretamente a tarefa e forneça respostas precisas.

### Dicas de Prompt Engineering no Azure OpenAI:

1. **Clareza e Especificidade:**
   - Certifique-se de que seu prompt é claro e específico. Um prompt vago pode levar a respostas irrelevantes ou imprecisas.
   
   Exemplo de bom prompt:
   ```plaintext
   "Liste as principais capitais da Europa e forneça um fato interessante sobre cada uma."
   ```
   
   Exemplo de prompt vago:
   ```plaintext
   "Me fale sobre a Europa."
   ```

2. **Contexto e Instruções:**
   - Forneça contexto suficiente para o modelo entender o que você está pedindo. Se necessário, explique a tarefa de forma detalhada.
   
3. **Uso de Exemplos:**
   - Fornecer exemplos no prompt pode melhorar a precisão das respostas. Isso ajuda o modelo a entender o formato desejado para a resposta.

   Exemplo com exemplo:
   ```plaintext
   "Aqui está um exemplo de resposta. Agora, forneça um resumo do artigo que segue:
   Exemplo: 'O clima no Brasil é tropical e variado. Há regiões de clima quente e outras com clima frio.'
   Artigo: 'A Austrália é conhecida por sua diversidade climática...'"
   ```

4. **Teste e Iteração:**
   - O processo de criação de prompts eficientes é iterativo. Teste diferentes variações de prompts para ver qual gera os melhores resultados. 

---

## Conclusão

Neste repositório, tentei explicar de forma resumida a  como configurar uma aplicação utilizando **Azure OpenAI** e **Semantic Kernel**. O **Semantic Kernel** facilita a criação de fluxos de trabalho complexos, combinando diferentes modelos de IA de forma eficiente. Além disso, fornecemos uma introdução ao conceito de **Prompt Engineering**, que é crucial para otimizar as interações com os modelos de IA e obter respostas mais precisas.

### Para mais informações e esclarecimento de dúvidas, consulte as documentações oficiais 📚:

- [Documentação do Azure OpenAI](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/)
- [Documentação do Semantic Kernel](https://github.com/microsoft/semantic-kernel)
- [Documentação sobre Prompt Engineering no OpenAI](https://beta.openai.com/docs/guides/completion/prompt-design)


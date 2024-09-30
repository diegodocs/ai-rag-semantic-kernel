# Accelerate AI adoption with Semantic Kernel and RAG

## What is Semantic Kernel?

Semantic Kernel (SK) is a powerful open-source framework developed by Microsoft that enables developers to create AI applications that leverage natural language understanding (NLU) and reasoning capabilities. With SK, developers can create semantic-aware applications that understand the context and meaning of user input, making interactions more intelligent and intuitive.

### Pros and Cons of Semantic Kernel

#### Pros

1. Enhanced User Experience: SK enables more natural and intuitive interactions between users and applications, improving overall user satisfaction.

1. Contextual Understanding: By understanding the context and meaning behind user inputs, SK allows for more accurate and relevant responses.

1. Flexibility: SK can be integrated with various AI services, including Azure OpenAI, allowing developers to choose the best tools for their specific needs.

1. Open Source: As an open-source framework, SK encourages community contributions and continuous improvement.

#### Cons

1. Complexity: Implementing SK requires a good understanding of NLU and AI concepts, which can be challenging for some developers.

1. Performance: Depending on the complexity of the application and the quality of the training data, SK's performance may vary.

1. Dependency on AI Services: While SK's flexibility is a strength, it also means that developers may need to rely on multiple AI services, which can increase costs and dependencies.

## Code Example: Using .NET and Azure OpenAI with Semantic Kernel

Here's a simple example to get you started with Semantic Kernel using .NET and Azure OpenAI.

```csharp
using System;
using Microsoft.SemanticKernel;
using Azure.AI.OpenAI;
using Azure.Identity;

class Program
{
    static void Main(string[] args)
    {
        // Set up Azure OpenAI client
        var client = new OpenAIClient(new DefaultAzureCredential());

        // Initialize Semantic Kernel
        var kernel = new SemanticKernelBuilder()
            .WithOpenAI(client)
            .Build();

        // Define a simple semantic task
        var task = new SemanticTask
        {
            Name = "GetWeather",
            Description = "Fetches the weather forecast for a given location.",
            SemanticQuery = "What is the weather like in {location}?",
            Parameters = new[]
            {
                new SemanticParameter { Name = "location", Type = SemanticParameterType.String }
            }
        };

        // Register the task
        kernel.RegisterTask(task);

        // Execute the task
        var result = kernel.ExecuteTask("GetWeather", new { location = "New York" });

        // Output the result
        Console.WriteLine(result.ResponseText);
    }
}
```

## Why create multi-agent solutions ?

Using a multi-agent solution with Semantic Kernel and .NET can offer various benefits:

1. Parallel Processing: Multi-agent systems can handle multiple tasks simultaneously, increasing efficiency and reducing response times.

1. Scalability: You can easily add or remove agents based on the workload, making it easier to scale your application.

1. Specialization: Different agents can specialize in different tasks, allowing for more focused and expert responses.

1. Resilience: If one agent fails, others can take over its tasks, increasing the overall resilience of the system.

1. Modularity: Multi-agent systems are inherently modular, making it easier to update or replace individual components without affecting the entire system.

1. Incorporating Semantic Kernel with .NET allows you to leverage Microsoft's robust ecosystem, combining it with Azure's powerful AI capabilities for enhanced natural language understanding and contextual awareness. This makes your multi-agent solution even more effective and intelligent.

## Use Case: Car recommendation

### Prompt Engineering

- define your chat

```bash
You are a chatbot to recommend a list of cars to based on user info:

Output the recommendations in HTML format using these elements:
    -Brand
    -Model
    -Year
    -Price in Reais (R$)
    -Internal Size in centimeters (cm)
    -Description (100 characters max in portuguese)
    -Maintenance Rating
    -Internal Size Rating 
    -General Rating

User preferences:
    -Age:  years
    -Height: cm
    -Weight: kg
    -Married: yes/no
    -Children: number                                               
    -Max Car Value: values in R$
```

### Sample Chat Response

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Car Recommendations</title>
</head>
<body>
    <h2>Car Recommendations</h2>
    <p>Based on your preferences, here are some cars you might like:</p>

    <div class="car-recommendation">
        <h3>Brand: Toyota</h3>
        <p><strong>Model:</strong> Corolla</p>
        <p><strong>Year:</strong> 2020</p>
        <p><strong>Price:</strong> R$ 85,000</p>
        <p><strong>Internal Size:</strong> 270 cm</p>
        <p><strong>Description:</strong> Compacto e confiável, ideal para famílias pequenas.</p>
        <p><strong>Maintenance Rating:</strong> 8/10</p>
        <p><strong>Internal Size Rating:</strong> 7/10</p>
        <p><strong>General Rating:</strong> 8/10</p>
    </div>
    <hr>

    <div class="car-recommendation">
        <h3>Brand: Honda</h3>
        <p><strong>Model:</strong> Civic</p>
        <p><strong>Year:</strong> 2021</p>
        <p><strong>Price:</strong> R$ 95,000</p>
        <p><strong>Internal Size:</strong> 280 cm</p>
        <p><strong>Description:</strong> Espaçoso e confortável, perfeito para famílias maiores.</p>
        <p><strong>Maintenance Rating:</strong> 9/10</p>
        <p><strong>Internal Size Rating:</strong> 8/10</p>
        <p><strong>General Rating:</strong> 9/10</p>
    </div>
    <hr>

    <div class="car-recommendation">
        <h3>Brand: Chevrolet</h3>
        <p><strong>Model:</strong> Onix</p>
        <p><strong>Year:</strong> 2019</p>
        <p><strong>Price:</strong> R$ 75,000</p>
        <p><strong>Internal Size:</strong> 265 cm</p>
        <p><strong>Description:</strong> Econômico e compacto, ideal para o uso diário.</p>
        <p><strong>Maintenance Rating:</strong> 7/10</p>
        <p><strong>Internal Size Rating:</strong> 6/10</p>
        <p><strong>General Rating:</strong> 7/10</p>
    </div>

    <style>
        .car-recommendation {
            margin-bottom: 20px;
        }
        hr {
            border: 1px solid #ccc;
        }
    </style>
</body>
</html>

```

This example sets up a console application that interacts with the Azure OpenAI API. The user inputs their preferences, and the chatbot generates car recommendations in HTML format based on those preferences. You'll need to replace "https://your-openai-resource.openai.azure.com/" and "your-api-key" with your actual Azure OpenAI endpoint and API key.

Make sure to add the necessary NuGet packages:

- Azure.AI.OpenAI
- Azure.Core

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using Azure.AI.OpenAI;
using Azure.Core;

namespace CarRecommendationChatbot
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var endpoint = "https://your-openai-resource.openai.azure.com/";
            var apiKey = "your-api-key";

            var client = new OpenAIClient(new Uri(endpoint), new AzureKeyCredential(apiKey));

            Console.WriteLine("Welcome to the Car Recommendation Chatbot!");

            while (true)
            {
                Console.Write("You: ");
                var userInput = Console.ReadLine();

                if (string.IsNullOrEmpty(userInput))
                    continue;

                var prompt = $"You are a chatbot that recommends cars based on user preferences. " +
                             $"The user preferences are: {userInput}. " +
                             $"Provide the recommendations in HTML format using these elements: " +
                             $"Brand, Model, Year, Price in Reais (R$), Internal Size in centimeters (cm), " +
                             $"Description (100 characters max in Portuguese), Maintenance Rating, " +
                             $"Internal Size Rating, General Rating.";

                var response = await GetChatbotResponse(client, prompt);

                Console.WriteLine($"Chatbot: {response}");
            }
        }

        static async Task<string> GetChatbotResponse(OpenAIClient client, string prompt)
        {
            var options = new ChatCompletionsOptions
            {
                Messages =
                {
                    new ChatMessage(ChatRole.System, "You are a helpful assistant."),
                    new ChatMessage(ChatRole.User, prompt)
                }
            };

            var response = await client.GetChatCompletionsAsync("gpt-3.5-turbo", options);

            return response.Value.Choices[0].Message.Content;
        }
    }
}

```

## Retrieval-Augmented Generation (RAG)

Retrieval-Augmented Generation (RAG) is an advanced AI approach that combines the power of retrieval-based models and generative models to improve the quality and relevance of responses. It leverages a large corpus of documents or indexed data to retrieve the most relevant pieces of information and then uses a generative model to create coherent and contextually appropriate responses based on this information.

### How RAG Works

- Retrieval: When a user makes a query, the system first retrieves relevant documents or data from an indexed database, such as Azure Cognitive Search.

- Augmentation: The retrieved data is then fed into a generative model, such as OpenAI's language model, which uses the information to generate a detailed and contextually accurate response.

### Benefits of RAG

1. Improved Accuracy: By using relevant documents as a basis, the responses are more accurate and factual.

1. Contextual Relevance: The generative model can create responses that are highly relevant to the user's query, considering the context provided by the retrieved data.

1. Scalability: RAG can easily handle large datasets, making it suitable for applications requiring extensive knowledge bases.

1. Flexibility: It can be applied to various domains, from customer support to content creation, enhancing the usefulness and adaptability of AI systems.

1. Efficiency: Combining retrieval and generation reduces the workload on the generative model, making the entire process more efficient and faster.

Integrating RAG in your chatbot, especially in the car recommendation use case, would make the recommendations more personalized and accurate by retrieving specific car details from a vast database and then generating a response that fits the user's preferences.

### Prerequisites

- Ensure you have Azure Cognitive Search set up.
- Index your car data in Azure Cognitive Search.

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using Azure;
using Azure.AI.OpenAI;
using Azure.Search.Documents;
using Azure.Search.Documents.Models;
using Azure.Core;

namespace CarRecommendationChatbot
{
    class Program
    {
        private static OpenAIClient _openAIClient;
        private static SearchClient _searchClient;

        static async Task Main(string[] args)
        {
            var openAIEndpoint = "https://your-openai-resource.openai.azure.com/";
            var openAIKey = "your-openai-api-key";
            var searchEndpoint = "https://your-search-resource.search.windows.net";
            var searchApiKey = "your-search-api-key";
            var searchIndexName = "your-index-name";

            _openAIClient = new OpenAIClient(new Uri(openAIEndpoint), new AzureKeyCredential(openAIKey));
            _searchClient = new SearchClient(new Uri(searchEndpoint), new AzureKeyCredential(searchApiKey), searchIndexName);

            Console.WriteLine("Welcome to the Car Recommendation Chatbot!");

            while (true)
            {
                Console.Write("You: ");
                var userInput = Console.ReadLine();

                if (string.IsNullOrEmpty(userInput))
                    continue;

                var searchResults = await SearchCarData(userInput);
                var response = await GenerateChatbotResponse(searchResults, userInput);

                Console.WriteLine($"Chatbot: {response}");
            }
        }

        static async Task<SearchResults<SearchDocument>> SearchCarData(string query)
        {
            var options = new SearchOptions
            {
                Size = 5, // Limiting to top 5 results
            };

            var response = await _searchClient.SearchAsync<SearchDocument>(query, options);
            return response.Value;
        }

        static async Task<string> GenerateChatbotResponse(SearchResults<SearchDocument> searchResults, string userInput)
        {
            var documents = string.Join("\n", searchResults.GetResults().Select(doc => doc.Document["Description"].ToString()));

            var prompt = $"You are a chatbot that recommends cars based on user preferences. " +
                         $"The user preferences are: {userInput}. " +
                         $"Here are some potential car options: {documents}. " +
                         $"Provide the recommendations in HTML format using these elements: " +
                         $"Brand, Model, Year, Price in Reais (R$), Internal Size in centimeters (cm), " +
                         $"Description (100 characters max in Portuguese), Maintenance Rating, " +
                         $"Internal Size Rating, General Rating.";

            var options = new ChatCompletionsOptions
            {
                Messages =
                {
                    new ChatMessage(ChatRole.System, "You are a helpful assistant."),
                    new ChatMessage(ChatRole.User, prompt)
                }
            };

            var response = await _openAIClient.GetChatCompletionsAsync("gpt-3.5-turbo", options);

            return response.Value.Choices[0].Message.Content;
        }
    }
}

```

### Explanation

- SearchClient Initialization: Added SearchClient to interact with Azure Cognitive Search.

- SearchCarData Method: Retrieves car data based on user input from the search index.

- GenerateChatbotResponse Method: Uses both the search results and user input to generate a more personalized response.

### Steps

- Replace "https://your-openai-resource.openai.azure.com/", "your-openai-api-key", "https://your-search-resource.search.windows.net", "your-search-api-key", and "your-index-name" with your actual Azure resource details.

- Index your car data in Azure Cognitive Search to enable the RAG feature.

- This setup will allow your chatbot to retrieve relevant car data and generate a personalized response for the user. Let me know if you need any more details!

## Conclusion

Combining the power of Semantic Kernel (SK) and Retrieval-Augmented Generation (RAG) brings a revolutionary approach to building intelligent applications. Semantic Kernel enables contextual understanding and natural language interactions, providing a solid foundation for creating user-friendly and intuitive applications. On the other hand, RAG enhances these applications by integrating precise, contextually relevant information from vast data sources, ensuring responses are accurate, timely, and highly personalized.

Together, they form a robust framework that enhances user experiences, boosts application efficiency, and opens up new avenues for innovative solutions. Whether you're building chatbots, recommendation systems, or any other AI-driven applications, leveraging SK and RAG can significantly elevate your project's capabilities, making your applications smarter, more responsive, and better suited to meet user needs. Embracing these technologies promises a future where human-computer interactions are not only seamless but also deeply insightful and efficient.

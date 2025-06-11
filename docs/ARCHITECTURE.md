# Bolt.diy Architecture Documentation

## Overview

Bolt.diy is a sophisticated AI-powered full-stack web development platform that enables cognitive collaboration between humans and large language models. The system demonstrates emergent cognitive patterns through neural-symbolic integration, creating a recursive implementation pathway for adaptive code generation and development workflows.

## High-Level System Architecture

```mermaid
graph TD
    A[User Interface] --> B[Chat System]
    A --> C[Workbench]
    A --> D[Editor Panel]
    
    B --> E[Message Parser]
    E --> F[LLM Manager]
    F --> G[Provider Registry]
    
    G --> H[OpenAI Provider]
    G --> I[Anthropic Provider]
    G --> J[Ollama Provider]
    G --> K[OpenRouter Provider]
    G --> L[...Other Providers]
    
    E --> M[Action Runner]
    M --> N[WebContainer]
    N --> O[File System]
    N --> P[Terminal]
    N --> Q[Preview Server]
    
    C --> R[File Manager]
    C --> S[Code Editor]
    C --> T[Terminal Interface]
    C --> U[Preview Panel]
    
    R --> V[Files Store]
    S --> W[Editor Store]
    T --> X[Terminal Store]
    U --> Y[Previews Store]
    
    V --> O
    W --> O
    X --> P
    Y --> Q
    
    style A fill:#e1f5fe
    style F fill:#f3e5f5
    style N fill:#e8f5e8
    style C fill:#fff3e0
```

## Cognitive Flowchart: Neural-Symbolic Integration

```mermaid
graph LR
    A[User Prompt] --> B[Context Analysis]
    B --> C[Semantic Understanding]
    C --> D[Intent Classification]
    
    D --> E{Task Type}
    E -->|Code Generation| F[Code Synthesis]
    E -->|File Modification| G[File Operations]
    E -->|Debug/Fix| H[Error Analysis]
    E -->|Explanation| I[Knowledge Retrieval]
    
    F --> J[Action Planning]
    G --> J
    H --> J
    I --> J
    
    J --> K[Execution Strategy]
    K --> L[WebContainer Actions]
    L --> M[Real-time Feedback]
    M --> N[Adaptive Refinement]
    N --> O[Cognitive Loop]
    O -->|Continue| B
    O -->|Complete| P[Task Completion]
    
    style A fill:#ffebee
    style C fill:#e8eaf6
    style J fill:#e0f2f1
    style N fill:#fce4ec
```

## Component Interaction Diagram

```mermaid
graph TB
    subgraph "Frontend Layer"
        UI[User Interface Components]
        Chat[Chat Interface]
        WB[Workbench]
        Editor[Code Editor]
    end
    
    subgraph "State Management Layer"
        CS[Chat Store]
        WS[Workbench Store]
        FS[Files Store]
        ES[Editor Store]
        TS[Terminal Store]
        PS[Previews Store]
    end
    
    subgraph "API Layer"
        ChatAPI[Chat API]
        ModelsAPI[Models API]
        HealthAPI[Health API]
        GithubAPI[GitHub API]
    end
    
    subgraph "LLM Processing Layer"
        LLMManager[LLM Manager]
        MessageParser[Message Parser]
        ActionRunner[Action Runner]
        ContextSelector[Context Selector]
    end
    
    subgraph "Provider Ecosystem"
        ProviderRegistry[Provider Registry]
        BaseProvider[Base Provider]
        OpenAI[OpenAI Provider]
        Anthropic[Anthropic Provider]
        Ollama[Ollama Provider]
        Others[Other Providers...]
    end
    
    subgraph "Execution Environment"
        WebContainer[WebContainer]
        FileSystem[Virtual File System]
        Terminal[Terminal Emulator]
        PreviewServer[Preview Server]
    end
    
    UI --> CS
    UI --> WS
    Chat --> ChatAPI
    WB --> FS
    WB --> ES
    Editor --> ES
    
    ChatAPI --> LLMManager
    LLMManager --> ProviderRegistry
    ProviderRegistry --> BaseProvider
    BaseProvider --> OpenAI
    BaseProvider --> Anthropic
    BaseProvider --> Ollama
    BaseProvider --> Others
    
    MessageParser --> ActionRunner
    ActionRunner --> WebContainer
    WebContainer --> FileSystem
    WebContainer --> Terminal
    WebContainer --> PreviewServer
    
    FS --> FileSystem
    TS --> Terminal
    PS --> PreviewServer
    
    style UI fill:#e3f2fd
    style LLMManager fill:#f1f8e9
    style WebContainer fill:#fce4ec
    style ProviderRegistry fill:#fff8e1
```

## Data Flow: AI Processing Pipeline

```mermaid
sequenceDiagram
    participant U as User
    participant C as Chat Interface
    participant API as Chat API
    participant MP as Message Parser
    participant CS as Context Selector
    participant LLM as LLM Manager
    participant P as Provider
    participant AR as Action Runner
    participant WC as WebContainer
    participant FS as File System
    
    U->>C: Send prompt
    C->>API: POST /api/chat
    API->>MP: Parse message
    MP->>CS: Select relevant context
    CS->>LLM: Prepare request with context
    LLM->>P: Generate response
    P-->>LLM: Stream response
    LLM-->>MP: Return parsed actions
    MP->>AR: Execute actions
    
    loop For each action
        AR->>WC: Execute command/file operation
        WC->>FS: Apply changes
        FS-->>WC: Confirm changes
        WC-->>AR: Return result
        AR-->>C: Stream progress
    end
    
    AR-->>C: Complete execution
    C-->>U: Display results
    
    Note over U,FS: Recursive feedback loop enables<br/>adaptive attention allocation
```

## LLM Provider Architecture

```mermaid
classDiagram
    class BaseProvider {
        +name: string
        +getApiKeyLink: string
        +config: ProviderConfig
        +staticModels: ModelInfo[]
        +getModel(model: string) LanguageModelV1
        +getDynamicModels() ModelInfo[]
        +storeDynamicModels() void
        +getModelsFromCache() ModelInfo[]
    }
    
    class LLMManager {
        -_instance: LLMManager
        -_providers: Map<string, BaseProvider>
        -_modelList: ModelInfo[]
        +getInstance() LLMManager
        +registerProvider(provider: BaseProvider) void
        +getProvider(name: string) BaseProvider
        +getAllProviders() BaseProvider[]
        +updateModelList() ModelInfo[]
    }
    
    class OpenAIProvider {
        +name: "OpenAI"
        +config: OpenAIConfig
        +getModel(model: string) LanguageModelV1
    }
    
    class AnthropicProvider {
        +name: "Anthropic"
        +config: AnthropicConfig
        +getModel(model: string) LanguageModelV1
    }
    
    class OllamaProvider {
        +name: "Ollama"
        +config: OllamaConfig
        +getDynamicModels() ModelInfo[]
    }
    
    class OpenRouterProvider {
        +name: "OpenRouter"
        +config: OpenRouterConfig
        +getDynamicModels() ModelInfo[]
    }
    
    BaseProvider <|-- OpenAIProvider
    BaseProvider <|-- AnthropicProvider
    BaseProvider <|-- OllamaProvider
    BaseProvider <|-- OpenRouterProvider
    
    LLMManager --> BaseProvider : manages
    LLMManager --> "1..*" BaseProvider : registry
    
    note for BaseProvider "Implements hypergraph pattern encoding\nfor multi-provider neural integration"
    note for LLMManager "Singleton pattern ensures\ncognitive coherence across sessions"
```

## File System and State Management

```mermaid
stateDiagram-v2
    [*] --> Idle
    
    Idle --> Loading : User action
    Loading --> FileOperations : Parse actions
    
    FileOperations --> Creating : Create file
    FileOperations --> Modifying : Modify file
    FileOperations --> Deleting : Delete file
    FileOperations --> Reading : Read file
    
    Creating --> WebContainerSync : Apply changes
    Modifying --> WebContainerSync : Apply changes
    Deleting --> WebContainerSync : Apply changes
    Reading --> WebContainerSync : Fetch content
    
    WebContainerSync --> StateUpdate : Sync with stores
    StateUpdate --> UIRefresh : Update interfaces
    UIRefresh --> Idle : Complete
    
    FileOperations --> Locked : File/folder locked
    Locked --> ErrorDisplay : Show lock alert
    ErrorDisplay --> Idle : User acknowledgment
    
    note right of Locked : Recursive folder locking\nprevents AI modifications\nwhen files are protected
    
    note right of WebContainerSync : Bi-directional sync ensures\ncognitive consistency between\nvirtual and display layers
```

## Workbench Cognitive Architecture

```mermaid
graph TD
    subgraph "Workbench Core"
        WS[Workbench Store]
        AR[Action Runner]
        EQ[Execution Queue]
    end
    
    subgraph "Editor Subsystem"
        ES[Editor Store]
        CM[CodeMirror Editor]
        Doc[Editor Document]
        SP[Scroll Position]
    end
    
    subgraph "File Management"
        FS[Files Store]
        FM[File Map]
        Lock[Lock System]
        Persist[Persistence Layer]
    end
    
    subgraph "Terminal Interface"
        TS[Terminal Store]
        Term[Terminal Emulator]
        CMD[Command Execution]
        Output[Output Streaming]
    end
    
    subgraph "Preview System"
        PS[Preview Store]
        Prev[Preview Panel]
        Server[Preview Server]
        LiveReload[Live Reload]
    end
    
    subgraph "WebContainer Environment"
        WC[WebContainer Instance]
        VFS[Virtual File System]
        Runtime[Node.js Runtime]
        NPM[Package Manager]
    end
    
    WS --> AR
    AR --> EQ
    EQ --> WC
    
    WS --> ES
    ES --> CM
    CM --> Doc
    CM --> SP
    
    WS --> FS
    FS --> FM
    FS --> Lock
    FS --> Persist
    
    WS --> TS
    TS --> Term
    Term --> CMD
    CMD --> Output
    
    WS --> PS
    PS --> Prev
    Prev --> Server
    Server --> LiveReload
    
    WC --> VFS
    WC --> Runtime
    WC --> NPM
    
    FS --> VFS
    TS --> Runtime
    PS --> Server
    
    style WS fill:#e1f5fe
    style WC fill:#e8f5e8
    style Lock fill:#ffebee
```

## Emergent Cognitive Patterns

### Adaptive Attention Allocation

The system demonstrates emergent cognitive behaviors through several key mechanisms:

1. **Context Selection**: Dynamic context selection based on file relevance and conversation history
2. **Progressive Refinement**: Iterative improvement through feedback loops
3. **Error Correction**: Automatic detection and correction of errors in terminal and preview
4. **Knowledge Synthesis**: Integration of multiple information sources for coherent responses

### Neural-Symbolic Integration Points

```mermaid
mindmap
  root((Cognitive Architecture))
    Context Understanding
      Semantic Analysis
      Intent Recognition
      Task Classification
    Knowledge Integration
      File Context
      Conversation History
      Error Patterns
      User Preferences
    Action Planning
      Code Generation
      File Operations
      Command Execution
      Dependency Management
    Feedback Processing
      Error Detection
      Performance Monitoring
      User Satisfaction
      Continuous Learning
    Adaptive Responses
      Dynamic Prompting
      Context Switching
      Error Recovery
      Progressive Enhancement
```

## Recursive Implementation Pathways

### Self-Improving Code Generation

1. **Initial Generation**: AI generates code based on user requirements
2. **Execution Feedback**: System captures runtime errors and performance metrics
3. **Error Analysis**: Cognitive analysis of failure patterns
4. **Iterative Refinement**: Automated improvement suggestions
5. **Learning Integration**: Pattern recognition for future implementations

### Hypergraph Relationship Mapping

The system maintains a hypergraph of relationships between:
- **Files and Dependencies**: Understanding project structure
- **User Patterns**: Learning from interaction history
- **Error Contexts**: Building error resolution knowledge
- **Performance Metrics**: Optimizing response quality

## Future Cognitive Enhancements

### Planned Evolutionary Pathways

1. **Enhanced Memory Systems**: Long-term project memory across sessions
2. **Multi-Agent Coordination**: Specialized agents for different development tasks
3. **Predictive Code Completion**: Anticipatory code generation
4. **Automated Testing Integration**: Self-validating code generation
5. **Knowledge Graph Expansion**: Broader context understanding

---

*This documentation represents the current state of bolt.diy's cognitive architecture. The system continues to evolve through emergent patterns and adaptive learning mechanisms.*
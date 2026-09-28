Bidirectional Agent for real-time streaming conversations.

Provides real-time audio and text interaction through persistent streaming connections. Unlike traditional request-response patterns, this agent maintains long-running conversations where users can barge in, provide additional input, and receive continuous responses including audio output.

Key capabilities:

-   Persistent conversation connections with concurrent processing
-   Real-time audio input/output streaming
-   Automatic barge-in detection and tool execution
-   Event-driven communication with model providers

## BidiAgent

```python
class BidiAgent(LocalAgent)
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:88](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L88)

Agent for bidirectional streaming conversations.

Enables real-time audio and text interaction with AI models through persistent connections. Supports concurrent tool execution and barge-in handling.

#### \_\_init\_\_

```python
def __init__(model: BidiModel | str | None = None,
             tools: list[str | AgentTool | ToolProvider] | None = None,
             system_prompt: str | list[SystemContentBlock] | None = None,
             messages: Messages | None = None,
             record_direct_tool_call: bool = True,
             load_tools_from_directory: bool = False,
             agent_id: str | None = None,
             name: str | None = None,
             description: str | None = None,
             hooks: list[HookProvider] | None = None,
             state: AgentState | dict | None = None,
             session_manager: "SessionManager[LocalAgent] | None" = None,
             tool_executor: ToolExecutor | None = None,
             storage: Storage | None = None,
             **kwargs: Any)
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:97](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L97)

Initialize bidirectional agent.

**Arguments**:

-   `model` - BidiModel instance, Bedrock model ID string, or None to use Nova Sonic 2.
-   `tools` - Optional list of tools with flexible format support.
-   `system_prompt` - System prompt for conversations as a string or structured content blocks. Structured blocks are retained, while their text is passed to Bidi models as a string.
-   `messages` - Optional conversation history to initialize with.
-   `record_direct_tool_call` - Whether to record direct tool calls in message history.
-   `load_tools_from_directory` - Whether to load and automatically reload tools in the `./tools/` directory.
-   `agent_id` - Optional ID for the agent, useful for connection management and multi-agent scenarios.
-   `name` - Name of the Agent.
-   `description` - Description of what the Agent does.
-   `hooks` - Optional list of hook providers to register for lifecycle events.
-   `state` - Stateful information for the agent. Can be either an AgentState object, or a json serializable dict.
-   `session_manager` - Manager for handling agent sessions including conversation history and state. If provided, enables session-based persistence and state management.
-   `tool_executor` - Definition of tool execution strategy (e.g., sequential, concurrent, etc.).
-   `storage` - Default storage backend for agent subsystems. When provided, subsystems that do not have their own explicit storage (e.g., SessionManager) resolve from this value. Each subsystem auto-namespaces under its own prefix to avoid key collisions. Storage specified directly on a subsystem always takes precedence over this agent-level default. Defaults to None.
-   `**kwargs` - Additional configuration for future extensibility.

**Raises**:

-   `ValueError` - If model configuration is invalid or state is invalid type.
-   `TypeError` - If model type is unsupported.

#### tool

```python
@property
def tool() -> _ToolCaller
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:235](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L235)

Call tool as a function.

**Returns**:

ToolCaller for method-style tool execution.

**Example**:

```plaintext
agent = BidiAgent(model=model, tools=[calculator])
agent.tool.calculator(expression="2+2")
```

#### tool\_names

```python
@property
def tool_names() -> list[str]
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:250](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L250)

Get a list of all registered tool names.

**Returns**:

Names of all tools available to this agent.

#### system\_prompt

```python
@property
def system_prompt() -> str | None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:260](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L260)

Get the system prompt as a string.

#### system\_prompt

```python
@system_prompt.setter
def system_prompt(value: str | list[SystemContentBlock] | None) -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:265](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L265)

Set the system prompt and retain its structured content representation.

#### system\_prompt\_content

```python
@property
def system_prompt_content() -> list[SystemContentBlock] | None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:270](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L270)

Get the system prompt as structured content blocks.

#### session\_id

```python
@property
def session_id() -> str
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:275](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L275)

Get the conversation session identifier.

#### storage

```python
@property
def storage() -> Storage | None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:280](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L280)

Default storage backend for agent subsystems.

#### sandbox

```python
@property
def sandbox() -> Sandbox
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:285](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L285)

Execution environment for tool code: the host, with no isolation.

#### context\_manager

```python
@property
def context_manager() -> "ContextManager | None"
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:290](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L290)

The ContextManager plugin; always None because bidirectional agents do not support plugins.

#### event\_loop\_metrics

```python
@property
def event_loop_metrics() -> "EventLoopMetrics"
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:295](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L295)

Raise because bidirectional agents do not collect event loop metrics yet.

#### cancel\_signal

```python
@property
def cancel_signal() -> threading.Event
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:304](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L304)

The cancellation signal; never set yet, because bidirectional agents do not act on it.

#### add\_hook

```python
def add_hook(callback: HookCallback[TEvent],
             event_type: type[TEvent] | list[type[TEvent]] | None = None,
             *,
             order: float = HookOrder.DEFAULT) -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:308](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L308)

Register a callback function for a specific event type.

This method supports multiple call patterns:

1.  `add_hook(callback)` - Event type inferred from callback’s type hint
2.  `add_hook(callback, event_type)` - Event type specified explicitly
3.  `add_hook(callback, [TypeA, TypeB])` - Register for multiple event types

When the callback’s type hint is a union type (`A | B` or `Union[A, B]`), the callback is automatically registered for each event type in the union.

Callbacks can be either synchronous or asynchronous functions.

**Arguments**:

-   `callback` - The callback function to invoke when events of this type occur.
-   `event_type` - The class type(s) of events this callback should handle. Can be a single type, a list of types, or None to infer from the callback’s first parameter type hint. If a list is provided, the callback is registered for each type in the list.
-   `order` - Execution priority. Lower values execute first. Use a HookOrder constant such as SDK\_FIRST (-100), DEFAULT (0), MODEL\_ROUTING (50), or SDK\_LAST (100).

**Raises**:

-   `ValueError` - If event\_type is not provided and cannot be inferred from the callback’s type hints, or if the event\_type list is empty.

#### start

```python
async def start(invocation_state: dict[str, Any] | None = None) -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:343](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L343)

Start a persistent bidirectional conversation connection.

Initializes the streaming connection and starts background tasks for processing model events, tool execution, and connection management.

**Arguments**:

-   `invocation_state` - Optional context shared by reference with tools and hooks until stop(), including across connection restarts. Tools access it through ToolContext.invocation\_state. Defaults to a new empty dictionary.

**Raises**:

RuntimeError: If agent already started.

**Example**:

```python
await agent.start(invocation_state=\{
    "user_id": "user_123",
    "session_id": "session_456",
    "database": db_connection,
})
```

#### send

```python
async def send(input_data: BidiAgentInput) -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:374](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L374)

Send user content to the model.

Strings are shorthand for text blocks. Lists of text and image blocks form one user message, preserving block order. Audio deltas are sent individually and are not added to conversation history. Tool results are sent by the agent’s tool runner.

**Arguments**:

-   `input_data` - Can be:
    
    -   str: Text message from user
    -   TextBlock, AudioDelta, or ImageBlock: Text, streaming audio, or image input
    -   BidiUserContentBlockData: A dictionary containing one text or image key
    -   BidiContentDeltaData: A dictionary containing one audio\_delta key
    -   list: A non-empty list of strings, text or image blocks, or their dictionary forms

**Raises**:

-   `RuntimeError` - If start has not been called.
-   `TypeError` - If the input has an unsupported type or invalid input arguments.
-   `ValueError` - If the input list is empty or an input dictionary does not contain exactly one supported key.

**Example**:

await agent.send(“Hello”) await agent.send(AudioDelta(format=“pcm”, source={“bytes”: audio\_bytes})) await agent.send({“audio\_delta”: {“format”: “pcm”, “source”: {“bytes”: audio\_bytes}}}) await agent.send(\[TextBlock(“Use these details.”), TextBlock(“Order number: 123.”)\])

#### receive

```python
async def receive() -> AsyncGenerator[BidiOutputEvent, None]
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:436](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L436)

Receive events from the model including audio, text, and tool calls.

**Yields**:

Model output events processed by background tasks including audio output, text responses, tool calls, and connection updates.

**Raises**:

-   `RuntimeError` - If start has not been called.

#### stop

```python
async def stop() -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:452](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L452)

End the conversation connection and cleanup all resources.

Terminates the streaming connection, cancels background tasks, and closes the connection to the model provider.

#### take\_snapshot

```python
def take_snapshot(*,
                  preset: SnapshotPreset | None = None,
                  include: list[SnapshotField] | None = None,
                  exclude: list[SnapshotField] | None = None,
                  app_data: dict[str, Any] | None = None) -> Snapshot
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:461](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L461)

Capture current agent state as an in-memory snapshot.

Captures committed conversation history and application state. Live connection state, in-progress responses, and pending tool calls are not included.

**Arguments**:

-   `preset` - Named preset of fields to capture. Currently only “session” is supported, which captures messages and state.
-   `include` - Additional fields to capture on top of the preset.
-   `exclude` - Fields to remove after applying preset and include.
-   `app_data` - Application-owned arbitrary JSON stored verbatim in the snapshot.

**Returns**:

A Snapshot containing the captured agent state.

**Raises**:

-   `SnapshotException` - If no fields are resolved or a field is invalid or unsupported.

#### load\_snapshot

```python
def load_snapshot(snapshot: Snapshot) -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:512](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L512)

Restore agent state from a previously captured snapshot.

Only fields present in snapshot.data are restored; absent fields are left unchanged and fields this agent does not support are ignored. The restored history is sent to the model on the next start().

**Arguments**:

-   `snapshot` - The snapshot to restore from.

**Raises**:

-   `SnapshotException` - If snapshot.schema\_version is not “1.0” or snapshot.scope is not “agent”.
-   `RuntimeError` - If the agent is started.

#### \_\_aenter\_\_

```python
async def __aenter__(
        invocation_state: dict[str, Any] | None = None) -> "BidiAgent"
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:541](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L541)

Async context manager entry point.

Automatically starts the bidirectional connection when entering the context.

**Arguments**:

-   `invocation_state` - Optional context to pass to tools during execution. This allows passing custom data (user\_id, session\_id, database connections, etc.) that tools can access via their invocation\_state parameter.

**Returns**:

Self for use in the context.

#### \_\_aexit\_\_

```python
async def __aexit__(*_: Any) -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:558](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L558)

Async context manager exit point.

Automatically ends the connection and cleans up resources including when exiting the context, regardless of whether an exception occurred.

#### run

```python
async def run(inputs: list[InputStream],
              outputs: list[OutputStream],
              invocation_state: dict[str, Any] | None = None) -> None
```

Defined in: [src/strands/experimental/bidi/agent/agent.py:567](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/experimental/bidi/agent/agent.py#L567)

Run the agent using provided I/O streams for bidirectional communication.

**Arguments**:

-   `inputs` - Streams that produce input for the agent.
-   `outputs` - Streams that consume output events from the agent.
-   `invocation_state` - Optional context shared by reference with tools and hooks for the duration of run(), including across connection restarts. Tools access it through ToolContext.invocation\_state. Defaults to a new empty dictionary.

**Example**:

```python
# Using default audio settings:
model = BedrockNovaSonicModel(model_id="amazon.nova-2-sonic-v1:0")
audio_io = AudioIO()
agent = BidiAgent(model=model, tools=[calculator])
await agent.run(
    inputs=[audio_io.input()],
    outputs=[audio_io.output()],
    invocation_state=\{"user_id": "user_123"}
)

# Using custom audio config:
model = BedrockNovaSonicModel(
    model_id="amazon.nova-2-sonic-v1:0",
    audio=\{
        "input": \{"sample_rate": 16000},
        "output": \{"sample_rate": 24000},
    }
)
audio_io = AudioIO()
agent = BidiAgent(model=model, tools=[calculator])
await agent.run(
    inputs=[audio_io.input()],
    outputs=[audio_io.output()],
)
```
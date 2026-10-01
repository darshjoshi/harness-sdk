Content-related type definitions for bidirectional streaming.

#### BidiUserContentBlock

A complete text or image block supplied by a user.

#### BidiContentBlock

A complete text, image, or tool result block.

#### BidiContentDelta

An audio delta for the live input stream.

#### BidiUserContentBlockData

Dictionary form of one user content block.

#### BidiContentBlockData

Dictionary form of one text, image, or tool result block.

#### BidiContentDeltaData

Dictionary form of an audio delta.

## BidiMessage

```python
@dataclass
class BidiMessage()
```

Defined in: [src/strands/bidi/types/content.py:33](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/content.py#L33)

An input message containing ordered content blocks.

Callers must supply at least one block when sending and must not mix tool results with user text or images. Send streaming deltas individually.

**Attributes**:

-   `content` - Ordered list of complete content blocks.

## BidiContentMetadata

```python
class BidiContentMetadata(TypedDict)
```

Defined in: [src/strands/bidi/types/content.py:46](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/content.py#L46)

Streamed content metadata stored under a message’s metadata.custom.bidi.

**Attributes**:

-   `kind` - Identifies the message as text, reasoning, or a transcript.
-   `status` - Whether the content is pending, complete, or incomplete.

Agent-related type definitions for bidirectional streaming.

This module defines the types used for BidiAgent.

#### BidiAgentInput

A single user input or list of user content blocks.

Media input types for bidirectional streaming.

## AudioDelta

```python
@dataclass
class AudioDelta()
```

Defined in: [src/strands/bidi/types/media.py:15](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/media.py#L15)

Audio samples to append to the live input stream.

Sending a delta does not explicitly end the user’s turn.

**Attributes**:

-   `format` - Audio format.
-   `source` - Source containing the audio samples.

#### to\_dict

```python
def to_dict() -> _AudioDeltaData
```

Defined in: [src/strands/bidi/types/media.py:28](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/media.py#L28)

Return the dictionary form of this delta.

Protocols for bidirectional input and output streams.

The protocols separate input and output concerns into independent callables with lifecycle methods managed by `BidiAgent`.

## InputStream

```python
@runtime_checkable
class InputStream(Protocol)
```

Defined in: [src/strands/bidi/types/io.py:18](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L18)

Callable input stream managed by a bidirectional agent.

An input stream reads one value from a source each time the agent calls it.

#### start

```python
async def start(agent: "BidiAgent") -> None
```

Defined in: [src/strands/bidi/types/io.py:24](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L24)

Start input.

#### stop

```python
async def stop() -> None
```

Defined in: [src/strands/bidi/types/io.py:28](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L28)

Stop input.

#### \_\_call\_\_

```python
def __call__() -> Awaitable[BidiAgentInput]
```

Defined in: [src/strands/bidi/types/io.py:32](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L32)

Read input data from the source.

**Returns**:

Awaitable that resolves to input content (audio, text, image, etc.)

## OutputStream

```python
@runtime_checkable
class OutputStream(Protocol)
```

Defined in: [src/strands/bidi/types/io.py:42](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L42)

Callable output stream managed by a bidirectional agent.

An output stream handles one event each time the agent calls it.

#### start

```python
async def start(agent: "BidiAgent") -> None
```

Defined in: [src/strands/bidi/types/io.py:48](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L48)

Start output.

#### stop

```python
async def stop() -> None
```

Defined in: [src/strands/bidi/types/io.py:52](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L52)

Stop output.

#### \_\_call\_\_

```python
def __call__(event: BidiOutputEvent) -> Awaitable[None]
```

Defined in: [src/strands/bidi/types/io.py:56](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/io.py#L56)

Process output events from the agent.

**Arguments**:

-   `event` - Output event from the agent (audio, text, tool calls, etc.)

Output event types for bidirectional streaming.

Defines the provider-agnostic events produced by bidirectional models and `BidiAgent`: connection lifecycle (start, restart, warning, stop), response start and stop, audio, text, reasoning, and transcript streams (start, delta, stop, and the completed block), barge-in, token usage, and tool-use groups. Also defines the `AudioChannel`, `AudioFormat`, and `Role` literals and the `BidiOutputEvent` union.

#### AudioChannel

Number of audio channels.

-   Mono: 1
-   Stereo: 2

#### AudioFormat

Audio encoding format of model audio output and `AudioStreamConfig`.

Distinct from `strands.types.media.AudioFormat`, the wider set of formats that types `AudioDelta.format` on audio input.

#### Role

Role of a message sender.

-   “user”: Messages from the user to the assistant.
-   “assistant”: Messages from the assistant to the user.

## BidiConnectionStartEvent

```python
class BidiConnectionStartEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:71](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L71)

Streaming connection established and ready for interaction.

**Arguments**:

-   `connection_id` - Unique identifier for this streaming connection.
-   `model` - Model identifier (e.g., “gpt-realtime-2.1”, “gemini-3.8-live”).

#### \_\_init\_\_

```python
def __init__(connection_id: str, model: str)
```

Defined in: [src/strands/bidi/types/events.py:79](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L79)

Initialize connection start event.

#### connection\_id

```python
@property
def connection_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:90](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L90)

Unique identifier for this streaming connection.

#### model

```python
@property
def model() -> str
```

Defined in: [src/strands/bidi/types/events.py:95](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L95)

Model identifier (e.g., ‘gpt-realtime-2.1’, ‘gemini-3.8-live’).

## BidiConnectionRestartEvent

```python
class BidiConnectionRestartEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:100](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L100)

Agent is restarting the model connection.

Emitted on both restart paths: reactively after the model reports a timeout, and proactively when the restart timer fires ahead of the provider’s limit.

**Arguments**:

-   `reason` - What triggered the restart (“timeout” reactively, “scheduled” proactively).
-   `timeout_error` - The model’s timeout error on the reactive path; None when scheduled.
-   `turn_interrupted` - True if the restart cut off an in-progress assistant response or a user turn that had not been answered yet. The new connection receives the history as context, so that turn is not answered on its own; an app can re-prompt or notify the user when this is set.

#### \_\_init\_\_

```python
def __init__(reason: Literal["timeout", "scheduled"],
             timeout_error: "ConnectionTimeoutError | None" = None,
             turn_interrupted: bool = False)
```

Defined in: [src/strands/bidi/types/events.py:115](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L115)

Initialize connection restart event.

#### reason

```python
@property
def reason() -> Literal["timeout", "scheduled"]
```

Defined in: [src/strands/bidi/types/events.py:132](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L132)

What triggered the restart (“timeout” or “scheduled”).

#### timeout\_error

```python
@property
def timeout_error() -> "ConnectionTimeoutError | None"
```

Defined in: [src/strands/bidi/types/events.py:137](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L137)

Connection timeout error on the reactive path; None when scheduled.

#### turn\_interrupted

```python
@property
def turn_interrupted() -> bool
```

Defined in: [src/strands/bidi/types/events.py:142](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L142)

True if the restart cut off an in-progress response or an unanswered user turn.

## BidiConnectionWarningEvent

```python
class BidiConnectionWarningEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:147](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L147)

Agent is approaching a proactive restart.

Emitted by the proactive restart timer before a restart; informational only.

**Arguments**:

-   `time_left_s` - Approximate seconds until the scheduled restart.

#### \_\_init\_\_

```python
def __init__(time_left_s: float)
```

Defined in: [src/strands/bidi/types/events.py:156](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L156)

Initialize connection warning event.

#### time\_left\_s

```python
@property
def time_left_s() -> float
```

Defined in: [src/strands/bidi/types/events.py:166](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L166)

Approximate seconds until the scheduled restart.

## BidiResponseStartEvent

```python
class BidiResponseStartEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:171](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L171)

Start of a model response.

**Arguments**:

-   `response_id` - Unique identifier for this response (used in BidiResponseStopEvent).

#### \_\_init\_\_

```python
def __init__(response_id: str)
```

Defined in: [src/strands/bidi/types/events.py:178](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L178)

Initialize response start event.

#### response\_id

```python
@property
def response_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:183](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L183)

Unique identifier for this response.

## BidiAudioStartEvent

```python
class BidiAudioStartEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:188](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L188)

Beginning of an assistant audio stream, identified by `content_id`.

#### \_\_init\_\_

```python
def __init__(content_id: str) -> None
```

Defined in: [src/strands/bidi/types/events.py:191](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L191)

Initialize audio start event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:196](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L196)

Identifier shared by this audio stream’s events.

## BidiAudioDeltaEvent

```python
class BidiAudioDeltaEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:201](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L201)

Incremental audio output from the model.

**Arguments**:

-   `audio` - Base64-encoded audio chunk.
-   `format` - Audio encoding format.
-   `sample_rate` - Number of audio samples per second in Hz.
-   `channels` - Number of audio channels (1=mono, 2=stereo).
-   `content_id` - Unique identifier shared by this audio stream’s events.

#### \_\_init\_\_

```python
def __init__(audio: str, format: AudioFormat, sample_rate: int,
             channels: AudioChannel, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:212](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L212)

Initialize audio delta event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:233](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L233)

Identifier shared by this audio stream’s events.

#### audio

```python
@property
def audio() -> str
```

Defined in: [src/strands/bidi/types/events.py:238](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L238)

Base64-encoded audio chunk.

#### format

```python
@property
def format() -> AudioFormat
```

Defined in: [src/strands/bidi/types/events.py:243](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L243)

Audio encoding format.

#### sample\_rate

```python
@property
def sample_rate() -> int
```

Defined in: [src/strands/bidi/types/events.py:248](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L248)

Number of audio samples per second in Hz.

#### channels

```python
@property
def channels() -> AudioChannel
```

Defined in: [src/strands/bidi/types/events.py:253](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L253)

Number of audio channels (1=mono, 2=stereo).

## BidiAudioStopEvent

```python
class BidiAudioStopEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:258](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L258)

End of an assistant audio stream, which may still be playing.

#### \_\_init\_\_

```python
def __init__(content_id: str) -> None
```

Defined in: [src/strands/bidi/types/events.py:261](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L261)

Initialize audio stop event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:266](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L266)

Identifier shared by this audio stream’s events.

## BidiTextStartEvent

```python
class BidiTextStartEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:271](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L271)

Beginning of assistant text output, identified by `content_id`.

#### \_\_init\_\_

```python
def __init__(content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:274](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L274)

Initialize text start event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:279](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L279)

Identifier shared by this text block’s events.

## BidiTextDeltaEvent

```python
class BidiTextDeltaEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:284](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L284)

Incremental assistant text output, separate from speech transcripts.

#### \_\_init\_\_

```python
def __init__(delta: str, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:287](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L287)

Initialize text delta event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:292](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L292)

Identifier shared by this text block’s events.

#### delta

```python
@property
def delta() -> str
```

Defined in: [src/strands/bidi/types/events.py:297](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L297)

Incremental text.

## BidiTextStopEvent

```python
class BidiTextStopEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:302](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L302)

End of an assistant text stream, before its completed block is emitted.

#### \_\_init\_\_

```python
def __init__(content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:305](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L305)

Initialize text stop event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:310](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L310)

Identifier shared by this text block’s events.

## BidiTextBlockEvent

```python
class BidiTextBlockEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:315](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L315)

Complete assistant text, emitted after its stop event by the agent.

#### \_\_init\_\_

```python
def __init__(text: str, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:318](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L318)

Initialize text block event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:323](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L323)

Identifier shared by this text block’s events.

#### text

```python
@property
def text() -> str
```

Defined in: [src/strands/bidi/types/events.py:328](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L328)

Complete text.

## BidiReasoningStartEvent

```python
class BidiReasoningStartEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:333](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L333)

Beginning of model-provided reasoning text, identified by `content_id`.

#### \_\_init\_\_

```python
def __init__(content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:336](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L336)

Initialize reasoning start event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:341](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L341)

Identifier shared by this reasoning block’s events.

## BidiReasoningDeltaEvent

```python
class BidiReasoningDeltaEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:346](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L346)

Incremental reasoning text or thought summary exposed by the model.

#### \_\_init\_\_

```python
def __init__(delta: str, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:349](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L349)

Initialize reasoning delta event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:354](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L354)

Identifier shared by this reasoning block’s events.

#### delta

```python
@property
def delta() -> str
```

Defined in: [src/strands/bidi/types/events.py:359](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L359)

Incremental reasoning text.

## BidiReasoningStopEvent

```python
class BidiReasoningStopEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:364](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L364)

End of a reasoning stream, before its completed block is emitted.

#### \_\_init\_\_

```python
def __init__(content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:367](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L367)

Initialize reasoning stop event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:372](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L372)

Identifier shared by this reasoning block’s events.

## BidiReasoningBlockEvent

```python
class BidiReasoningBlockEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:377](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L377)

Complete reasoning text, emitted after its stop event by the agent.

#### \_\_init\_\_

```python
def __init__(text: str, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:380](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L380)

Initialize reasoning block event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:385](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L385)

Identifier shared by this reasoning block’s events.

#### text

```python
@property
def text() -> str
```

Defined in: [src/strands/bidi/types/events.py:390](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L390)

Complete reasoning text or thought summary.

## BidiTranscriptStartEvent

```python
class BidiTranscriptStartEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:395](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L395)

Beginning of a user or assistant transcript, before its text arrives.

**Arguments**:

-   `role` - Who is speaking (“user” or “assistant”).
-   `content_id` - Unique identifier shared by this transcript’s events.

#### \_\_init\_\_

```python
def __init__(role: Role, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:403](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L403)

Initialize transcript start event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:414](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L414)

Identifier shared by this transcript’s events.

#### role

```python
@property
def role() -> Role
```

Defined in: [src/strands/bidi/types/events.py:419](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L419)

The role of the speaker.

## BidiTranscriptDeltaEvent

```python
class BidiTranscriptDeltaEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:424](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L424)

Incremental transcription of user or assistant speech.

**Arguments**:

-   `delta` - The incremental transcript text.
-   `role` - Who is speaking (“user” or “assistant”).
-   `content_id` - Unique identifier shared by this transcript’s events.

#### \_\_init\_\_

```python
def __init__(delta: str, role: Role, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:433](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L433)

Initialize transcript delta event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:445](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L445)

Identifier shared by this transcript’s events.

#### delta

```python
@property
def delta() -> str
```

Defined in: [src/strands/bidi/types/events.py:450](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L450)

The incremental transcript text.

#### role

```python
@property
def role() -> Role
```

Defined in: [src/strands/bidi/types/events.py:455](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L455)

The role of the message sender.

## BidiTranscriptStopEvent

```python
class BidiTranscriptStopEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:460](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L460)

End of a transcript stream, before its completed block is emitted.

**Arguments**:

-   `role` - Who spoke (“user” or “assistant”).
-   `content_id` - Unique identifier shared by this transcript’s events.

#### \_\_init\_\_

```python
def __init__(role: Role, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:468](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L468)

Initialize transcript stop event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:479](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L479)

Identifier shared by this transcript’s events.

#### role

```python
@property
def role() -> Role
```

Defined in: [src/strands/bidi/types/events.py:484](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L484)

The role of the speaker.

## BidiTranscriptBlockEvent

```python
class BidiTranscriptBlockEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:489](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L489)

Complete transcript, emitted after its stop event by the agent.

**Arguments**:

-   `transcript` - The final transcript text.
-   `role` - Who spoke (“user” or “assistant”).
-   `content_id` - Unique identifier shared by this transcript’s events.

#### \_\_init\_\_

```python
def __init__(transcript: str, role: Role, content_id: str)
```

Defined in: [src/strands/bidi/types/events.py:498](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L498)

Initialize transcript block event.

#### content\_id

```python
@property
def content_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:510](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L510)

Identifier shared by this transcript’s events.

#### transcript

```python
@property
def transcript() -> str
```

Defined in: [src/strands/bidi/types/events.py:515](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L515)

The final transcript text.

#### role

```python
@property
def role() -> Role
```

Defined in: [src/strands/bidi/types/events.py:520](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L520)

The role of the speaker.

## BidiBargeInEvent

```python
class BidiBargeInEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:525](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L525)

Stop current response generation or playback while the session continues.

#### \_\_init\_\_

```python
def __init__() -> None
```

Defined in: [src/strands/bidi/types/events.py:528](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L528)

Initialize barge-in event.

## BidiResponseStopEvent

```python
class BidiResponseStopEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:533](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L533)

Response output ended. User transcription may still be pending.

**Arguments**:

-   `response_id` - ID of the response that ended (matches BidiResponseStartEvent).

#### \_\_init\_\_

```python
def __init__(response_id: str)
```

Defined in: [src/strands/bidi/types/events.py:540](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L540)

Initialize response stop event.

#### response\_id

```python
@property
def response_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:550](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L550)

Unique identifier for this response.

## ModalityUsage

```python
class ModalityUsage(dict)
```

Defined in: [src/strands/bidi/types/events.py:555](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L555)

Token usage for a specific modality.

**Attributes**:

-   `modality` - Type of content.
-   `input_tokens` - Tokens used for this modality’s input.
-   `output_tokens` - Tokens used for this modality’s output.

## BidiUsageEvent

```python
class BidiUsageEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:569](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L569)

Token usage event with modality breakdown for bidirectional streaming.

Tracks token consumption across different modalities (audio, text, images) during bidirectional streaming sessions.

**Arguments**:

-   `input_tokens` - Total tokens used for all input modalities.
-   `output_tokens` - Total tokens used for all output modalities.
-   `total_tokens` - Sum of input and output tokens.
-   `modality_details` - Optional list of token usage per modality.
-   `cache_read_input_tokens` - Optional tokens read from cache.
-   `cache_write_input_tokens` - Optional tokens written to cache.

#### \_\_init\_\_

```python
def __init__(input_tokens: int,
             output_tokens: int,
             total_tokens: int,
             modality_details: list[ModalityUsage] | None = None,
             cache_read_input_tokens: int | None = None,
             cache_write_input_tokens: int | None = None)
```

Defined in: [src/strands/bidi/types/events.py:584](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L584)

Initialize usage event.

#### input\_tokens

```python
@property
def input_tokens() -> int
```

Defined in: [src/strands/bidi/types/events.py:609](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L609)

Total tokens used for all input modalities.

#### output\_tokens

```python
@property
def output_tokens() -> int
```

Defined in: [src/strands/bidi/types/events.py:614](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L614)

Total tokens used for all output modalities.

#### total\_tokens

```python
@property
def total_tokens() -> int
```

Defined in: [src/strands/bidi/types/events.py:619](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L619)

Sum of input and output tokens.

#### modality\_details

```python
@property
def modality_details() -> list[ModalityUsage]
```

Defined in: [src/strands/bidi/types/events.py:624](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L624)

Optional list of token usage per modality.

#### cache\_read\_input\_tokens

```python
@property
def cache_read_input_tokens() -> int | None
```

Defined in: [src/strands/bidi/types/events.py:629](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L629)

Optional tokens read from cache.

#### cache\_write\_input\_tokens

```python
@property
def cache_write_input_tokens() -> int | None
```

Defined in: [src/strands/bidi/types/events.py:634](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L634)

Optional tokens written to cache.

## BidiToolUseBlocksEvent

```python
class BidiToolUseBlocksEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:639](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L639)

A complete group of tool calls requested by the model.

**Arguments**:

-   `tool_uses` - Tool calls to execute together.

#### \_\_init\_\_

```python
def __init__(tool_uses: list[ToolUse])
```

Defined in: [src/strands/bidi/types/events.py:646](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L646)

Initialize a tool-use group.

#### tool\_uses

```python
@property
def tool_uses() -> list[ToolUse]
```

Defined in: [src/strands/bidi/types/events.py:651](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L651)

Tool calls in provider order.

## BidiConnectionStopEvent

```python
class BidiConnectionStopEvent(TypedEvent)
```

Defined in: [src/strands/bidi/types/events.py:656](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L656)

Streaming connection closed.

**Arguments**:

-   `connection_id` - Unique identifier for this streaming connection (matches BidiConnectionStartEvent).
-   `reason` - Why the connection was closed. `"user_request"` after `agent.cancel()` takes effect.

#### \_\_init\_\_

```python
def __init__(connection_id: str, reason: Literal["user_request"])
```

Defined in: [src/strands/bidi/types/events.py:664](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L664)

Initialize connection stop event.

#### connection\_id

```python
@property
def connection_id() -> str
```

Defined in: [src/strands/bidi/types/events.py:679](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L679)

Unique identifier for this streaming connection.

#### reason

```python
@property
def reason() -> Literal["user_request"]
```

Defined in: [src/strands/bidi/types/events.py:684](https://github.com/strands-agents/harness-sdk/blob/main/strands-py/src/strands/bidi/types/events.py#L684)

Why the connection was closed.

#### BidiOutputEvent

Union of different bidi output event types.
Defined in: [src/models/bedrock.ts:178](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L178)

Configuration for Bedrock guardrails.

## See

[https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)

## Properties

### guardrailIdentifier

```ts
guardrailIdentifier: string;
```

Defined in: [src/models/bedrock.ts:180](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L180)

Guardrail identifier

---

### guardrailVersion

```ts
guardrailVersion: string;
```

Defined in: [src/models/bedrock.ts:183](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L183)

Guardrail version (e.g., “1”, “DRAFT”)

---

### trace?

```ts
optional trace?: "enabled" | "disabled" | "enabled_full";
```

Defined in: [src/models/bedrock.ts:186](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L186)

Trace mode for evaluation.

#### Default Value

```ts
'enabled'
```

---

### streamProcessingMode?

```ts
optional streamProcessingMode?: "sync" | "async";
```

Defined in: [src/models/bedrock.ts:189](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L189)

Stream processing mode

---

### redaction?

```ts
optional redaction?: BedrockGuardrailRedactionConfig;
```

Defined in: [src/models/bedrock.ts:192](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L192)

Redaction behavior when content is blocked

---

### guardLatestUserMessage?

```ts
optional guardLatestUserMessage?: boolean;
```

Defined in: [src/models/bedrock.ts:206](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L206)

Only evaluate the latest user message with guardrails. When true, wraps the latest user message’s text/image content in guardContent blocks. This can improve performance and reduce costs in multi-turn conversations.

#### Remarks

The implementation finds the last user message containing text or image content (not just the last message), ensuring correct behavior during tool execution cycles where toolResult messages may follow the user’s actual input.

#### Default Value

```ts
false
```
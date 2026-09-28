Defined in: [src/models/bedrock.ts:159](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L159)

Redaction configuration for Bedrock guardrails. Controls whether and how blocked content is replaced.

## Properties

### input?

```ts
optional input?: boolean;
```

Defined in: [src/models/bedrock.ts:161](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L161)

Redact input when blocked.

#### Default Value

```ts
true
```

---

### inputMessage?

```ts
optional inputMessage?: string;
```

Defined in: [src/models/bedrock.ts:164](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L164)

Replacement message for redacted input.

#### Default Value

```ts
'[User input redacted.]'
```

---

### output?

```ts
optional output?: boolean;
```

Defined in: [src/models/bedrock.ts:167](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L167)

Redact output when blocked.

#### Default Value

```ts
false
```

---

### outputMessage?

```ts
optional outputMessage?: string;
```

Defined in: [src/models/bedrock.ts:170](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/models/bedrock.ts#L170)

Replacement message for redacted output.

#### Default Value

```ts
'[Assistant output redacted.]'
```
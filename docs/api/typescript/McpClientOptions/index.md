Defined in: [src/mcp/client.ts:99](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L99)

Behavioral options shared by all MCP client configurations.

## Extends

-   `RuntimeConfig`

## Properties

### applicationName?

```ts
optional applicationName?: string;
```

Defined in: [src/mcp/client.ts:34](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L34)

#### Inherited from

```ts
RuntimeConfig.applicationName
```

---

### applicationVersion?

```ts
optional applicationVersion?: string;
```

Defined in: [src/mcp/client.ts:35](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L35)

#### Inherited from

```ts
RuntimeConfig.applicationVersion
```

---

### disableMcpInstrumentation?

```ts
optional disableMcpInstrumentation?: boolean;
```

Defined in: [src/mcp/client.ts:101](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L101)

Disable OpenTelemetry MCP instrumentation.

---

### prefix?

```ts
optional prefix?: string;
```

Defined in: [src/mcp/client.ts:104](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L104)

Prefix for agent-facing tool names, applied as `<prefix>_<toolName>`.

---

### toolFilters?

```ts
optional toolFilters?: McpToolFilters;
```

Defined in: [src/mcp/client.ts:107](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L107)

Filters controlling which tools this client exposes.

---

### tasksConfig?

```ts
optional tasksConfig?: TasksConfig;
```

Defined in: [src/mcp/client.ts:115](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L115)

Configuration for task-augmented tool execution (experimental).

Temporarily unavailable while task support is rebuilt on the MCP tasks extension ([https://github.com/strands-agents/harness-sdk/issues/1659](https://github.com/strands-agents/harness-sdk/issues/1659)). When set, `callTool` throws.

---

### elicitationCallback?

```ts
optional elicitationCallback?: ElicitationCallback;
```

Defined in: [src/mcp/client.ts:122](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L122)

Callback to handle server-initiated elicitation requests. When provided, the client advertises elicitation support (form + url modes) and routes incoming elicitation requests to this callback.

---

### continueOnError?

```ts
optional continueOnError?: boolean;
```

Defined in: [src/mcp/client.ts:125](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L125)

When true, connection failures and overlong prefixed names during tool listing are skipped with warnings.

---

### logHandler?

```ts
optional logHandler?: (params) => void;
```

Defined in: [src/mcp/client.ts:128](https://github.com/strands-agents/harness-sdk/blob/main/strands-ts/src/mcp/client.ts#L128)

Called when the server emits a log message. Defaults to routing through the Strands logger.

#### Parameters

| Parameter | Type |
| --- | --- |
| `params` | `LoggingMessageNotificationParams` |

#### Returns

`void`
---
title: "OpenTelemetry in Go with Gin framework"
date: 2023-10-22T08:34:44+02:00
---

On [OpenTelemetry](https://opentelemetry.io/) website you can read definition what is OpenTelemetry:

> OpenTelemetry is a collection of APIs, SDKs, and tools. Use it to instrument, generate, collect, and export telemetry data (metrics, logs, and traces) to help you analyze your software’s performance and behavior.

For me is not vendor free way how instrument your code. In Go we don't have in Oct 2023 full support for traces, metrics and logs and SDK are not 1.x but still you can start using it at least for traces and metrics.

I'm working on project that I need make traces from Python to my Go API and we want all stored in logs and traces. I started with [Gin framework](https://gin-gonic.com/) that is my default simple framework for APIs. There are many good alternatives but I like use this one.

It's great if you Google `otel gin` you get [otelgin](https://pkg.go.dev/go.opentelemetry.io/contrib/instrumentation/github.com/gin-gonic/gin/otelgin) library for instrument your code.

You find good [example](https://github.com/open-telemetry/opentelemetry-go-contrib/blob/instrumentation/github.com/gin-gonic/gin/otelgin/example/v0.45.0/instrumentation/github.com/gin-gonic/gin/otelgin/example/server.go) where you see how to use it.

```go
r.Use(otelgin.Middleware("my-server"))
```

You add new line to your code with `otelgin` middleware and you are done! Or not?

## Context Propagation

If you need connect traces across services you need setup propagation and you find in every example of your tracer.

```go
otel.SetTextMapPropagator(propagation.NewCompositeTextMapPropagator(propagation.TraceContext{}, propagation.Baggage{}))
```

I will focus on `propagation.TraceContext{}`. [TraceContext](https://www.w3.org/TR/trace-context/) is W3C spec how to pass information about TraceId, SpanId across services in Header `traceparent`. All works for you if you call another system. But doesn't work for me.

I tested with this command:

```
curl -H "traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01" http://localhost:8080
```

and I can't find in my correct traceId in my logs.

You need configure `otelgin` middleware with the propagator too. Not just tracer.

```go
otelginOption := otelgin.WithPropagators(propagation.TraceContext{})
r.Use(otelgin.Middleware("my-server", otelginOption))
```

after that all start working.


## How to debug it?

I use two ways how to debug.

First I'm using [otel-desktorp-viewer](https://github.com/CtrlSpice/otel-desktop-viewer) for debug traces locally without need to run observability server.

Second, logging. I'm using `log/slog` library for logging (available in Go 1.21+). I make simple logging middleware for Gin that log traceId and spanId.

```go
func GinSlogMiddleware(logger *slog.Logger) gin.HandlerFunc {
	return func(c *gin.Context) {
		start := time.Now().UTC()
		path := c.Request.URL.Path
		query := c.Request.URL.RawQuery
		c.Next()
		ctx := c.Request.Context()
		end := time.Now().UTC()
		latency := end.Sub(start)
		fields := slog.Group("http",
			slog.Int("status", c.Writer.Status()),
			slog.String("method", c.Request.Method),
			slog.String("path", path),
			slog.String("query", query),
			slog.String("ip", c.ClientIP()),
			slog.String("user-agent", c.Request.UserAgent()),
			slog.Duration("latency", latency),
		)
		if len(c.Errors) > 0 {
			for _, e := range c.Errors.Errors() {
				logger.ErrorContext(ctx, e, fields)
			}
		} else {
			logger.InfoContext(ctx, path, fields)
		}
	}
}
```

You can see that key is logging with Context that contains information about traces. Context handler will log information about traces.

```go
type ContextHandler struct {
	slog.Handler
}

func (h ContextHandler) Handle(ctx context.Context, r slog.Record) error {
	r.AddAttrs(h.addTraceFromContext(ctx)...)
	return h.Handler.Handle(ctx, r)
}

func (h ContextHandler) addTraceFromContext(ctx context.Context) (as []slog.Attr) {
	if ctx == nil {
		return
	}
	span := trace.SpanContextFromContext(ctx)
	traceID := span.TraceID().String()
	spanID := span.SpanID().String()
	traceGroup := slog.Group("trace", slog.String("id", traceID))
	spanGroup := slog.Group("span", slog.String("id", spanID))
	as = append(as, traceGroup)
	as = append(as, spanGroup)
	return
}
```

and my logger init looks as this:

```go
func InitLog() *slog.Logger {
  jsonHandler := slog.NewJSONHandler(os.Stdout)
  ctxHandler := ContextHandler{jsonHandler}
  return slog.New(ctxHandler)
}
```

You can make custom JSONHandler if you need override logging format for example for support [Elastic ECS](https://www.elastic.co/guide/en/ecs/current/index.html) if your destination is Elastic stack.

Resources and libraries supporting `log/slog` aren't always ready, but I expected that will be resolved in the future.

---
title: "Workflow Engines - Temporal.io"
date: 2023-03-10T10:51:34+01:00
---

Nevím kolik z vás se setkalo s pojmem Workflow Engines. Ve wikipedii se dočtete něco jako:

> Workflow engine je softwarová aplikace, která řídí podnikové procesy. Je klíčovou součástí workflow technologie  a obvykle využívá databázový server.

> Workflow engines mají především tři funkce:

> - Ověřování aktuálního stavu procesu: Ověření, zda je platné provedení úlohy vzhledem k aktuálnímu stavu.
> - Určení oprávnění uživatelů: Kontrola, zda je aktuální uživatel oprávněn provést úlohu.
> - Spuštění skriptu podmínek: Po absolvování předchozích dvou kroků provede engine pracovního postupu úlohu, a pokud se provedení úspěšně dokončí, vrátí úspěch, pokud ne, ohlásí chybu spuštění a vrátí změnu zpět.

V článku se budu zabývat specifickou skupinou nástrojů, které se združují pod pojmem Durable Execution (DE). Jejich hostorie je celkem dlouhá a postupně vznikají evolučně nové a nové nástroje.

```mermaid
flowchart TD
    A[Amazon SWF]
    A-->B[Azure/durable task]
    A-->C[infinitic]
    B-->D[Azure Durable Functions]
    B-->E[Cadence]
    A-->E
    E-->F[Temporal.io]
    click A href "https://docs.aws.amazon.com/amazonswf/latest/developerguide/swf-welcome.html" "Amazon SWF"
    click C href "https://docs.infinitic.io/" "infinitic"
    click D href "https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp-inproc" "Azure Durable Function"
    click E href "https://cadenceworkflow.io/" "Cadence"
    click F href "https://temporal.io/" "Temporal.io"
```

## Temporal.io

Já se zaměřím na nástoj Temporal.io, který je nejnovější, ale neznamená to, že vznikl před pár lety, [jeho tvůrci už právě pracovali na Amazon SWF a potom v Uberu](https://temporal.io/about) a v Microsoftu. V roce 2019 založili startup Temporal.io, kde navázali na práci v Cadence a Temporal je fork Cadence.

Maxim Fateev (Co-Founder & CEO Temporal.io) měl skvělou přednášku [Designing a Workflow Engine from First Principles](https://www.youtube.com/watch?v=t524U9CixZ0) ze které také pochazí následující diagram.

```mermaid
%% https://mermaid.js.org/syntax/sequenceDiagram.html
sequenceDiagram
    rect rgb(220, 220, 220)
    note right of Client: ACID
        actor Client;
        participant Engine;
        participant WorkflowState;
        participant Timers;
        participant TaskQueues;
        participant WorkflowDefinition;
        Client->>Engine: start
        activate Engine
        Engine->>WorkflowState: create
        WorkflowState-->>Engine: ack
        Engine-->>Timers: add workflow timeout
        Timers-->>Engine: ack
        Engine-->>TaskQueues: add workflow task
        TaskQueues-->>Engine: ack
        TaskQueues-->>WorkflowDefinition: send
        deactivate Engine
    end
    rect rgb(220, 220, 220)
    note right of Engine: ACID
        participant Engine;
        participant WorkflowState;
        participant Timers;
        participant TaskQueues;
        participant WorkflowDefinition;

        activate Engine
        WorkflowDefinition->>Engine: execute Task 1
        Engine->>WorkflowState: update
        WorkflowState-->>Engine: ack
        Engine->>TaskQueues: add Task 1
        TaskQueues-->>Engine: ack
        Engine->>Timers: add Task 1 timeout
        Timers-->>Engine: ack
        Engine-->>WorkflowDefinition: send
        deactivate Engine
    end
```

V tom diagramu je dobře vidět rozdíl Temporalu oproti klasickým frontám. Máte to [ACID](https://en.wikipedia.org/wiki/ACID) transakce, které vám zaručí spolehlivost při změnách a to i když část infrastruktury má potíže, je snaha o to, aby veškerý stav byl bezpečně uložený v databázi. Když k tomu přidáte pravidla jak správně psát workflows tak by jste neměli narazit z mé zkušenosti na problémy. Důležité je aby Activities byli [idempotetní](https://docs.temporal.io/activities#idempotency) bez vedlejších effektů, je to stejné jako když píšete skript na infrastrukturu a taky nechcete, aby script vytvářel nově instance pokud ho pustíte znovu.

## Praktický příklad

Dost teorie, praktický příklad jak se píše workflow a activity. Mám malý projekt [Hugo Netlify Autoupdater](https://github.com/abtris/hugo-netlify-autoupdater), který funguje jako dependBot pro specifický účel update verze [Hugo](https://gohugo.io/) ve všech mých projektech, které Hugo používají. Vzal jsem projekt a přepsal ho pro Temporal, kód najdete v [repozitáři](https://github.com/abtris/temporalio-example).

Vlastní workflow je super jednoduché. Nejdříve zjistíme jaká je poslední releasu Huga a zapíšeme si ji do proměné typu HugoResult.

```go
var result HugoResult
 err := workflow.ExecuteActivity(ctx, CheckHugoReleaseVersion, sourceRepo).Get(ctx, &result)
```

Potom načteme konfiguraci z TOML souboru, kde jsou jednotlivé projekty, které potřebujeme udržovat. Pro každé repository zkontrolujeme jaká verze je aktuálně nasazená.

```go
var deployedResult DeployResult
err = workflow.ExecuteActivity(ctx, CheckCurrentDeployedVersion, repository).Get(ctx, &deployedResult)
```

Nakonec pokud je nová verze k dispozici tak provedem vytvoření příslušného pull requestu.

```go
  if deployedResult.deployVersion != result.hugoVersion {
   var resultChild bool
   err = workflow.ExecuteActivity(ctx, DeployNewVersion, result, deployedResult, repository).Get(ctx, &resultChild)
   if err != nil {
    return finalResult, err
   }
   finalResult += 1
  }
```

Celé workflow je v souboru [workflow.go](https://github.com/abtris/temporalio-example/blob/main/workflow.go) a všechny activity jsou v [activity.go](https://github.com/abtris/temporalio-example/blob/main/activity.go). Tohle je hodně jednoduchý příklad, tak jsem se moc nezatěžoval s tím to rozdělit nějak více. Activity jsou jednoduché Go funkce, které jsou spuštěny workerem, musíte se postarat, aby byli všechny workery správně nakonfigurovány. Vlastní [worker](https://github.com/abtris/temporalio-example/blob/main/worker/main.go) si musí zaregistrovat aktivity, které podporuje a ty bude zpracovávat. Workflow se nastartuje pomocí [startovací funkce](https://github.com/abtris/temporalio-example/blob/main/start/main.go). Neměl jsem k dispozici Temporal Cloud a celý temporal jsem běžel v [docker compose](https://docs.temporal.io/kb/all-the-ways-to-run-a-cluster#docker--docker-compose) u sebe na laptopu.

## Užitečné nástroje na práci s Temporal.io

- [TemporalLite](https://github.com/temporalio/temporalite) - lokální testování, jeden soubor, který vám pustí celý Temporal
- [Context switch pro Temporal, podobné kubectx](https://github.com/jlegrone/tctx)
- [Limity Temporal.io platformy](https://docs.temporal.io/kb/temporal-platform-limits-sheet)

## Doporučené články v angličtině

- [Mikhail Shilkov - A Practical Approach to Temporal Architecture](https://mikhail.io/2020/10/practical-approach-to-temporal-architecture)
- [Alameddin Çelik - What is Temporal.io?](https://alameddinc.medium.com/what-is-temporal-io-3e7356fe7c7d)
- [Awesome Temporal](https://github.com/temporalio/awesome-temporal)
- [Datadog talk about Temporal](https://www.youtube.com/watch?v=LxgkAoTSI8Q)
- [Proposals about Temporal](https://github.com/temporalio/proposals)
- [Chris Gillum - Common Pitfalls with Durable Execution Frameworks, like Durable Functions or Temporal](https://medium.com/@cgillum/common-pitfalls-with-durable-execution-frameworks-like-durable-functions-or-temporal-eaf635d4a8bb)
- [Alexandra Li - Intern Summer Spotlight: Temporal Replay through CircleCI](https://medium.com/convoy-tech/intern-summer-spotlight-temporal-replay-through-circleci-870d9246c2e8)
- [How to explain Temporal](https://docs.temporal.io/kb/how-to-explain-temporal)

## Shrnutí

Temporal mi příjde jako skvělý nástroj, který se hodí do portfolia vývojáře, rozhodně dosáhnete lepší vývsledky než když používáte klasické fronty ([Sidekiq](https://sidekiq.org/), [Faktory](https://github.com/contribsys/faktory)) a pokud si o tom chcete dozvědět více, přijďte na další [Go meetup v dubnu](https://www.meetup.com/prague-golang-meetup/events/292126460/), kde Václav Brož bude mluvit o svých zkušenostech s tímto systémem.

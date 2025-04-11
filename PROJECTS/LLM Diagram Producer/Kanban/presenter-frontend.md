---

kanban-plugin: board

---

## Bugs

- [ ] **APP**
	
	==Problem==: Page DOM when navigate got blank after initialised.
- [ ] **INFRASTRUCTURE**
	
	==Problem==: Events got created even they got derived from error execution from first-party/microservice
	
	<mark style="background: #BBFABBA6;">Expect</mark>: Events can't be create if the events got error from derived on microservice
- [ ] **INFRASTRUCTURE**
	
	<mark style="background: #FFF3A3A6;">**Problem**</mark> Concurrency bottleneck on CQRS when **Service** tried to create event and then automatically fetch the event that just had created 
	
	<mark style="background:  #FFB8EBA6;">Reproduce</mark> 
	1. Created event
	2. Fetch event that just was created
	- Got: 200 OK but data not found
	- Expect:  200 OK and data should be found if event was really created on event DB
	
	<mark style="background: #BBFABBA6;">Expect</mark>: Infra should handle the concurrent correctly on CQRS pattern
	
	FIx: 
	- Creating `localEventState` that acts like state event handler for CQRS (2)
	- Like


## Done

- [ ] **INFRASTRUCTURE**
	
	<mark style="background: #FFF3A3A6;">Problem</mark>: EventBus (Kafka) do not subscribe from event store on both `@ai-ctx/presenter-backend` and `@ai-ctx/analyzer` 
	
	<mark style="background: #BBFABBA6;">Execpt</mark>: EventBus should be subscribe following the list from `BUS_EVENT_LIST_SUBSCRIBE`
	
	<mark style="background: #FFB86CA6;">Cause:</mark>
	- The Kafka's offset tracking is not sync or the Consumer client "consumed" them. Which it leads to not subscribe those consumed events
	- Consumer Group Conflict, happend when services using the `same consumer group ID`. So it cause the conflict of partition assignment among the event
	- Kafka do not consume event from the `beginning`, by default the Kafka would starts consume when messages published **after** they joined. So it mean that they were simply waiting for new messages and ignoring all historical messages.
	
	<mark style="background: #ADCCFFA6;">Fix:</mark>: 
	- Set "fromBeginning: true" on `kafkaConsumer.subscribe({ topic, fromBeginning: true });` to tell the Kakfa  to start consuming from the beginning.


## Features





%% kanban:settings
```
{"kanban-plugin":"board","list-collapse":[false,false,false]}
```
%%
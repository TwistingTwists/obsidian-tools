Notify = 
- semaphore with 0 permits 
- multiple callers can notify mutliple times => the notifiee acts on it only once and the notification is consumed.
- any new callee (notifee) => they need to be sent notification again. because the previous one was 'consumed'. More like pubsub

CancellationToken = 
- When need to propagate cancellation in hierarchial strcuture (processes in parent child relations)
- once cancelled =>  any new callee joins =>  cancellation remains valid and gets cancelled. => the cancelled state is 'sticky'


So, CancellationToken = Add sticky state + loop (`AtomicBool shutdown` + `Notify`)
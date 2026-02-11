As a senior Java architect, analyze impact of modifying the `payment-central` module in an enterprise multi-module Spring Boot 3 (JDK 17\) microservices project.

Project structure:

* policy-service

* billing-service

* payment-central

* claims-service

* notification-service

Payment-central:

* Exposes REST APIs: issueDirectPayment, autoPay, lockboxPayment

* Publishes Kafka events: PaymentCompletedEvent, PaymentFailedEvent

* DB tables: payment\_txn, autopay\_schedule, lockbox\_batch

* Used by billing-service via Feign

Proposed change:

* Introduce new payment status "PARTIALLY\_SETTLED"

Provide:

* Dependent services impact

* API contract changes

* Database migration impact

* Event schema changes

* Backward compatibility risks

* Deployment sequencing strategy

* Rollback risks


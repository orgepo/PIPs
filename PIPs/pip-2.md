---
pip: 2
title: Lock Time Transactions
author: Mostafa Sedaghat Joo (@b00f)
status: Accepted
type: Interface
created: 2023-08-23
---

## Abstract

This document proposes to add a `LockTime` field to the Pactus transaction structure to
support scheduled or lock-time transactions.
The `LockTime` field specifies the block number at which the transaction can be unlocked.

## Motivation

Scheduled transactions are a type of transaction set to be executed in the future.
They are commonly referred to as "lock-time" transactions.
The current transaction structure in Pactus does not provide support for such scheduled transactions.

## Specification

To support lock-time transactions, a new field is added to the transaction structure: `LockTime`.

By introducing the `LockTime` field, users can specify a block number at which a lock-time transaction can be unlocked and executed.
With each block in Pactus having a fixed time interval of 10 seconds, users can ensure that their transactions are executed
at nearly precise times in the future.
This field is similar to `nLockTime` [^1] in Bitcoin.
However, in Pactus, it is mandatory for all transactions, and it only accepts block numbers as input.

The `LockTime` and `Stamp` fields should be set as follows:

- For scheduled transactions, the `Stamp` field should be set to zero, and the `LockTime` should be set to a block number in future.
- For stamped transactions, the `LockTime` value should be same as the block number referenced by the Stamp.

While users have the option to set the `Stamp` field to zero for all transactions,
there is a strong motivation for them to avoid doing so.
Stamped transactions provide resistance against long-range attack [^2].

Both locked-time and stamped transactions have a limited lifespan,
and they become invalid once the Transaction-To-Live (TTL) interval expires.

## References:

[^1]: [https://en.bitcoin.it/wiki/Protocol_documentation#tx](https://en.bitcoin.it/wiki/Protocol_documentation#tx)
[^2]: [https://pactus.org/learn/transaction/stamping/#long-range-attack](https://pactus.org/learn/transaction/stamping/#long-range-attack)

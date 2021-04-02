---
description: >-
  A basic understanding of options is critical to interact with our code
  seamlessly.
---

# What are options?

Option contracts represent a **right** but **not an obligation** to the option **buyer**. On the other hand, the option sellers commit to allowing the buyers to exercise the options if they want to.

One could think of it as a type of financial instrument with specific conditions, such as _"if this then that."_ It represents an action if a particular situation is met.

{% embed url="https://www.youtube.com/watch?v=VJgHkAqohbU" caption="\"Options Explained\" by The Plain Bagel " %}

Options are a well-known kind of derivative in traditional finance. They represent a commitment \(of the seller\) and an opportunity \(of the buyer\) to buy or sell - depending on the contract they hold - the underlying asset upon expiration. The seller issues the contract, and, unlike futures, it represents a right, **but not an obligation**, of the buyer to exercise the contract terms if it wants to.

The concept of a contract that allows the buyer to hold a right but not an obligation is the most basic and inherent feature of every options contract. Besides that, there are different types of options, types of exercising rules, and types of settlement that can be tweaked to get your best option.

## Options types and flavors

There are two types of options: **calls** and **puts**.

`A` **`call`** `option allows the holder to` **`buy`** `the underlying asset at the strike price.`

`A` **`put`** `option allows the holder to` **`sell`** `the underlying asset at the strike price`.

{% embed url="https://www.youtube.com/watch?v=uQLMSU2NNlk" caption="\"Call vc Put Options Basics\" by Option Alpha" %}

Another vital characteristic of options regards the rules to exercise the contract. Options can be either **European** or **American\*** options. An American option is a "continuous time instrument" because it allows the buyer to exercise it **at any moment until expiry**. European options are a "point in time instrument" since they will enable the buyer to exercise **only when the expiration is reached**. You can find both **American** and **European** options in our protocol.

{% hint style="info" %}
**\***Although the code is ready to receive either American or European options, it is **not recommended** to implement American options using the current pricing model applied on the Options AMM. Find more about this in the Pricing section.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=Yu27K3KB2Wo" caption="\"American vs. European Style Options\" by Option Alpha" %}

An option can be exercised in two ways: **cash** or **physical settlement**. Cash settlement assumes that only the difference between the counterparts should be offset, which means that the contract's margin to be valid could be smaller. On the other hand, physical settlement assumes that all the collateral or margin held in custody will be exercised.

{% embed url="https://www.youtube.com/watch?v=Udl9Q5yrDlY" caption="\"Cash vs. Physical Settlement\" by Option Alpha" %}


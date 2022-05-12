# Ticks and Ranges

To implement custom liquidity provision, the space of possible prices is demarcated by discrete *ticks*.

Liquidity providers can provide liquidity in a range between any two ticks (which need not be
adjacent). Each range can be specified as a pair of signed integer tick indices:
a lower tick and an upper tick). 

Ticks represent prices at which the virtual liquidity of the contract can change. We will assume that prices are always expressed as the price of one of the tokens —called `token0`— in terms of the other token — called `token1`. 

The assignment of the two tokens to `token0` and `token1` is arbitrary and does not affect the logic of the contract (other than through possible rounding errors).

Conceptually, there is a tick at every price 𝑝 that is an integer power of 1.0001. Identifying ticks by an integer index 𝑖, the price at each is given by:

$$𝑝(𝑖) = 1.0001^𝑖$$

This has the desirable property of each tick being a .01% (1 basis point) price movement away from each of its neighboring ticks. For technical reasons, pools actually track ticks at every square root price that is an integer power of $$\sqrt{1.0001}$$ . Consider the above equation, transformed into square root price space:


$$\sqrt{𝑝}(𝑖) = \sqrt{1.0001}^𝑖 = 1.0001^{𝑖/2}$$

As an example:

-  $$\sqrt{p}(0)$$ — the square root price at tick 0 — is equal to 1, 
-  $$\sqrt{p}(1)$$ is $$\sqrt{1.0001}$$ ≈ 1.00005, and 
-  $$\sqrt{p}(-1)$$ is $$\frac{1}{\sqrt{1.0001}}$$ ≈ 0.99995.

When liquidity is added to a range, if one or both of the ticks is not already used as a bound in an existing position, that tick is *initialized*.

Not every tick can be initialized. The pool is instantiated with a parameter, tickSpacing; only ticks with indexes that are divisible by tickSpacing can be initialized. For example, if tickSpacing is 2, then only even ticks (...-4, -2, 0, 2, 4...) can be initialized. 

Small choices for tickSpacing allow tighter and more precise ranges, but may cause swaps to be more steps-intensive (since each initialized tick that a swap crosses imposes a step cost on the swapper).

Whenever the price crosses an initialized tick, virtual liquidity is kicked in or out. The step cost of an initialized tick crossing is constant, and is not dependent on the number of positions being kicked in or out at that tick.

Ensuring that the right amount of liquidity is kicked in and out of the pool when ticks are crossed, and ensuring that each position earns its proportional share of the fees that were accrued while it was within range, requires some accounting within the pool.

The pool contract uses storage variables to track state at a global (per-pool) level, at a *per-tick* level, and at a *per-position* level.
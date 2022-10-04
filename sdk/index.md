# Convexus SDKs

Convexus provides multiple SDKs for building transactions in order to communicate with the Convexus smart contracts.

There are currently 2 existing SDKs : one in Javascript, another one in Python.

Both SDKs are extremely similar. The classes and methods names are the same, and output the same result given the same inputs. The only exceptions are coming from high precision maths operations such as square roots, but the result remains significantly the same.

The Convexus SDK is separate from the Convexus protocol. It is designed to assist developers when interacting with the protocol in any environment that can execute JavaScript or Python, such as websites, node scripts, or server backends. With the SDK, you can manipulate data using libraries that assist with several needs, such as data modeling and protection from rounding errors.

# Alpha software

The latest version of the SDK is used in production in the Convexus Interface,
but it is considered Alpha software and may contain bugs or change significantly between patch versions.
If you have questions about how to use the SDK, please reach out in the `#developper` channel of [the Discord](https://discord.convexus.net).
Pull requests welcome!

# Convexus SDK (Javascript)

- [**SDK JS Github Repo**](https://github.com/Convexus-Protocol/convexus-sdk-js)
- [**SDK NPM Package**](https://www.npmjs.com/package/@convexus/sdk)

[![npm version](https://img.shields.io/npm/v/@convexus/sdk/latest.svg)](https://www.npmjs.com/package/@convexus/sdk/v/latest)
[![npm bundle size (scoped version)](https://img.shields.io/bundlephobia/minzip/@convexus/sdk/latest.svg)](https://bundlephobia.com/result?p=@convexus/sdk@latest)

# Convexus SDK (Python)

- [**SDK Python Github Repo**](https://github.com/Convexus-Protocol/convexus-sdk-py)
- [**SDK Pypi Package**](https://pypi.org/project/convexus/)

[![PyPI - latest](https://img.shields.io/pypi/v/convexus?label=latest&logo=pypi)](https://pypi.org/project/convexus)
[![PyPI - Python](https://img.shields.io/pypi/pyversions/convexus?logo=pypi)](https://pypi.org/project/convexus)

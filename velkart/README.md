# Velkart Documentation
[![Build Status](https://travis-ci.org/indiehd/velkart.svg?branch=master)](https://travis-ci.org/indiehd/velkart) [![codecov](https://codecov.io/gh/indieHD/velkart/branch/master/graph/badge.svg)](https://codecov.io/gh/indieHD/velkart)

"Velkart" is a portmanteau of "Laravel" and "Kart". At its core, Velkart is an
extensible shopping cart, order management, and payment processing system built
upon clean-coding principles and designed for Laravel.

This documentation describes *our particular implementation* of the Velkart
package, given our specific use-case. Our primary API, which resides in the
`indiehd/web-api` repository, depends on Velkart, and this documentation
describes that relationship, rather than a more "vanilla" e-commerce
implementation for which Velkart could certainly be used. As such, this
documentation contains implementation-specific references throughout. 

## Repository Duties

- Shopping cart
- Order management
- Payment processing

## Qualifications

- PHP
- Laravel Framework
- Strong understanding of S.O.L.I.D. Principles
- Familiarity with PHPUnit testing

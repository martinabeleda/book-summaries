# Clean Code

[Goodreads](https://www.goodreads.com/book/show/3735293-clean-code)

## Preface

pxix. Honesty in small things is no small thing

pxxii. Quality is the result of a million selfless acts of care - not just of any great method that descends from the heavens.
That these acts are simple doesn’t mean they are simplistic, and it hardly means that they are easy. They are nonetheless the
fabric of greatness and, more so, of beauty, in any human endeavour. To ignore them is not yet to be fully human.

## Classes

p138. **Single Responsibility Principle (SRP):** A class or module should have one, and only one reason to change.

p149. **Open-closed Principle (OCP):** Classes should be open for extension but closed for modification. We want to structure
our systems so that we muck around with as little as possible when we update them with new or changed features. In an ideal
system, we incorporate new features by extending the system, not by making modifications with existing code.

p149. Isolating for change. A client class depending on concrete details is at risk when those details change. We can introduce
interfaces and abstract classes to help isolate the impact of those details.

P150. **Dependency Inversion Principle (DIP):** our classes should depend on abstractions not on concrete details.


+++
subtitle = ""
title = "An intro to Hypothesis"
bigimg = ""
date ="2016-08-08T12:32:17+02:00"

+++

I have recently listened to the [Episode 67](https://talkpython.fm/episodes/show/67/property-based-testing-with-hypothesis) of the TalkPython podcast, which is an interview with [David MacIver](http://www.drmaciver.com/), the primary author of the [Hypothesis](https://github.com/HypothesisWorks/hypothesis-python) Python library.

The concept of [property based testing](https://github.com/HypothesisWorks/hypothesis-python) immediately caught my attention so I decided to give it try.

<!-- TEASER_END -->

The rough idea behind property based testing is focusing not on specific examples of inputs and otputs but checking the properties of the expected results. It has many concepts in common with functional programming. And, as with other functional programming tools and constructions, it can be used in Python although your code is not entirely functional (of not functional at all).

For example, with Hypothesis is quite easy to create [fuzzy testing](http://hypothesis.works/articles/getting-started-with-hypothesis/). That means that the test input values are generated at runtime, instead of statically coded.

Let's say that we have a module `money.py`  with a function to convert between two currencies:

```python
rates_table = {
        "USD": 102.42,
        "EUR": 113.54,
        "YEN": 1.0,
        "ELB": -1.23,
        }

def convert_currency(previous_currency, new_currency, amount):
    if amount < 0:
        return ValueError("Amount must be a positive value")
    return amount * rates_table.get(previous_currency) / rates_table.get(new_currency)
```

It is a toy example, but there is an easy-to-spot bug there. We are going to test it with Hypothesis:

```python
from hypothesis import strategies as st, given
from money import rates_table, convert_currency

@given(st.sampled_from(rates_table.keys()), st.sampled_from(rates_table.keys()), st.floats(min_value=0))
def test_convert_currency(previous_currency, new_currency, amount):
    r = convert_currency(previous_currency, new_currency, amount)
    assert isinstance(r, float)
    assert r >= 0
```

Hypothesis comes with a decorator named `given` that is used to inject input values to the test function according to some strategies. The values are received as arguments, in a similar and compatible way as [pytest fixtures](http://doc.pytest.org/en/latest/fixture.html). In this case, we are using a `sampled_from` strategy to select among the different currencies and a `floats` strategy for the input amount, parameterized to generate only positive values. As a first strategy, we will not test the conversion but we will check just two properties of the result of the conversion: that the value is a floating number and positive.


Running the test we may have this result:

```text
previous_currency = 'YEN', new_currency = 'ELB', amount = 5e-324

    @given(st.sampled_from(rates_table.keys()), st.sampled_from(rates_table.keys()), st.floats(min_value=0))
    def test_convert_currency(previous_currency, new_currency, amount):
        r = convert_currency(previous_currency, new_currency, amount)
        assert isinstance(r, float)
>       assert r >= 0
E       assert -5e-324 >= 0

test_money.py:9: AssertionError
------------------------------------------------------------------------------------------------------------- Hypothesis --------------------------------------------------------------------------------------------------------------
Falsifying example: test_convert_currency(previous_currency='YEN', new_currency='ELB', amount=5e-324)
```

We can see that the test has failed for the input values 'YEN', 'ELB' and 5e-324. If we go through the code, we can spot that the ELB currency has a change rate which is negative. The generative nature of fuzzy testing allows us to create a compact test covering every input value and uncover bugs that otherwise could be skipped when creating classic-style unit tests with examples. Hypothesis shrinks the results to present the simplest test case failing and also stores the example in an internal cache-like [database](https://hypothesis.readthedocs.io/en/latest/database.html) so that re-runs of the tests will start by retrying the examples that failed the last time.

There are many other interesting features in Hypothesis that I have not used in the example but are worth knowing:

- The [assume](https://hypothesis.readthedocs.io/en/latest/details.html#making-assumptions) function to discard test cases that are not valid
- The `example` decorator to specify examples that you always want to run
- Many [strategies](https://hypothesis.readthedocs.io/en/latest/data.html) for generating lists, dicts, numbers, strings, nested structures, random choices...  Hint: you can test your strategies with the functions `example` and `find`
- [Extra packages](https://hypothesis.readthedocs.io/en/latest/extras.html) for integration with Django, NumPy and datetime
- Built-in [health checks](https://hypothesis.readthedocs.io/en/latest/healthchecks.html) to detect if your strategies are wrong
- [Settings](https://hypothesis.readthedocs.io/en/latest/settings.html) that you can change, including the number of examples to generate

I recommend that you start easy reading the [introductory articles](http://hypothesis.works/articles/intro/) and incorporating some fuzzy tests in your code.

Happy hacking!

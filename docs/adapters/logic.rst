==============
Logic Adapters
==============

Logic adapters determine the logic for how ChatterBot selects a responses to a given input statement.

The logic adapter that your bot uses can be specified by setting the `logic_adapters` parameter to the import path of the logic adapter you want to use.

It is possible to enter any number of logic adapters for your bot to use.
If multiple adapters are used, then the bot will return the response with
the highest calculated confidence value. If multiple adapters return the
same confidence, then the adapter that is entered into the list first will
take priority.

.. code-block:: python

   chatbot = ChatBot(
       "My ChatterBot",
       logic_adapters=[
           "chatterbot.adapters.logic.ClosestMatchAdapter"
       ]
   )

Closest Match Adapter
---------------------

.. autofunction:: chatterbot.adapters.logic.ClosestMatchAdapter

The `ClosestMatchAdapter` selects a response based on the closest know match to a given statement.

How it works
++++++++++++

The closest match algorithm determines the similarity between the input statement and a set of known statements. For example, there is a 65% similarity between the statements _"where is the post office?"_ and _"looking for the post office"_. The closest match algorithm selects the highest matching known statements and returns a response based on that selection.

Closest Meaning Adapter
-----------------------

.. autofunction:: chatterbot.adapters.logic.ClosestMeaningAdapter

The `ClosestMeaningAdapter` selects a response based on how closely two statements match each other based on the closeness of the synsets of each word in the word matrix formed by both sentences.

How it works
++++++++++++

The closest meaning algorithm uses the [wordnet](http://www.nltk.org/howto/wordnet.html) functionality of [nltk](www.nltk.org) to determine the similarity of two statements based on the path similarity between each token of each statement. This is essentially an evaluation of the closeness of synonyms. The statement that has the closest path similarity of synsets to the input statement is returned.

Approximate Sentence Match Adapter
---------------------

.. autofunction:: chatterbot.adapters.logic.ApproximateSentenceMatchAdapter

The `ApproximateSentenceMatchAdapter` calculates a Jaccard index and give result to a given statement.

How it works
++++++++++++

The Jaccard index is composed of a numerator and denominator.
  In the numerator, we count the number of items that are shared between the sets.
  In the denominator, we count the total number of items across both sets.
  Let’s say we define sentences to be equivalent if 50% or more of their tokens are equivalent.  Here are two sample sentences:

      The young cat is hungry.
      The cat is very hungry.

  When we parse these sentences to remove stopwords, we end up with the following two sets:

      {young, cat, hungry}
      {cat, very, hungry}

  In our example above, our intersection is {cat, hungry}, which has count of two.
  The union of the sets is {young, cat, very, hungry}, which has a count of four.
  Therefore, our Jaccard similarity index is two divided by four, or 50%.

Time Logic Adapter
------------------

.. autofunction:: chatterbot.adapters.logic.TimeLogicAdapter

The `TimeLogicAdapter` identifies statements in which a question about the current time is asked.
If a matching question is detected, then a response containing the current time is returned.

Example
+++++++

| User: What time is it?
| Bot: The current time is 4:45PM.

Mathematical Evaluation Adapter
---------------------------------

.. autofunction:: chatterbot.adapters.logic.MathematicalEvaluation

The `MathematicalEvaluation` logic adapter checks a given statement to see if
it contains a mathematical expression that can be evaluated. If one exists,
then it returns a response containing the result.
This adapter is able to handle any combination of word and numeric operators.

Example
+++++++

| User: What is four plus four?
| Bot: (4 + 4) = 8

Creating a new logic adapter
----------------------------

You can write your own logic adapters by creating a new class that
inherits from `LogicAdapter` and overides the overrides necessary
methods established in the base `LogicAdapter` class.

.. autofunction:: chatterbot.adapters.logic.LogicAdapter

.. code-block:: python

   from chatterbot.adapters.logic import LogicAdapter

   class MyLogicAdapter(LogicAdapter):
       def __init__(self, **kwargs):
           """
           The __init__ method is optional.
           If you need to access any of the kwargs that were
           passed into the ChatBot constructor this is where
           you can do so.

           (An API key might be an example of a parameter
           you would want to access here.)
           """

       def can_process(self, statement):
           """
           This is a preliminary method that can be used to
           check the input statement to see if a value can
           possibly be returned when it is evaluated using
           the adapter's process method.

           This method returns true by default.
           """
           return True

       def process(self, statement):
           """
           This method will receive the input statement.
           Establish custom response selection logic here.
           """
           return selected_statement

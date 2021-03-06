StronglyTypedContext
====================

A library for creating strongly typed scenario contexts in specflow using Castle Dynamic Proxy

## Why

Specflow is a fantastic library for writing BDD tests.  One of the downfalls of the library is to keep data between steps you have to rely on the ScenarioContext.Current dictionary.  The signature of the dictionary is:

To add an item:
    `ScenarioContext.Current.Add(string key, object value);`

To get an item:
    `var item = ScenarioContext.Current[key];`

The problem with this is that you end up with "magic" strings littered throughout your code and you have to cast to and from object.  You can tidy this up a bit by using constants and generics but it is still a bit clunky.  Wouldn't it be much nicer if we could have a strongly typed context?

## How

This library provies a strongly typed context.  To use the library all you have to do is
* Install the [StronglyTypedContext] (https://www.nuget.org/packages/StronglyTypedContext) nuget package from nuget.org
* Derive your step definition class from the abstract StronglyTypedContext.BaseBinding class
* Create an interface to hold your scenario context information
* Create a public virtual property on your step definition class of interface you have created and mark it with the ScenarioContext attribute
* That's it you are good to go

Example
=======

    public interface IMyContext
    {
        public int Value { get; set; }
        public string AnotherValue { get; set; }
    }

    public class StepDefiniton : BaseBinding
    {
        [ScenarioContext]
        public virtual IMyContext Context { get; set; }

        [Given("I am doing something")]
        public void GivenIAmDoingSomething()
        {
           // You can now use your context here
           Context.Value = 42;
        }
    
        [When("I execute")]
        public void WhenIExecute()
        {
           // Context is persisted using Scenario context
           var value = Context.Value; // will be 42
        }
    }

## More information
For more information on how the code was written see [my blog post](http://kevinholditch.co.uk/2013/12/10/strongly-typed-scenariocontext-in-specflow/)

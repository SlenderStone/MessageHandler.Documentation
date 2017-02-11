# SDK Reference : Handler Runtime

## Getting Started

The handler runtime provides all the features required to run autonomous software components. 

This runtime is intended for hosting inside a console application, or a similar host. Here is some example code that shows how to configure, initialize and run the runtime.

<!-- Start Code Block -->

	class Program
    {
        static void Main(string[] args)
        {
            try
            {
                MainAsync(args).Wait();
            }
            catch (AggregateException ex)
            {
                ExceptionDispatchInfo.Capture(ex.Flatten().InnerExceptions.First()).Throw();
            }
        }

        static async Task MainAsync(string[] args)
        {
			// configuration phase
            var config = new HandlerRuntimeConfiguration();
			// initialization phase
            var runtime = await HandlerRuntime.Create(config);
			// operation phase
            await runtime.Start();
            Console.WriteLine("Handler started");
            // wait until signalled to stop
			Console.ReadLine();
            await runtime.Stop();
        }
    }

<!-- End Code Block -->

The runtime's lifecycle goes through three different phases.
 
 * **Configuration Phase:** During this phase configuration values are added to the `HandlerRuntimeConfiguration`'s internal settings. These values will be used during the initialization and runtime phase.
 * **Initialization Phase:** During the initialization phase, the configuration becomes readonly and is applied to all different internal, and external, objects required to operate the hosted tasks.
 * **Operation Phase:** Runs all registered startup tasks, followed by all background tasks. Background tasks will remain running until the runtime is instructed to stop.

## Reference

* [Performing Configuration](/documentation/sdk/runtime/configuration)
	* Managing Settings
	* Managing dependencies with the Inversion Of Control Container
	* Extending the configuration infrastructure
* Startup Tasks
* Background Tasks* [Serialization](/documentation/sdk/runtime/serialization)
* [Working with Dynamic Json](/documentation/sdk/dynamic-json)

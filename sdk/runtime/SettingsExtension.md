# SDK Reference : Settings Extension

## Getting Started

This is a code example of how you can best code a settings extension for MessageHandler.
To make an extension method you have 2 options. You can use ISettings directly or use HandlerRuntimeConfiguration indirectly.
To make an extension method using HandlerRuntimeConfiguration, you'll need `.GetSettings()`

<!-- Start Code block -->
	public static class MyExtensions
    	{
        	private const string MyPropertyKey = "MyProperty";
	        public static void SetMyProperty(this HandlerRuntimeConfiguration configuration, TimeSpan timeSpan)
        	{
			var settings = configuration.GetSettings();
			settings.Set(MyPropertyKey, timeSpan);
        	}

	        public static TimeSpan GetMyProperty(this ISettings settings)
        	{
			return settings.Get<TimeSpan>(MyPropertyKey);
        	}

	        public static void SetDefaultMyProperty(this ISettings settings, TimeSpan timeSpan)
        	{
			settings.SetDefault(MyPropertyKey, timeSpan);
		}
    }
<!-- End Code Block -->

If you want your code to only be accessible from your assembly, you'll need to use "Internal".


## Reference
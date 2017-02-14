# SDK Reference : Settings Extension

This documentation is meant for developers that want to build their own higher level frameworks on top of the MessageHandler Runtime.

##Example

This is a code example of how you can create a settings extension for the MessageHandler Runtime.

To make an extension method you have 2 options. You can extend ISettings directly or extend it through HandlerRuntimeConfiguration indirectly.
In this case you can leverage the `.GetSettings()`extension method to get access to the settings.

<!-- Start Code block -->
	public static class MyExtensions
    {
        private const string MyPropertyKey = "MyProperty";

	    public static void SetMyProperty(this HandlerRuntimeConfiguration configuration, TimeSpan timeSpan)
    	{
			var settings = configuration.GetSettings();
			settings.Set(MyPropertyKey, timeSpan);
      	}

        internal static TimeSpan GetMyProperty(this ISettings settings)
     	{
			return settings.Get<TimeSpan>(MyPropertyKey);
     	}

	    internal static void SetDefaultMyProperty(this ISettings settings, TimeSpan timeSpan)
    	{
			settings.SetDefault(MyPropertyKey, timeSpan);
		}
    }
<!-- End Code Block -->

Methods that need to be available for other developers should be public and available on HandlerRuntimeConfiguration. Methods that are for internal use only should extend ISettings and be marked as internal.
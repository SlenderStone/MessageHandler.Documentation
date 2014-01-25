To send a message to your channel using the AMQP protocol, you need to use a client side library for the AMQP 1.0 protocol and specify the information provided by the endpoint. 
We used the azure servicebus sdk in the following example.

	private void Send()
    {
		 var factory = MessagingFactory.Create(
                "sb://" + endpoint.host,
                new MessagingFactorySettings()
                {
                    TransportType = TransportType.Amqp,
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(endpoint.username, endpoint.password)
                });

            var sender = factory.CreateMessageSender(endpoint.path);

            sender.Send(new BrokeredMessage("Hello world"));
            
            sender.Close();
    }
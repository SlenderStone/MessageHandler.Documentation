## Average in period

Computes an average value for measurements in a defined time period

### Handler properties

**MeasurementType:** A string representing the measurement type.

**DurationInSeconds:** A window of time in which to compute the mathematical average.

### Message requirements

**Serializer:** JSON

**Format:** 

	{
		Amount: double,
        Unit: string,
        MeasurementType: string,
        AggregationMode: string
	}

###Notes

**AggregationMode:** Must be empty or have value "None" 

### Outgoing message

**Serializer:** JSON

**Format:** 

	{
		Amount: double,
        Unit: string,
        MeasurementType: string,
        AggregationMode: string
	}

###Notes

**AggregationMode:** Will have the value "Average"


# automation-settings
Custom Setting for managing disabling automations in Salesforce

## Apex Triggers
Should be incorporated into a centralised Trigger Handler.
```
if (Automation_Settings__c.getInstance().Disable_Triggers__c) {
    // Don't run trigger
    return;
}
```

## Process Builder
All Process Builders should have the Custom Setting evaluation check at the START of the PB. This ensures that the Custom Setting condition doesn’t need to be embedded into every condition on the Process Builder.

Example as follows:
1. Process Builder Disabled is the first condition in the Process Builder
2. It uses a formula evaluation to return TRUE if the Custom Setting to DISABLE Process Builder is ENABLED
3. Using the STOP decision at the end of evaluation will cause nothing else in the Process Builder to run
4. The “Do Nothing” action only exists because an Immediate Action is required by Process Builder. This quite literally does nothing (it calls a blank Apex method).

![PB](img/pb.png?raw=true "PB")
![PB](img/pb1.png?raw=true "PB")


## Record-Triggered Flows
All Record-Triggered Flows should also have a condition added to the start to only execute logic if Flows ARE NOT disabled.

This is similar to Process Builder above, and requires a condition added to the start of each Flow. 

Note: This should also be added to added to any Scheduled Flows or Autolaunched Flows that are executed from Apex or other jobs.

1. First element of the Flow should evaluate the Custom Setting
2. If NOT disabled, continue with rest of Flow of logic.

![Flow](img/flow.png?raw=true "Flow")
![Flow](img/flow1.png?raw=true "Flow")

## Workflow Rules
```
AND(
    NOT($Setup.Automation_Settings__c.Disable_Workflow__c),
    ... other condtions
)
```
OR
```
NOT($Setup.Automation_Settings__c.Disable_Workflow__c)

&&

... other conditions
```

## Validation Rules
```
AND(
    NOT($Setup.Automation_Settings__c.Disable_Validations__c),
    ... other condtions
)
```

OR

```
NOT($Setup.Automation_Settings__c.Disable_Validations__c)

&&

... other conditions
```
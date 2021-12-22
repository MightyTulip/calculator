# calculator

## Install dependencies
```
npm install
```

## Run application
```
vue-cli-service serve
```

## Solution
- Structure of and styling taken mostly from google itself and used library to parse the calculation for easiness
- Needed various class names to implement the slightly different styling for the buttons which should have been resolved if using a UI library such as tailwind or bootstrap

## Length ~4 hours

## Assumptions
- If dividing by 0 we only provide an error then allow the user to continue with a new input
- the `Ans=` wording seem to indicate that you should always have that there, but the way google function is it replaces the `Ans =` with the input when user makes a new calculation. So implementation follows that.

## Most proud
- Implementation of the `Ans = ` that gets replaced with the previous input I thought was good - could be improved in terms of implementation however the functionality is there and looks like google's implementation 

## Improvement
- Potentially create component based for each of the button so each click has an active state (not implemented here) and general code clean up as this file seems too big the with styling added in
- Checks implemented are very strict potentially could of used `indexOf` more often as I continued to implement I needed to keep adding new class names as some buttons had slightly different styling to others and some functionality were different
- use regex to replace certain symbols instead of multiple replace

## Deviations
- No active state when user clicks on any of the buttons 
- No active state on the input field as users are adding inputs values  

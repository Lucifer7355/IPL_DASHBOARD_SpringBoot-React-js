# IPL_DASHBOARD_SpringBoot-React-js

# Demonstration :-
## HomePage 
![HomePage1](https://github.com/Lucifer7355/IPL_DASHBOARD_SpringBoot-React-js/blob/main/photos_for_demonstration/Screenshot%20(271).png)
![HomePage2](https://github.com/Lucifer7355/IPL_DASHBOARD_SpringBoot-React-js/blob/main/photos_for_demonstration/Screenshot%20(272).png)
## TeamAnalysisPage
![TeamAnalysisPage1](https://github.com/Lucifer7355/IPL_DASHBOARD_SpringBoot-React-js/blob/main/photos_for_demonstration/Screenshot%20(273).png)
![TeamAnalysisPage2](https://github.com/Lucifer7355/IPL_DASHBOARD_SpringBoot-React-js/blob/main/photos_for_demonstration/Screenshot%20(274).png)
## TeamMatchesPerYear
![MatchesPerYear1](https://github.com/Lucifer7355/IPL_DASHBOARD_SpringBoot-React-js/blob/main/photos_for_demonstration/Screenshot%20(275).png)
![MatchesPerYear2](https://github.com/Lucifer7355/IPL_DASHBOARD_SpringBoot-React-js/blob/main/photos_for_demonstration/Screenshot%20(276).png)

# Installation :- 
```
- clone the repository by hitting command
git clone https://github.com/Lucifer7355/IPL_DASHBOARD_SpringBoot-React-js.git
- open frontend and install dependencies by writing command : 'npm i' without quotes.
- To start front-end run command 'npm start' without quotes.
- Now move to backend and run the backend SpringBoot instance using STS.
```

# Backend : -
Tools : SpringBoot, H2-DB, Spring Web, Spring MVC.

1. First Of all the data used for processing and sending needs to be set. That IPL match data has been downloaded from Kaggle in a csv file having name match-data.csv .
2. That CSV has to be put under resources folder for processing purpose.
3. The first thing is processing the CSV data. It has 17 Columns having data about---> 
   Columns_names = {id,city,date,player_of_match,venue,neutral_venue,team1,team2,toss_winner,toss_decision,winner,result,result_margin,eliminator,method,umpire1,umpire2}
4. Initially in the package "data", MatchInput class is created having the attributes same as column names having data type as String Format.
5. MatchDataProcessor class is created inside the same package which implements ItemProcessor interface. It takes the "MatchInput" class object as input and creates a "Match" object which is an entity model inside the model package.
   Match object has only following details to be mapped to any database :  {id,city,date,playerOfMatch,venue,team1,team2,tossWinner,tossDecision,matchWinner,result,resultMargin,umpire1,umpire2}.
   Match Object takes required details from MatchInput object. Sets Team1 and Team2 based on the innings order.
6. Now after completion of a Batch Processing Job, there has to be a notifier. Hence JobCompletionNotificationListener class is created which extends JobExecutionListenerSupport class. After job of processing the data is completed using csv data, It will print [teamName, totalMatches, totalWins] for each of the teams from available csv data. 
7. Now a BatchConfig Class is created inside "data" package. Here all the 17 FIELD_NAMES are listed in array. It will process the required data from the csv file and store into the in-memory database.
8. After that under model package Team entity class is created and under repository package repositories are created for MatchRepository and TeamRepository. Using these repositories, in-memory data objects can be accessed using JPA.
9. Inside controller package, Created a TeamController java class which has routes configured and repositories for Team and Match entity classes autowired.
```
The routes are as follows: - 
    - "/team" ---> it returns all the teams present in the database.
	- "/team/{teamName}" ----> it finds out that team using teamName       (obtained from pathvariable) from database and sets 4 latest matches for the same team from the matches database.
	- "/team/{teamName}/matches" ----> the year is passed in requestparams and teamName in PathVaribales. It will return all matches for that team in that year from Match Repository.
```
# FrontEnd : -
Tools : React js, SASS, Axios.

1. For FrontEnd a page has been made to render number of teams, stats for each teams and number of matches won or lost for each year by a team.
2. Since its a multicomponent single page application, a BrowserRouter has been used to route request to required components as per routes to Different components namely---> MatchPage, TeamPage, HomePage.
3. HomePage Component makes a REST API call to backend to fetch all team details and renders it on UI.
   TeamPage Component makes a REST API call to get total number of matches,totalwins,teamname and last 4 matches for the current team and displays it beautifully on the web UI.
   MatchPage Component makes a REST API call to get all matches for a team in a particular year and renders it beautifully on the web UI.

4. Further reusable components are MatchDetailCards which are used for rendering matchdetails for a particular team in rectangular form. 
   MatchSmallCard is used for past 3 teams to be displayed under the main team being considered. 
   TeamTile is just to put the name of Team on the UI.
   YearSelector is made to make available a certain number of years for which that team played match so that, details of matches played by a team in a particular year is made available in UI.
5. Finally SASS is used to style the components and their childs.
6. Finally the front-end can be build and bundled to be server and deployed from the same port as backend which makes deployment process more easy. 


```
Now Just Run Backend and frontend separately on distinct ports. The request from frontend port is whitelisted in the backend controller, hence cross-origin-request are easy to process.
```

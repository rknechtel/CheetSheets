# Copy/Clone SQL Server Agent Job  

**The EASY Way!**  

- Connect to SQL Server Instance in SQL Server Mangement Studio
- Click on `SQL Server Agent` in left hand navigation to expand it.
- Click on `Jobs` in left hand navigation to expand it.
- Right cilck on Agent Job to Copy/Clone
- Select "Script Job As" --> "CREATE To" --> "New Query Editor Window" 
  OR
  Select "Script Job As" --> "CREATE To" --> "File"
- Save as: AgentJob-JOBNAME.sql
- Then do a "refresh" on the "Jobs" and you will see your cloned job you can then modify.
- To "import" the Agent Job, just open the .sql file and click the "Execute Query" button.  
- Then "Refresh" the Agent Jobs"

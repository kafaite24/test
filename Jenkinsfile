import groovy.sql.Sql


output = ""

pipeline {
    agent any
    stages {
       
		stage('Create tabless'){
			 steps{
				 
                script{
		
		 	sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM employees_final)=100 THEN 1 ELSE 0 END as output") { row ->
				output= "All rows inserted in employees table"+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM departments_final)=4 THEN 1 ELSE 0 END as output") { row ->
				output += "All rows inserted in departments table"+"\t\t\t$row.output\n"
		}
			
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM jobs_final)=5 THEN 1 ELSE 0 END as output") { row ->
				output += "All rows inserted in jobs table      "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT COUNT(*) FROM employees_final GROUP BY employee_id HAVING COUNT(*) > 1) is null THEN 1 ELSE 0 END as output") { row ->
				output += "No duplicates in employees table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT COUNT(*) FROM departments_final GROUP BY department_id HAVING COUNT(*) > 1) is null THEN 1 ELSE 0 END as output") { row ->
				output += "No duplicates in departments table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
                  	
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT COUNT(*) FROM jobs_final GROUP BY job_id HAVING COUNT(*) > 1) is null THEN 1 ELSE 0 END as output") { row ->
				output += "No duplicates in jobs table   "+"\t\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM dbc.columns WHERE tablename= 'employees_final' and columnname IS NULL)=0 then 1 else 0 end as output") { row ->
				output += "No nulls in employees table   "+"\t\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM dbc.columns WHERE tablename= 'departments_final' and columnname IS NULL)=0 then 1 else 0 end as output") { row ->
				output += "No nulls in departments table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM dbc.columns WHERE tablename= 'jobs_final' and columnname IS NULL)=0 then 1 else 0 end as output") { row ->
				output += "No nulls in jobs table   "+"\t\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(gender) from employees_final where gender in ('Male', 'Female'))=0 then 1 else 0 end as output") { row ->
				output += "Gender column values changed to 'M' and 'F'"+"\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
		
      }
}}}
	post {
                always {
                    archiveArtifacts artifacts: 'output.txt', allowEmptyArchive: true
                }
            }
}

@NonCPS
    def sqlconnection() {
        String URL = "jdbc:teradata://wbtlabserver.teradata.com/KH255051";
        String username = "KH255051";
        String password = "bSji5jkv";
        String driver= "com.teradata.jdbc.TeraDriver"
        
        echo "Building connection"
        def sqlconn = Sql.newInstance(URL, username, password, driver)
        return sqlconn
    }

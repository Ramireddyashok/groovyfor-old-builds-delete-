# groovyfor-old-builds-delete-


println("=======================================================================");
println("=======================================================================");
println("========================Pipeline jobs=================================");

def getPipelineJobNames() {
    Hudson.instance.getAllItems(org.jenkinsci.plugins.workflow.job.WorkflowJob)*.fullName 
}

Jenkins.instance.getAllItems(org.jenkinsci.plugins.workflow.job.WorkflowJob).each {it ->
	
	   def fullJobName=it.getFullDisplayName().toString();
	   
		if(fullJobName.contains("Retention_policy"))
		{
		    println("Excluding pipeline job :" + fullJobName)
		}
		else if(fullJobName.contains("Jenkins_Retention_Policy"))
		{
		    println("Excluding pipeline job :" + fullJobName)
		}
		//In applying to production the below if is to be replaced with else ,to apply the discard on all jobs except sourcing
        else
		{ 
		    println("Clearing up Build History for Job Name :" + fullJobName)
		    def builds=it.getBuilds()
		    buildtoKeep=20;
		    numberOfBuildsinJob = builds.size()
		    println();
		    if(numberOfBuildsinJob>buildtoKeep)
		    {
				println("The number of Builds for the job :" + fullJobName + " is :" + numberOfBuildsinJob)
				builds.drop(buildtoKeep).each
				{b-> b.number;
try {
					    b.delete();
					    println( "Deleted build :" + b.number)
    }
catch(InterruptedException ex)
{
    Thread.currentThread().interrupt(); // Here!
    throw new RuntimeException(ex);
    println("Interrupted")
}
catch (Exception ex)
{
    println("Exception occured in job " +fullJobName + " build "+b.number)
    println("Exception Reason: "+ex)
}
    
		    		}
		    }
		   else
		   {
		       println("No Builds will be deleted as the build count is less than Builds to Keep");
		   }
		    
		}
}

println("=======================================================================");
println("=======================================================================");
println("========================Freestyle jobs=================================");

Jenkins.instance.getAllItems(AbstractProject.class).each {it ->
	def fullJobName=it.getFullDisplayName().toString();
        ///*Code to exclude Sourcing JOBS
	    if(fullJobName.contains("Retention_policy"))
		{
		    println("Excluding pipeline job :" + fullJobName)
		}
		else if(fullJobName.contains("Jenkins_Retention_Policy"))
		{
		    println("Excluding pipeline job :" + fullJobName)
		}
		
	   //In applying to production the below if is to be replaced with else ,to apply the discard on all jobs except sourcing
        else
		{ 
		    println("Clearing up Build History for Job Name :" + fullJobName);
		    def builds=it.getBuilds()
		    buildtoKeep=20;
		    numberOfBuildsinJob = builds.size()
		    println();
		    if(numberOfBuildsinJob>buildtoKeep)
		    {
				println("The number of Builds for the job :" + fullJobName + " is :" + numberOfBuildsinJob)
				builds.drop(buildtoKeep).each
				{b-> b.number;
try {
					    b.delete();
					    println( "Deleted build :" + b.number)
    }
catch(InterruptedException ex)
{
    Thread.currentThread().interrupt(); // Here!
    throw new RuntimeException(ex);
    println("Interrupted")
}
catch (Exception ex)
{
    println("Exception occured in job " +fullJobName + " build "+b.number)
    println("Exception Reason: "+ex)
}
			}
		     }
		    else
		   {
		       println("No Builds will be deleted as the build count is less than Builds to Keep");
		   }
		    
		}

}


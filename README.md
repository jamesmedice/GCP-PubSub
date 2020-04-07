gsutil mb gs://$BUCKET_NAME


gcloud pubsub topics create cron-topic


gcloud scheduler jobs create pubsub publisher-job --schedule="* * * * *" \
 --topic=cron-topic --message-body="JAMES MEDICI"
 

 gcloud scheduler jobs run publisher-job


git clone https://github.com/GoogleCloudPlatform/java-docs-samples.git
cd java-docs-samples/pubsub/streaming-analytics


mvn compile exec:java \
  -Dexec.mainClass=com.medici.app.Application \
  -Dexec.cleanupDaemonThreads=false \
  -Dexec.args=" \
    --project=$PROJECT_NAME \
    --inputTopic=projects/$PROJECT_NAME/topics/cron-topic \
    --output=gs://$BUCKET_NAME/samples/output \
    --runner=DataflowRunner \
    --windowSize=2"
	
	
gsutil ls gs://${BUCKET_NAME}/samples/
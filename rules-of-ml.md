From: [https://developers.google.com/]
# Rules of machine learning
## Best practices of ML Engineering

This document is intended to get the benefit of Google's best practices in machine learning.

### Terminology
* Instance: The thing about which you want to make a prediction. For example, the instance might be a picture that you want to classify as either "a cat" or "not a cat".
* Label: An answer for a prediction task either the answer produced by a machine learning system, or the right answer supplied in training data. For example, the label for a picture might be "a cat".
* Feature: A property of an instance used in a prediction task. For example, a picture might have a feature "has whiskers".
* Feature column: A set of related features, such as the set of all possible countries in which users might live. An example may have one or more features present in a feature column.
* Example: An instance (with its features) and a label.
* Model: A statistical representation of a prediction task. I train a model on examples then use the model to make predictions.
* Metric: A number that you care about. May or may not be directly optimized.
* Objective: A metric that your algorithm is trying to optimize.
* Pipeline: The infrastructure surrounding a machine learning algorithm. Includes gathering the data from the front end, putting it into training data files, training one or more models, and exporting the models to production.

## Overview:
To make great products:
**Do machine learning like the great engineer you are, not like the great machine learning expert you aren't**.

Most of the problems I will face are engineering problems. Even with all the recourses of a great machine learning expert, most of the gains come from great features, not great machine learning algorithms. The basic approach is:
1. Make sure your pipeline is solid end to end.
2. Start with a reasonable objective.
3. Add common-sense features in a simple way.
4. Make sure that your pipeline stays solid.

This approach will work well for a long period of time. Diverge from this approach only when there are no more simple tricks to get you any farther. Adding complexity slows future releases.
Once you've exhausted the simple tricks, cutting-edge machine learning might indeed be in your future.

This document is arranged as follows:
1. The first part should help you understand whether the time is right for building a machine learning system.
2. The second part is about deploying your first pipeline.
3. The third part is about launching and iterating while adding new features to your pipeline, how to evaluate models and training-serving skew.
4. The final part is about what to do when you reach a plateau.

## Before Machine Learning
### Rule #1: Don't be afraid to lauch a product without machine learning.

Machine learning is cool, but it requires data. Theoretically, you can take data from a different problem and then tweak the model for a new product, but this will likely underperform basic heuristics. For instance, if you are ranking apps in an app marketplace, you could use the install rate or number of installs as heuristics. If you are detecting spam, filter out publishers that have sent spam before. Don't be afraid to use human editing either. If you need to rank contacts, rank the most recently used highest. If machine learning is not absolutely required for your product, don't use until you have data.

### Rule #2: First, design and implement metrics.
Before formalizing what your machine learning system will do, track as much as possible in your current system. Do this for the following reasons:
1. It is easier to gain permission from the system's users earlier on.
2. If you think that something might be a concern in the future, it is better to get historical data now.
3. If you design your system with metric instrumentation in mind, things will go better for you in the future. Specifically, you don't want to find yourself grepping for string in logs to instrument your metrics!
4. You will notice what things change and what stays the same. For instance, suppose you want to directly optimize one-day active users. However, during your early manipulations of the system, you may notice that dramatic alterations of the user experience don't noticeable change this metric.

### Rule #3: Choose machine learning over a complex heuristic.
A simple heuristic can get your product out of the door. A complex heuristic is unmaintainable. Once you have data and a basic idea of what you are trying to accomplish, move on to machine learning. As in most software engineering tasks, you will want to be constantly updating your approach, whether it is a heuristic or a machine learned model, and you will find that the machine-learned model is easier to update and maintain.

ML Phase 1: Your First Pipeline
Focus on your system infrastructure for your first pipeline. While it is fun to think about all the imaginative machine learning you are going to do, it will hard to figure out what is happening if you don't first trust your pipeline.

### Rule #4: Keep the first model simple and get the infrastructure right.
The first model provides the biggest boost to your product, so it doesn't need to be fancy. But you will run into more infrastructure issues than you expect. Before anyone can use your fancy new machine learning system, you have to determine:
    * How to get examples to your learning algorithm.
    * A first cut as to what "good" and "bad" mean to your system.
    * How to integrate your model into your application. You can either apply the model live, or precompute the model on examples offline and store the results in a table. For example, you might want to preclassify web pages and store the results in a table, but you might want to classify chat messages live.
    
Choosing simple features makes it easier to ensure that:
    * The features reach your learning algorithm correctly.
    * The model learns reasonable weights.
    * The features reach your model in the server correctly.

Once you have a system that does these three things reliable, you have done most of the work. Your simple model provised you with baseline metrics and a baseline behaviour that you can use to test more complex models. Some teams aim for a "neutral" first launch: a first lauch that explicitly deprioritizes machine learning gains, to avoid getting distracted.

### Rule #5: Test the infrastructure independently from the machine learning.
Make sure that the infrastructure is testable, and that the learning parts of the system are encapsulated so that you can test everything around it. Specifically:
    1. Test getting data into the algorithm. Check that feature columns that should be populated are populated. Where privacy permits, manually inspect the input to your training algorithm. If possible, check statistics in your pipeline in comparison to statistics for the same data processed elsewhere.
    2. Test getting models out of the training algorithm. Make sure that the model in your training environment gives the same score as the model in your serving environment.

Machine learning has an element of unpredictability, so make sure that you have tests for the code for creating examples in training and serving, and that you can load and use a fixed model during serving.

### Rule #6: Be careful about dropped data when copying pipelines.
Often we create a pipeline by copying an existing pipeline, and the old pipeline drops data that we need for the new pipeline. For example, the pipeline for Google Plus What's Hot drops older posts. This pipeline was copied to use for Google Plus Stream, where older posts are still meaningful, but the pipeline was still dropping old posts. Another common pattern is to only log data that was seen by the user. Thus, this data is useless if we want to model why a particular pors was not seen by the user, because all the negative examples have been dropped. A similar issue occurred in Play. While working on Play Apps Home, a new pipeline was created that also contained examples from the landing page for Play Games without any feature to disambiguate where each example came from.

### Rule #7: Turn heuristics into features, or handle them externally.
Usually the problems that machine learning is trying to solve are not completely new. There is an existing system for ranking, or classifying, or whatever problem you are trying to solve. This means that there are a bunch of rules and heuristics. **These same heuristics can give you a lift when tweaker with machine learning.** Your heuristics should be mined for whatever information they have, for two reasons. First, the transition to a machine learning system will be smoother. Second, usually those rules contain a lot of the intuition about the system you don't want to throw away. There are four ways you can use an existing heuristic:
    * Preprocess using the heuristic. If the feature is increadibly awesome, then this is an option. For example, if, in a spam filter, the sender has already been blacklisted, don't try to relearn what "blacklisted" means. Block the message. This approach makes the most sense in binary classification tasks.
    * Create a feature. Directly creating a feature from the heuristic is great. For example, if you use a heuristic to compute a relevance score for query result, you can include the score as the value of a feature. Later on you may want to use machine learning techniques to massage the value but start by using the raw value produced by the heuristic.
    * Mine the raw inputs of the heuristic. If there is a heuristic for apps that combines the number of installs, the number of characters in the text, and the day of the week, then consider pulling these pieces apart, adn feeding these inputs into the learning separately. Some techniques that apply to ensembles apply here.
    * Modify the label. This is an option when you feel that the heuristic captures information not currently contained in the label. For example, if you are trying to maximize the number of downloads, but you also want quality content, then maybe the solution is to multiply the label by the average number of stars the app received. There is a lot of leeway here.

Do be mindful of the added complexity when using heuristics in an ML system. Using old heuristics in your new machine learning algorithm can help to create a smooth transition, but think about whether there is a simpler way to accomplish the same effect.

Monitoring
In general, practice good alerting hygiene, such as making alerts actionable and having a dashboard page.

### Rule #8: Know the freshness requirements of your system.
How much does performance degrade if you have a model that is a day old? A week old? A quarter old? This information can help you to understand the priorities of your monitoring. If you lose significant product quality if the model is not updated for a day, it makes sense to have an engineer watching it continuously. Most ad serving systems have new advertisements to handle every day, and must update daily. For instance, if the ML model for Google Play Search is not updated, it can have a negative impact in under a month. Some models for What's Hot in Google Plus have no post identifier in their model so they can export these models frequently. Other models that have post identifiers are updated much more frequently. Also notice that freshness can change over time, especially when feature columns are added or removed from your model.

### Rule #9: Detect problems before exporting models.
Many machine learning systems have a stage where you export the model to serving. If there is an issue with an exported model, it is a user-facing issue.

Do sanity checks right before you export the model. Specifically, make sure that the model's performance is reasonable on help out data. Or, if you have lingering concerns with the data, don't export a model. Many teams continuously deploying models check the area under the ROC curve (ROC curve is created by plotting the true positive rate against the false positive rate). **Issues about models that haven't been exported require an email alert, but issues on a user-facing model may require a page.** So better to wait and be sure before impacting users.

### Rule #10: Watch for silent failures.
This is a problem that occurs more for machine learning systems than for other kinds of systems. Suppose that a particular table is being joined is no longer being updated. The machine learning system will adjust, and behaviour will continue to be reasonable good, decaynig gradually. Sometimes you find tables that are months out of data, and a simple reshresh improves performance more than any other launch that quarter! The coverage of a feature may change due to implementation changes: for example a feature column could be populated in 90% of the examples, and suddenly drop to 60% of the examples. Play once had a table that was stale for 6 months, and refreshing the table gave a boost of 2% in install rate. If you track statistics of the data, as well as manually inspect the data on occassion, you can reduce these kinds of failures.

### Rule #11: Give feature coluns owners and documentation.
If the system is large, and there are many feature columns, know who created or is maintaining each feature column. If you find that the person who understands a feature column is leaving, make sure that someone has the information.


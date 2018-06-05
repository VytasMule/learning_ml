# Contents:
1. How should we define AI?
2. Related fields
3. Philosophy of AI

# How should we define AI?
Application 1. Self-driving cars
    Implications: road safety should eventually improve as the reliability of the systems surpasses human level. The efficiency of logistics chains when moving goods should improve. Humans move into a supervisory role, keeping an eye on what's going on while machines take care of the driving.
    
Application 2. Content recommendation
    Implications: while many companies don't want to reveal the details of their algorithms, being aware of the basic principles helps you understand the potential implications: these involve so called filter bubbles, echo-chambers, troll factories, fake news, and new forms of propaganda.
    
Application 3. Image and video processing
    Implications: when such techniques advance and become more widely available, it will be easy to create natural looking fake videos of evens that are impossible to distinguish from real footage. This challenges the notion that "seeing is believing".

## What is, and what isn't AI? Not an easy question!
Reason 1: no officially agreed definition
Reason 2: the legacy of science fiction
Reason 3: what seems easy is actually hard. Example: while it's easy for you, grasping objects by a robot is extremely hard, and it is an area of active study. Recent examples include Google's robotic grasping project and cauliflower picking robot.
Reason 4: and what seems hard is actually easy. Example: it turns out that playing chess is very well suited to computers, which can follow fairly rules and compute many alternative move sequences at a rate of billions of computations a second. While in-depth mastery of mathematics requires human intuition and ingnuity, many exercises of a typical high-school or college course can be solved by applying a calculator and simple set of rules.

## Key terminology
Autonomy: the ability to perform tasks in complex environments without constant guidance by a user.
Adaptivity: the ability to improve performance by learning from experience.

### Note
Watch out for 'suitcase words'. Marvin Minsky, a cognitive scientist and one of the greatest pioneers in AI, coined the term suitcase word for terms that carry a whole bunch of different meanings that come along even it we intend only one of them. Using such terms increases the risk of misinterpretations such as the ones above.

## Exercise 1: Is this AI or not?
1. Spreadsheet that calculates sums and other pre-defined functions on given data. No
2. Predicting the stock market by fitting a curve to past data about stock prices. Yes/No/Kind of
3. A GPS navigation system for finding the fastest route. Yes/No/Kind of
4. A music recommendation system such as Spotify that suggests music based on the user's listening behavior. Yes
5. Big data storage solutions that can store huge amounts of data (such as images or video) and stream them to many users at the same time. No
6. Image filters in applications such as Photoshop. No/Kind of
7. Style transfer filters in applications such as Prisma that take a photo and transform it into different art styles (impressionist, cubist, ...). Yes

# Related fields
## Key terminology
Machine learning: systems that improve their performance in a given task with more and more experience and data.

**Deep learning** is a subfield of machine learning, which ifself is a subfield of AI, which itself is a subfield of computer science. For now let us just note that the "depth" of deep learning refers to the complexity of a mathematical model, and that the increased computing power of modern computers has allowed researchers to increase this complexity to reach levels that appear not only quantitatively but also qualitatively different from before.

**Deep science** is a recent umbrella term that includes machine learning and statistics, certain aspects of computer science including algorithms, data storage, and web application development. Data science is also a practical discipline that requires understanding of the domain in which it is applied in, for exaple, business or science.

**Robotics** means building and programming robots so that they can operate in complex, real-world scenarios. In a way, robotics is the ultimate challenge of AI since it requires a combination of virtually all areas of AI. For example:
    * Computer vision and speech recognition for sensing the environment.
    * Natural language processing, information retrieval, and reasoning under uncertainty for processing instructions and predicting consequences of potential actions.
    * Cognitive modeling and affective computing for interacting and working together with humans.

Many of the robotics-related AI problems are best approached by machine learning, which makes machine learning a central branch of AI for robotics.

## Exercise 2: Taxonomy of AI
A taxonomy is a scheme for classifying many things that may be special cases of one another. We have explained the relationships between a number of disciplines or fields and pointed out, for example, that machine learning is usually considered to be a subfield of AI.

**Your task: construct a taxonomy in the Euler diagram example given below showing the relationships between the following things: AI, machine learning, computer science, data science and deep learning.

Where would you put AI?
AI is a part of computer science.

Where would you put machine learning?
Machine learning is usually considered to be a part of AI.

Where would you put computer science?
Computer science is a relatively broad field that includes AI but also other subfields such as distributed computing, human-computer interaction and software engineering.

Where would you put data science?
Data science needs computer science and AI. However, it also involves a lot of statistics, business, law and other application domains, so it is usually not considered to be a part of computer science.

Where would you put deep learning?
Deep learning is a part of machine learning.

## Exercise 3: Examples of tasks
Consider the following example tasks. Try to determine which AI-related fields are involved in them.

Autonomous car. Statistics/Robotics/Machine learning
Steering a rocket into orbit. Robotics
Online ad optimization. Statistics/Machine learning
Customer service chatbot. Machine Learning
Summarizing gallup results. Statistics

# Philosophy of AI
The Turing test
Alan Turing was an English mathematician and logician. He is rightfully considered to be the father of computer science. Turing was fascinated by intelligence and thinking, and the possibility of simulating them by machines. Turing's most prominent contribution to AI in his imitation game, which later became known as the Turing test.
In the test, a human interrogator interacts with two players, A and B, by exchanging written messages. If the interrogator cannot determine which player, A or B, is a computer and which is a human, the computer is said to pass the test. The argument is that if a computer is indistinguishable from a human in a general natural language conversation, then it must have reached human-level intelligence.

The Chinese room argument
The idea that intelligence is the same as intelligent behaviour has been challenged by some. The best know counter-argument is John Searle's Chinese Room thought experiment. Searle describes an experiment where a person who doesn't know Chinese is locked in a room. Outside the room is a person who slip notes written in Chinese inside the room through a mail slot. The person inside the room is given a big manual where she can find detailed instructions for responding to the notes she receives from the outside.

Searle argued that even if the person outside the room gets the impression that he is in a conversation with another Chinese-speaking person, the persoin inside the room does not understand Chinese. Likewise, his argument continues, even if a machine behaves in an intelligent manner, for example, by passing the Turing test, it doesn't follow that it is intelligent or that is has a "mind" in the way that a human has. The world "intelligent" can be replaced by the word "conscious" and a similar argument can be made.

How much does philosophy matter in pracice?
The definition of intelligence, natural or artificial, and consciousness appears to be extremely evasive and leads to apparently never-ending discourse. In an intellectual company, this discussion can be quite enjoyable. However, as John McCarthy pointed out, the philosophy of AI is "unlikely to have any more effect on the practice of AI research than philosophy of science generally has on the practice of science."

## Key terminology
**General vs narrow AI** Narrow AI refers to AI that handles one task. Generaly AI, or AGI (artificial general intelligence) refers to a machine that can handle any intellectual task. All the AI methods we use today fall under narrow AI, with general AI being in the realm of science fiction. In fact, the ideal idea of AGI has been all but abandoned by the AI researchers because of lack of progress towards it in more than 50 years despite all the effort. In contrast, narrow AI makes progress in leaps and bouds.

**Strong and weak AI** A related dichotomy is "strong" and "weak" AI. This boils down to the above philosophical distinction between being intelligent and acting intelligently, which was emphasized by Searly. Strong AI would amount to a "mind" that is genuinely intelligent and self-conscious. Weak AI is what we actually have, namely systems that exhibit intelligent behaviours despite being "mere" computers.

## Exercise 4: Definitions, definitions

Which definition of AI do you like best? Perhaps you even have your own definition that is better than all of the others?
Let's first scrutinize the following definitions that have been proposed earlier:
* "cool things that computers can't do"
* machines imitating intelligent human behavior
* autonomous and adaptive systems

**Your task**: Do you thing these are good definitions? Consider each of them in turn and try to come up with things that they get wrong - either things that you think should be counted as AI but aren't according to the definition, or vice versa. Explain your answers by a few sentences per item. 

Also come up with your own, improved definition that fixed the problems with the above candidates. Explain with a few sentences why your definition may be better than the above ones.

**My answer**: The very first definition of "cool things that computers can't do" refers to the inability to make any progress, which, in my opinion, is a very narrow-minded thing to say. What this definition gets wrong is the progress on AI, like neural networks. As the complexity of them increases, so the problems they can solve and accuracy they can achieve. 
The second definition of machines imitating the intelligent human behavior is extremely accurate to what we think of AI is right now.  The same neural networks were built by the inspiration of nature - it resembles the wiring in our brains. In that sense, this definition is extremely accurate.
The last one is autonomous and adaptive systems, which refer to the ability to operate on their own. I would say that this definition lacks complexity because such systems can exist without implementing any complex machine learning algorithms.

My definition would be a mix of a second and third one - autonomous and adaptive machines imitating intelligent human behavior. This definition is complete because it refers to the complexity of human intelligence and constant adaptability and learning process of the system.

## After completing chapter 1 you should be able to:
* Explain autonomy and adaptivity as key concepts for explaining AI
* Distinguish between realistic and unrealistic AI
* Express the basic philosophical problems related to AI including the implications of the Turing test and Chinese room though experiment.
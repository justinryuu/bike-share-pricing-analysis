# Optimizing Bike-Share Pricing in Toronto

# Introduction

With every growing concern of climate change, many cities around the world are shifting their focus to
developing eco-friendly and sustainable communities. One of these goals is to lower the emission of carbon
dioxide, as per the Paris agreement aims to achieve. In an effort to decrease the number of citizens driving
cars on the road, Toronto has implemented a community bike-share system. An individual is able to use
the bike share as a member (paying an annual membership of 99 CAD) or as a casual member (paying 3.25
CAD per ride). Pricing mechanisms are a very good way to change community behavior. As we want to
promote more usage of these bike shares, data on the usage of these bikes will help policymakers decide on
the best pricing mechanism to promote sustainability, while still gaining enough profit for the system to be
self-maintaining.

The dataset we will be using in our analysis today is the Bike Share Toronto Ridership Data from 2017
Quarter 1. In the dataset, there are two variables per user. First, there is trip duration which is the length
of the bike ride in seconds. Secondly, there is user type; this is either member or casual. Members have
annual memberships. Casual users pay per ride.

We are interested in two questions. One, we want to find out how long the average bike-share user rides their
bike. Two, we want to find out the proportion of annual member users among the bike share community.
Answering these questions through analysis is important in optimizing the pricing mechanism for the bike
share system. In the current system, a casual user can ride a bike once for 3.25 CAD for 30 minutes, and
annual users can ride unlimited 30 minutes bike rides all year round. (bikesharetoronto.com) We infer from
our common knowledge that it is likely that annual members ride bikes more often than casual members.
Thus, ideally, to promote the use of the bike share and lower carbon emissions, we want to see a high
proportion of annual members among bike-share users. However, suppose we find from our Question 2
analysis that the proportion of annual members is actually quite low. Then policymakers will be able to
consider perhaps lowering the cost of the annual membership or increasing the cost per ride for casual
members, to incentivize more people to sign up. As you can see, the results from our analysis can directly
lead to a more educated recommendation of the pricing mechanism for the bike share, which is very important
and exciting knowledge for policymakers.

Providing some context, price mechanisms exist in almost every aspect of our lives. It is involved in everything
we purchase and interact with on a daily basis. In our markets driven by capitalism and efficiency, it is difficult
for sustainable initiatives to win over consumers over their less sustainable counterparts. From our failed
attempts at halting climate change, we all know that believing in the good nature and will of people is not
enough. Thus, it is critical for sustainable products and initiatives to be just as attractive as their substitutes
to be adopted by consumers. Obtaining and executing the most optimal commodity price for sustainable
goods is an issue with large global relevance and necessity. Optimal price systems can only be achieved by
observing community behavior and reaction. This is where data analysis becomes a very powerful tool in
figuring out the behavior of the masses and whether or not changes need to be made to get the best results.
Before we get started, let us introduce some terminology. Let us distinguish between a population and
a sample. A population is an entire group that we would like to draw conclusions about.  A
sample is a specific group that we have collected data from. Thus, the sample is always a
subset of the population. The dataset we are working with today is a sample and is not representative of
the population data. This means that the statistics that we extract from our dataset are called sample
statistics, such as the sample mean or sample median. Let’s also go over confidence intervals. A confidence
interval gives an estimated range of values that is likely to include an unknown population parameter, the
estimated range being calculated from a given set of sample data. We will be using bootstrap
analysis to produce these confidence intervals. An explanation of bootstrapping will be covered in the section
METHODS.

We will be analyzing two variables, trip duration in seconds and the proportion of annual members. We
hypothesize that the mean/median (whichever we decide to use) for the trip duration will be about 550
seconds (10 minutes) and that due to the high quality of our data we will be able to get a 95% confidence
interval with a very small interval for the estimation of our population mean/median for trip duration. We
also hypothesize that the proportion of annual members will be quite high (higher than casual members); as
the annual fee for membership is quite low. In addition to that, we also are hopeful that the 95% confidence
interval produced for the proportion of annual members will also have a small interval.

Our data analysis report will be structured as follows. The DATA section will include a description of the
data collection and cleaning process, a description of the important variables we will use in our analysis,
some numerical summaries of our dataset, and some helpful graphical displays of the data we are working
with. The METHODS section will include an explanation of bootstrap sampling and the type we have
used in this analysis, some clarifications on confidence intervals and our parameters of interest, as well as
a detailed outline of our bootstrap analysis. The RESULTS section will include the results of our two
bootstrap sampling analyses as well as its produced confidence intervals. Each will be accompanied by some
explanation of the results and their interpretation. The CONCLUSIONS section will include some refreshers
on our findings and conclusions from our results, some consideration of the drawbacks and limitations of our
analysis, and some recommendations for future steps to go beyond our analysis.

# Data
We will be using the Bike Share Toronto Ridership Data from 2017 Quarter 1. This dataset was obtained
from the opendatatoronto R package. No external data collection was necessary.

Before we start the analysis we did a little bit of data cleaning. First, we loaded the dataset from the
opendatatoronto R package. Secondly, we subsetted the dataset to contain the rider_id, trip duration and
user type. Thirdly, we renamed the column names of the dataset to more intuitive headers. Fourth, we
checked if there were any NA values in the dataset. We found that there were no NA’s so no missing value
cleaning was necessary. Fifthly, we saw that there was an extremely long almost invisible thin tail at the
upper end of our data in trip duration. So we decided to remove the outliers that satisfied the IQR criterion
using the boxplot.stats() function. Lastly, we realized that the original number of observations in the
sample was 127579, which would be very tedious to go through as there are a lot of observations. So we
decided to take a random sample of 1000 observations from our original dataset, so it is easier to work with.

Here is a detailed outline of the important variables in the dataset. (rider_id is the unique identifier for each
observation) 
1. Trip duration: The user’s bike ride length in seconds. Numeric continuous variable. Integer
data type. 
2. User type: The membership type of the user, either “Member” for annual member or “Casual”
for casual member. Categorical nominal variable. String data type.
Here is a glimpse of the dataset, for a better understanding.

## Rows: 1,000
## Columns: 3
## $ rider_id <int> 839156, 842723, 725738, 798191, 852178, 757631, 72242...
## $ trip_duration <int> 1107, 777, 526, 734, 232, 755, 398, 1265, 856, 509, 4...
## $ user_type <chr> "Member", "Member", "Member", "Member", "Member", "Me...

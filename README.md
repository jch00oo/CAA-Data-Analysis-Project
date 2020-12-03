# CAA-Data-Analysis-Project

Our Fall 2020 Data Science Discovery Program research project was conducted under the guidance of the Cal Alumni Association. Our goal is to understand and analyze subscriber engagement in the past year with the CalCon newsletter, a monthly email newsletter for the UC Berkeley alumni community, in order to present actionable insights.

This project was created to not only understand more about the individuals who are subscribed, but also to identify highly-engaged content, grow the size of active subscribers, and sustain overall engagement with the alumni community.

## Table of Contents

1. [Understanding CalCon Engagement (10/2019 - 9/2020)](https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/README.md#understanding-calcon-engagement-102019---92020)
2. [Opens-to-clicks Conversion Rate](https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/README.md#opens-to-clicks-conversion-rate)
3. 

----

## Understanding CalCon Engagement (10/2019 - 9/2020)

In the context of the CalCon newsletter, engagement is segmented in three ways:
1. *Subscribers*: individuals signed up to receive CalCon monthly newsletters
2. *Openers*: subcribers who have opened an email newsletter
3. *Clickers*: Openers who have clicked a URL link

Alumni who are subscribed to the newletter but have not opened or clicked a link in CalCons between October 2019 and September 2020 are defined as *non-engagers*. Roughly 78,641 subscribers fall in this category, with respect to the indicated time period.

Of all subscribers, over half (55%) of those subscribed to CalCon have opened a newsletter between October 2019 and September 2020. A subscriber averages roughly 2.6 opens within the specified time frame; the average number of opens between those who have opened at least once is 4.7 opens.

![Screen Shot 2020-12-02 at 10 49 40 PM](https://user-images.githubusercontent.com/70298391/100982600-18a88680-34fd-11eb-8e43-7210de0eac35.png)

15% of those subscribed to CalCon engaged with a newsletter by clicking between October 2019 and September 2020. A subscriber averages roughly .13 clicks within the specified time frame. The average number of clicks for subscribers who have opened at least once is .54 clicks.

![Screen Shot 2020-12-02 at 11 06 15 PM](https://user-images.githubusercontent.com/70298391/100982676-3bd33600-34fd-11eb-9d6e-153d5c060154.png)

---
## Opens-to-clicks Conversion Rate

The conversion rate between opens to clicks is defined as the number of people who have clicked a link over the number of people who have opened a CalCons newsletter. 

The overall opens-to-clicks conversion rate is 0.23697, indicating that about 24% of people who opened a newsletter in the past year clicked on a link at least once. Analysis on individual subscriber opens-to-clicks conversion rates for those who have opened at least once shows that, on average, a subscriber will click about one link per two opens. 

![Screen Shot 2020-12-02 at 11 32 56 PM](https://user-images.githubusercontent.com/70298391/100982807-59080480-34fd-11eb-9ad2-3d60c7ed5967.png)

After conducting a monthly breakdown of the opens-to-clisks conversion rate, it seems that alumni are generally less likely to click on links during the school year months. We hypothesize that the conversion rate was the highest in December 2019 (20%) because Cal won the Big Game that year; conversion rates were low in April 2020 (2%) because of the coronavirus stay-at-home order.

![Screen Shot 2020-12-02 at 11 47 08 PM](https://user-images.githubusercontent.com/70298391/100982855-69b87a80-34fd-11eb-82a8-85a378199202.png)

---
## Age vs Activity Participation Rate

*Add graph/visualization later*

We divided subscribers into three groups -- young, mid, and old -- based the CAA membership type as well as looking at the distribution of ages and dividing them accordingly:

| Group       | Birth year range  |
| ------------- |:-------------:|
| Young     | 1980 - 2020 |
| Mid      | 1955 - 1979    |
| Old | 0 - 1954    |

For those whose birth years were not available, we used their graduation year to infer their age and put them in the appropriate age group. 

After conducting a series of Kruskal-Wallis tests on each age group's alumni event participation rate versus the population's event participation rate, we've concluded that the old age group had significantly higher participation rates compared to the young and mid age groups, participating in alumni events three times as much as the young age group on average.

---
## Student Activity vs Activity Participation Rate

In order to characterize highly engaged subscribers, we looked at each alumnus' student activity (affiliatio while they were a student) and analyzed whether individuals in certain student activities are more likely to engage in alumni events hosted by CAA.

We began by grouping alumni event participants by their student activity and conducting the Kruskal-Wallis test to compare each group's distribution of participation rates to that of the population of all subscribers.
```python
for i in stud_acts_list:
  curr = stud_act_actCount.loc[stud_act_actCount['student_activity_desc'] == i]
  list1 = curr['counts']
  x, p_val = stats.kruskal(list1, stud_act_actCount['counts'])
  sig_results.append(p_val)
```
Then we used 5% significance level to filter out activities who had significantly higher participation rates:
```python
to_add = list()

for j in sig_results:
  if j <= 0.05:
    to_add.append('yes')
  else:
    to_add.append('no')
``` 
As a result, we've extracted the top 15 student activities with significantly higher participation rates:
| Index         | Student Activity   |
| ------------- |:------------------:|
| 1    | SLCP-Student Participants |
| 2    | Interational House Resident  |
| 3 | Undergrad Research Apprentice Pgm (URAP) |
| 4 | Educational Opportunity Program |
| 5 | Univ Students Co-op Assoc. Resident |
| 6 | Athletic Study Center |
| 7 | Disabled Student Program Alumni |
| 8 | Summer Bridge Program Participant |
| 9 | Tau Beta Pi Engineers' Honors Society |
| 10 | TRSP Transfer Student Services |
| 11 | Berkeley Connect |
| 12 | Biology Scholars Program |
| 13 | PDP Calculus Intensive |
| 14 | UC Extension |
| 15 | Education Abroad Program - France |

Alumni from these student activities, on average, participate in alumni events more frequently, hence CAA can focus on creating events and newsletter content more catered to these groups to further increase engagement and participation.

---
Please put results with visualization under this file

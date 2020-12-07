# CAA Data Analysis Project

Our Fall 2020 Data Science Discovery Program research project was conducted under the guidance of the Cal Alumni Association. Our goal is to understand and analyze subscriber engagement in the past year with the CalCon newsletter, a monthly email newsletter for the UC Berkeley alumni community, in order to present actionable insights.

This project was created to not only understand the individuals who are subscribed, but also to identify highly-engaged content, grow the size of active subscribers, and sustain overall engagement with the alumni community.

We took various approaches to understand the most active subscribers and the most "attractive" content, which we have outlined below through points, and make suggestions to improve the CAA newsletter.

----
## Engagement Among Clickers

We defined active engagement as >= 7 clicks in the past year and repeat clickers as those who clicked on the CalCon Newsletters more than or equal to 7 times in the past 12 months.

Before we look into the entities of all clickers and repeat clickers, we find that there are issues with the data on repeated records on clicks and around 10 percent of missing values of clickers information. 

To clean up the data, we drop duplicates of repeated clicks on the same website within a day and count only the first click as a click for the day. With that, we find that there are roughly 52605 clicks and 25402 unique clickers clicked on the CalCon newsletters at least once in the past year. For the missing values, we infer the birth date of clickers based on the clickers’ "degree 1 year".

![1](https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/1.png)

The average age of repeat clickers is around 63 years old, which indicates that those who actively clicked on the CalCon newsletters in the past year are also seniors and the age of most repeat clickers are greater than 75.

![2](https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/2.png)

We can see that the average number of alumni activity participation of repeat clickers is 9 times in the past 12 months, which is 1.5 times of that of all clickers.

![3](https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/3.png)

Same indication appears for repeat clickers. We can see that improving the clickers’ performance on engaging with the CalCon newsletters can increase their performance on the alumni activity participation.

![4](https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/4.png)

This plot indicates an even higher correlation(=30.8) between the average amount of gift and the number of clicks of repeat clickers than that of all clickers.

---
## Opens-to-clicks Conversion Rate

The conversion rate between opens to clicks is defined as the number of people who have clicked a link over the number of people who have opened a CalCons newsletter. 

The overall opens-to-clicks conversion rate is 0.23697, indicating that about 24% of people who opened a newsletter in the past year clicked on a link at least once. Analysis on individual subscriber opens-to-clicks conversion rates for those who have opened at least once shows that, on average, a subscriber will click about one link per two opens. 

![Screen Shot 2020-12-02 at 11 32 56 PM](https://user-images.githubusercontent.com/70298391/100982807-59080480-34fd-11eb-9ad2-3d60c7ed5967.png)

After conducting a monthly breakdown of the opens-to-clicks conversion rate, it seems that alumni are generally less likely to click on links during the school year months. We hypothesize that the conversion rate was the highest in December 2019 (20%) because Cal won the Big Game that year; conversion rates were low in April 2020 (2%) because of the coronavirus stay-at-home order.

![Screen Shot 2020-12-02 at 11 47 08 PM](https://user-images.githubusercontent.com/70298391/100982855-69b87a80-34fd-11eb-82a8-85a378199202.png)

Overall, the demographic of CalCon's opener engagers seem to be older, with 30 to 40 year olds and 40 to 50 year olds having the highest open rate, indicating this age group (Generation X) defines most of CalCon's engagers.

![Screen Shot 2020-12-03 at 12 27 16 PM](https://user-images.githubusercontent.com/70298391/101084498-e6317480-3562-11eb-8540-c297496f7fba.png)

To further understand clickers engagement, we analyzed the relationship between number of clicks per edition and number of URLs included in each monthly newsletter. We used the code below to extract the total number of links per PDF edition of every monthly CalCons newsletter. [source](https://www.thepythoncode.com/article/extract-pdf-links-with-python)

```python
total_urls = []
for i in newsletters:
  pdf_file = pikepdf.Pdf.open(i)
  urls = []
    # iterate over PDF pages
  for page in pdf_file.pages:
    for annots in page.get("/Annots"):
      uri = annots.get("/A").get("/URI")
      if uri is not None:
          urls.append(uri)
  total_urls.append(len(urls))
  print("[*] Total URLs extracted:", len(urls))
```

There does seem to be a positive relationship between total number of links clicked and URLS included; for every additional URL included, we can expect about 3.5 more clicks for a newsletter.

![Screen Shot 2020-12-03 at 3 18 08 PM](https://user-images.githubusercontent.com/70298391/101100108-cc9c2700-357a-11eb-84c9-66256490a7af.png)

Then, we divided subscribers into three groups -- young, mid, and old -- based the CAA membership type as well as looking at the distribution of ages and dividing them accordingly:

| Group       | Birth year range  |
| ------------- |:-------------:|
| Young     | 1980 - 2020 |
| Mid      | 1955 - 1979    |
| Old | 0 - 1954    |

For those whose birth years were not available, we used their graduation year to infer their age and put them in the appropriate age group. 

After conducting a series of Kruskal-Wallis tests on each age group's alumni event participation rate versus the population's event participation rate, we've concluded that the old age group had significantly higher participation rates compared to the young and mid age groups, participating in alumni events three times as much as the young age group on average.

---
## Effect of link description on clicks in CalCons Newsletter
Calcons Newsletters have multiple links in them. While many have a description under their titles, many don't. We found this distribution of descriptions vs no descriptions to be randomly distributed across all CalCon Newsletters and decided to investigate what it meant on the number of clicks.

We made a scraper using Beautiful Soup that can scrape a url to get its title and description.
```python

title_names = []
for i in left_over_links:
    url = i   
    try:
        # making requests instance 
        reqs = requests.get(url) 

        # using the BeaitifulSoup module 
        soup = BeautifulSoup(reqs.text, 'html.parser') 

        # displaying the title 
	print("Title of the website is : " + title.get_text())
	print("Description of title is : " + title.get_des()) 

        for title in soup.find_all('title'): 
            title_names.append(title.get_text()
	    print(title.get_text())
    except:
        title_names.append(0)
	print("ERROR")
``` 

Looking at the average number of times links in both the categories were clicked, we see that there were 73.6% more clicks on links with a description on average.

<p align="center">
  <img src="https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/Screen%20Shot%202020-12-04%20at%209.52.30%20PM.png" width="450" title="avg # of clicks per category">
</p>

However, averages are prone to problems due to extremes. So I created a boxplot of the distribution of the number of clicks per category. I found that links with a description were really skewed due to a few outliers.

<p align="center">
  <img src="https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/Screen%20Shot%202020-12-04%20at%2010.02.56%20PM.png" width="350" title="Boxplot of Distributions">
</p>

Even by looking at the median, we can see that the links with descriptions had 47.5% more clicks.

<p align="center">
  <img src="https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/Screen%20Shot%202020-12-04%20at%209.52.44%20PM.png" width="450" title="Median # of clicks per category">
</p>

The barplot shows that 60% of the most clicked links for all top clicks/ newsletter issue had a description.

<p align="center">
  <img src="https://github.com/jch00oo/CAA-Data-Analysis-Project/blob/main/Screen%20Shot%202020-12-04%20at%209.58.12%20PM.png" width="350" title="Descriptions in most clicked links per calcons">
</p>

We proposed a hypothesis that links with descriptions get more clicks and conducted a Two Value T-test using scipy.

Null Hypothesis (H0): Having meta descriptions does not affect median number of clicks, 
Alternate Hypothesis (H1): Having meta descriptions matters affects the median number of clicks

```python
from scipy import stats
stats.ttest_ind(vals_1, vals_0)
```
p_value in our case = 0.085 t_stat = 1.729

As 0.085 > 0.05, we fail to reject the null hypothesis at the 95% confidence level.

But, if we take 90% confidence interval, then p-value is 0.1 and then we reject the Null Hypothesis in favor of the Alternate Hypothesis

Due to extreme variance of the values in links with description, we think we get a higher p value. However, we can conclude at a 90% confidence level that there is strong statistical evidence against the Null Hypothesis. Therefore, adding more descriptions to links in newsletters is benefitial and has the ability to increase the median number of clicked links by 47.5%. 

---
## Next Steps

From the insights we have extracted through our analysis, we have made the following suggestions to CAA in order to optimize their content to increase their subscribers' engagement:
* Appeal to older alumni by curating content for them and trying to get more senior subscribers as they are much more likely to participate in events and donate
* Write a description under every link as it has the potential to increase the probability of that link being clicked.

However, there is still analysis left to do to solidify our findings and identify more areas for improvement:
* Conduct NLP analysis on the "Feature Benefits" sections of each newsletter
* Access data regarding when people subscribed/unsubscribed to the newsletter
* Classify the content for links with description vs those without a description to see if people tend to generally click less on a particular type of content

We believe the insights and suggestions we have extracted will increase alumni engagement with the newsletters and events as well as donations, allowing long term success for the future operations of CAA.

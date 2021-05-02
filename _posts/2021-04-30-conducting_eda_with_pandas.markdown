---
layout: post
title:      "Conducting EDA with pandas"
date:       2021-04-30 17:29:30 -0400
permalink:  conducting_eda_with_pandas
---

*Exploratory data analysis (aka EDA) is a critical part of the analysis process because it is where a data scientist will get a grasp on the data they will be working with, have the opportunity to clean up datasets, and begin performing so simple analyses.*

*Using concrete, real-life examples along the way, this post will serve as a brief tutorial of the EDA process, harnessing the power and simplicity of the pandas library to manipulate and clean data.*

## Background
Let's say you work as a data scientist for an insurance broker in Florida that deals primarily with personal insurance products. The company has noticed that while it does a good job selling a person both home and auto insurance, it isn't doing very well with umbrella policies (for more information on what an umbrella policy is, [click here](https://www.investopedia.com/articles/personal-finance/040115/how-umbrella-insurance-works.asp)!) Umbrella policies are usually a good idea for people with both home and auto insurance, so your company decides it wants to focus on selling this for the next few months. You are tasked with creating a list of eligible clients that can be given to each employee, so they can try to cross-sell to their clients.

## Getting Acclimated
Now that you have been presented with the project, it's time to get started! The very first step is finding the appropriate datasets to work with. You may not always be given data to work with, so it's up to you to gather what you need. Luckily, you have identified two datasets that you believe contain sufficient information for this project: *clientList*, which contains a list of the active clients, and *policyList*, which contains current policies. Let's go ahead and take a look at each!

We will start by loading our clients into the variable ```clients``` and previewing the results.

```
clients = pd.read_csv('clientList.csv')
clients.head()
```

![](https://i.ibb.co/WgdQY6V/Screen-Shot-2021-04-30-at-5-17-19-PM.png)

Let's do the exact same thing for our policies by loading them into the variable ```policies``` and previewing the results.

```policies = pd.read_csv('policyList.csv')
policies.head()
```

![](https://i.ibb.co/RhfbXFb/Screen-Shot-2021-04-30-at-5-32-58-PM.png)

Right off the bat, we can see that our two dataframes contain different information, but also contain the shared colum ```clientID```, a unique identifier for the clients. 

*Our data is in pretty good shape, but this is usually not the case. Often times, you will run into rows containing NaN values, columns with formatting issues, etc. It's up to you to determine the best course of action to clean up the data. For example, with numeric data you can attempt to fill in NaN using the columns mean or median, or choose to drop those rows completely if they represent a small enough sample. Afterall, you cannot perform math on with NaN values. *

Now that we know what is in each dataframe and how they are related, we can begin move onto our next step, which will be joining our datasets!

## Joining/merging dataframes
Joinging and merging are two methods you can use in pandas to combine data from two seperate datafrmes into one. Both are very similar, but with two distinctions:
1.  ```merge``` is generally used when you want to merge two dataframes on a column(s) that are not the index - hence, you can choose what you merge on.
2.  ```join``` is not as flexible, and typically is used to merge on the index (though you can technically specify other columns to join on).

You can read more about joining and merging [here](https://towardsdatascience.com/pandas-join-vs-merge-c365fd4fbf49#:~:text=We%20can%20use%20join%20and,join%20on%20for%20both%20dataframes.)!

Let's merge our two dataframes on the ```clientID``` column and see the results.

```
merged = clients.merge(policies, on='clientID')
merged.head()
```

![](https://i.ibb.co/VJwHv52/Screen-Shot-2021-04-30-at-6-12-17-PM.png)

This is a great start, but notice that we now have created a larger dataframe; the client information has been duplicated to pair with each policy a client has. Jane Doe is listed three times because she has three policies with your company. We want to have something like this:

![](https://i.ibb.co/G904phy/Screen-Shot-2021-04-30-at-6-28-31-PM.png)

As you can see, each client is listed once and there is a column denoting all the policies they have. We will over again and use some neat, built-in pandas methods to create a dataframe that will be helpful for us.

## Data manipulation
We are looking to create a row with all the policy types for each client. `.apply()` is a great method that allows you to apply functions to a series within a dataframe in a streamlined, compact manner. This can roughly be related to using `for` loops, but that would be a much more cumbersome and lengthy to create.

We will start by grouping our policies dataframe by `clientID`; in order to "group" the policies together, we can pass an easy `lambda` function that will join all the policies a particular client has together with a comma. 

We will create a new pandas dataframe from this and reset the index.

```
policies_grouped = pd.DataFrame(policies.groupby('clientID')['policyType'].apply(lambda x: ', '.join(x)))
policies_grouped.reset_index(inplace=True)
policies_grouped.head()
```

![](https://i.ibb.co/nPHb5fg/Screen-Shot-2021-04-30-at-6-45-39-PM.png)

Let's rename the `policyType` header to something a bit more useful:
`policies_grouped.rename(columns={'policyType': 'policies'}, inplace=True)`

**Note: `inplace=True` is a great way to bypass having to assign your code to a variable; your changes will be applied inplace.**

Finally, let's merge `policies_grouped` with our `clients` dataframe on `clientID`.

```
clientPolicies = clients.merge(policies_grouped, on='clientID')
clientPolicies
```

![](https://i.ibb.co/rwXd6TL/Screen-Shot-2021-04-30-at-7-01-18-PM.png)

It's looking great! We now have a dataframe that includes all of our clients and their contact information along with all the policies they currently have with us. Let's first filter our dataframe so we only have to concern ourselves with individuals that have home and auto insurance with us; these are the individuals we want to sell umbrella policies to.

To accomplish this, we can create a list of the policies that we want a person to have, and search each row in the `policies` column to find out if both policies are in that row. The trick here is to utilize the numpy library. We will create a list of all the individuals that meet our criteria, and then turn that list into an array, and back into a dataframe! The nice part is we can effectively carry all of this out in three, short lines of code.

```
to_filter = ['Home', 'Auto']
filtered_list = [clientPolicies['policies'].str.contains(i) for i in to_filter]
filtered_policies = pd.DataFrame(clientPolicies[np.all(filtered_list, axis=0)])
filtered_policies
```

![](https://i.ibb.co/Y3FYgMx/Screen-Shot-2021-05-01-at-2-57-50-PM.png)

We can see that only clients that have home and auto insurance with us are in our new dataframe. This is almost exactly what we want. However, there are two people in this dataframe that already have umbrella policies. To remove them, we can perform a similar operation as above, but in just one line.

We will make use of the `~` operator, which is the inverse operator.
`filtered_policies = filtered_policies.loc[~filtered_policies['policies'].str.contains('Umbrella')]`

Essentially, we are finding all policies that have "Umbrella", but the inverse operator excludes them from the final dataframe.

![](https://i.ibb.co/B41v5wk/Screen-Shot-2021-05-01-at-3-07-45-PM.png)

And, viola! We now have a dataframe that has been filtered and meets our objective!

## Final remarks

Pandas is a very powerful, simple library that makes data analysis efficient. Through this example, we saw how we can use pandas to explore, merge, and manipulate data to answer questions. This is just the tip of the iceberg; pandas has a lot of different methods and capabilities that can be used to make data analysis easier.

For more information, visit the pandas documentation ![here](https://pandas.pydata.org/docs/).



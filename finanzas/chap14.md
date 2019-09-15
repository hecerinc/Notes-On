# Chapter 14
## Planning for Retirement


## Overview

- No financial goal is more important than achieving a comfortable standard of living in retirement.

1. The first step is to **set retirement goals for yourself**.
	- Describe what you want to do in retirement, the level of income you'd like to receive, and any special retirement goals you may have (buying a home in Florida or an around-the-world cruise).
1. Establish the size of the _nest egg_ that you're going to need to achieve your retirement goals.
	- Formulate an _investment program_ that enables you to build up your nest egg.
	- This involves (1) creating systematic savings plan and (2) identifying the types of investments that will best meet your retirement needs.
	- Investments and investment planning are the vehicles for building up your retirement funds.
	- Tax planning is also important because a major objective of sound retirement planning is to legitimately shield as much income as possible from taxes, and, in so doing, maximize the accumulation of retirement funds.


### 3 biggest pitfalls of retirement planning

> People often get carried away with the amount of money they want to build up for retirement. Having a nest egg of $4 million or $5 million would be great, but it's really beyond the reach of most people. Besides, you don't need that much to live comfortably in retirement.

3 big mistakes:

1. Starting too late
1. Putting away too little.
1. Investing too conservatively.

- Start with a default amount of 15% of your pre-tax income and go from there.
- Many people tend to be far too conservative in investing retirement money. They place way too much of their retirement money into _low-yielding_, fixed-income securities such as CDs and Treasury notes. Although you should never speculate with your retirement plan, there's no need to aovid risk altogether.
- Note that it is _the combination of these three factors_ that determines the amount you'll have at retirement. 
- This means there are _several ways of getting roughly the same reuslt_. Knowing the size of the nest egg you'd like to end up with, you can pick the combination of variables (period of accumulation, annual contribution, rate of return) that you're most comfortable with.

### Estimating income needs

2 strategies:

1. **Plan for retirmeent over a series of short-run time frames.** A good way to do this is to state your retirement income objectives as a percentage of your present earnings. For example, if you want a retirement income equal to 80% of your final take-home pay, then you can determine the amount necessary to fund this need. Then, every 3 to 5 years, you can revise and update your plan.
1. **Long-term approach**. Estimate the level of income you'd like to receive in retirement; along with the amount of funds you must amass to achieve that desired standard of living. Of course, if conditions or expectations should change dramatically in the future, then it may be necessary to make corresponding alterations to your long-run retirement goals and strategies.

![Effect of inflation on future retirement needs](img/inflation_retirement.png)


![Worksheet 14.1](img/ws141.png)


#### 1. Determine future retirement needs

**Determine your expenses when you retire.**

- In your 30s, two children and an annual income of 80K before taxes.
- Likely household expenditures estimated to be 60-70% of your current expenditures (kids don't live with you anymore, you already own your home, etc)

In the example, the Pendletons' current expenditures are 56K (this information is readily obtained from the **Income & Expense Statement**). At 70%, their estimated retirement expenditures are (`56*.7 =`) $39,200 **in today's dollars**.



#### 2. Estimating retirement income

**Where you will get the money to meet your projected expenses.**

E.g.: 

- Social security
- Employer's pension

Make this estimate in **today's dollars**, and then obtain the **future value**.

> The next question is: Where will the Pendletons get the money to meet their projected household expenses of $39,200 a year? They've addressed this problem by estimating what their income will be in retirement—again based on today's dollars. Their two basic sources of retirement income are Social Security and employer-sponsored pension plans. They estimate that they'll receive about $24,000 a year from Social Security (as we'll see later in this chapter, you can obtain an estimate directly from the Social Security Administration of what your future Social Security benefits are likely to be when you retire) and another $9,000 from their employer pension plans, for a total projected annual income of $33,000. When comparing this figure to their projected household expenditures, it's clear the Pendletons will be facing an annual shortfall of $6,200 (see steps E through I in Worksheet 14.1). This is the amount of additional retirement income they must come up with; otherwise, they'll have to reduce their standard of living in retirement. 

> At this point, we need to introduce the inflation factor to our projections in order to put the annual shortfall of $6,200 in terms of retirement dollars. Here, we assume that both income and expenditures will undergo approximately the same average annual rate of inflation, which will cause the shortfall to grow by that rate over time. In essence, 30 years from now, the annual shortfall is going to amount to a lot more than $6,200. How large this number becomes will, of course, depend on what happens to inflation. Assume that the Pendletons expect inflation over the next 30 years to average 5 percent. While that's a bit on the high side by today's standards, the Pendletons are concerned that the ballooning of the federal deficit in response to the financial crisis of 2007-2009 will cause inflation to rise over the long term. Using the compound value table from Appendix A, we find that the inflation factor for 5 percent and 30 years is 4.322. Multiplying this inflation factor by the annual shortfall of $6,200 gives the Pendletons an idea of what that figure will be by the time they retire: $6,200 x 4.322 = $26,796 or nearly $27,000 a year (see steps J to L in Worksheet 14.1). Thus, based on their projections, the shortfall should amount to about $26,796 a year when they retire 30 years from now. _This is the amount they'll have to come up with through their own supplemental retire-ment program_. 


#### 3. Funding a projected shortfall

Determine:

1. **How big your retirement nest egg must be to cover the projected annual income shortfall**
1. **How much to save each year to accumulate the required amount by the time they retire.**


This process assumes that you cover the shortfall by accumulating an amount at retirement age and investing that amount at a projected rate. The earnings on this investment represent the shortfall you're trying to cover.

> Let's assume that this rate of return is estimated at 8 percent, in which case the Pendletons should accumulate $334,950 by retirement to cover the projected shortfall. This figure is found by capitalizing the estimated shortfall of $26,796 at an 8 percent rate of return: $26,796/0.08 = $334,950 (see steps M and N). Given an 8 percent rate of return, such a nest egg will yield $26,796 a year: $334,950 x 0.08 = $26,796. So long as the capital ($334,950) remains untouched, it will generate the same amount of annual income for as long as the Pendletons live and can eventually become a part of their estate. 

Now you calculate how much you need to save _each year_ to get to the amount you're going to invest at retirement.

This is called a "Sinking Fund Payment". The formula in Excel is `=PMT(Rate, NPER, Present Value, Future Value)`.

> For the Monthly Investment (with no up-front lump sum), you would put the monthly investment as the payment, and 0 for the Present Value. The Future Value is still the same. 


(Taken from https://money.stackexchange.com/questions/26368/whats-the-formula-to-calculate-the-monthly-or-lump-sum-investment-amount-for-a)

> To find out how much must be saved each year to achieve a targeted sum in the future, we can use the table of annuity factors in Appendix B. The appropriate interest factor depends on the rate of return one expects to generate and the length of the investment period. In the Pendletons' case, there are 30 years to go until retirement, meaning that the length of their investment period is 30 years. Suppose that they believe they can earn a 6 percent average rate of return on their investments over this 30-year period. From Appendix B, we see that the 6 percent, 30-year interest factor is 79.058. Because the Pendletons must accumulate $334,950 by the time they retire, the amount they'll have to save each year (over the next 30 years) can be found by dividing the amount they need to accumulate by the appropriate interest factor; that is, $334,950 79.058 = $4,237 (see steps 0 to Q in Worksheet 14.1). 


In Excel:

```
=PMT(0.06,30,0, 334950) # -> ($4,236.75)
```

The Pendletons now know what they must do to achieve the kind of retirement they want: _Put away $4,237 a year and invest it at an average annual rate of 6% over the next 30 years_.


**The procedure done here is a bit simplified.** One important simplifying assumption in the procedure is that it ignores the income that can be derived from the _sale of a house_. This offers special tax features and can generate a significant amount of cash flow.

When inflation occurs, it is likely to drive up home prices right along with the cost of everything else. Rather than trying to factor in the cash flow of selling a home into the forecast of retirement income and needs, we suggest that you _recognise_ the existence of this cash-flow source in your retirement planning and consider it as a cushion against all the uncertainty inherent in retirement planning projections.


### Online retirement planning

- Helpful tools at quicken.com and bloomberg.com

You answer a few questions about expected inflation, desired rate of return on investments, and current levels of income and expenditures. Then the online app determines the size of any income shortfall, the amount of retirement funds that must be accumulated over time, and different ways to achieve the desired retirement nest egg. 

**An attractive feature of most of these internet sites is the ability to run through "what-if" excercises easily**. 

![Sources of income for average retiree](img/e142.jpg)

![Behaviour matters: bias in retirement planning](img/bm_retirement.jpg)

## Social Security

Social Security Act of 1935.

### Coverage

Coverage today extends to all gainfully employed workers. Two major classes of employees are now exempt from _mandatory_ participation in the SS system: 

1. federal _civilian_ employees who were hired before 1984 and are covered under the Civil Service Retirement System
2. employees of state and local governments who have chosen not to be covered (although most are covered through the _voluntary participation_)

Certain positions like newspaper carriers under age 18 and full-time college students working for a university are also exempt.

- To qualify for benefits, nearly all workers today must be employed in a job covered by SS for **at least 40 quarters**, or 10 years.
- The quarters _need not be consecutive_.
- Once the 40-quarter retirement is met, the worker becomes fully insured and remains eligible for retirement payments even if he or she never works again in covered employment.
- The surviving spouse and dependent children of a _deceased worker_ are also eligible for monthly benefits if the worker was fully insured at the time of death or, in some special cases, if certain other requirements are met.

### Payroll taxes

Check: https://tax.thomsonreuters.com/news/social-security-wage-base-increases-to-132900-for-2019/

The cash benefits provided by SS are derived from the payroll (FICA) taxes paid by covered employees and their employers. 

- Tax rate in 2015 for employees was 6.2% of SS and 1.45% for Medicare (total **7.65%**) (Figures were the same in 2019)
- Rates are different for self-employed people (twice as much. yikes!)
- Social security tax == Old Age, Survivors, and Disability Insurance (OASDI)
- Hospital Insurance == Medicare tax


These rates are only applicable to the first $132,900 in wages for SS and $200K for Medicare. This is known as a maximum **wage base**, and it increases each year (in 2015 this was $118,500).

### Retirement benefits

Two main categories:

1. old-age benefits
1. survivor's benefits

#### Retirement benefits

Workers who are fully covered may receive retirement benefits for life once they reach full retirement age. 

- For anyone born in **1960 or later**: SS defines retirement age as **age 67**.
- Early retirement: **age 62** receive partial benefits (70 to 80%) of the full amount (depending on when you were born)


If the retiree has a spouse age 67 or older, the spouse is entitled to benefits equal to 1/2 of the amount received by the retired worker and can also file for early receipt of reduced benefits at age 62.

Social Security should be viewed as a **foundation for your retirement income**. By itself, **it's insufficient to enable a worker and spouse to maintain their preretirement standard of living**.

In two-income families, both the husband and wife may be eligible for SS benefits. These can be chosen in one of two ways:

1. take the full benefits to which each is entitle from his or her account
1. take the husband and wife benefits of the higher-paid spouse.

If each receives their full share, no spousal benefits are given. If they take the husand and wife benefits of the higher-paid spouse, they effectively receive 1.5 shares.


#### Survivor's benefits

If the covered worker dies, the spouse can receive survivor's benefits from SS.

Benefits include:

- small lump payment of several hundred dollars
- followed by monthly benefit checks

To be eligible for monthly payments, the surviving spouse must generally be at least 60 years of age or have a dependent and unmarried child of the deceased worker in his or her care.

To qualify for _full_ benefits, the spouse must be >= 67. Reduced benefits are payable between ages 60 and 67.

If the children of a deceased worker reach age 16 before the spouse reaches age 60, the monthly benefits cease and do not resume until the spouse turns 60. This period during which survivor's benefits are not paid is sometimes called the _widow's gap_.


### Monthly SS benefits

The amount of SS benefits to which an eligible person is entitled to is set by law and defined according to a fairly complex formula.

Your **Social Security Statement** (available at ssa.gov) lists the year-by-ear Social Security earnings you've been credited with and shows (in today's dollars) what benefits you can expect under three scenarios:

1. if you retire at age 62 and receive 70-80% of the full benefit
2. the full benefit at age 65 to 67 (depends on your year of birth)
3. the increased benefit (of up to 8% per year) that's available if you delay retirement until age 70.

The statement also estimates what your children and surviving spoues would get if you die and how much you'd receive monthly if you became disabled.


![Financial Planning Tips: Depending on SS](img/fpt_retirement.png)

#### Range of benefits

Exhibit 14.3 shows the current average level of benefits (for someone who retired in 2015). Keep in mind that the figures given in the exhibit represent amounts that the beneficiaries will receive in their _first year_ of retirement. Those amounts will be adjusted upward each year with subsequent increases in the cost of living.

![Average monthly SS benefits paid in 2015](img/e143.png)

Note that the average benefits reflect the fact that they _may be reduced_ if the SS recipient is _under age 67 and still gainfully employed_ &mdash; perhaps in a part-time job.

Retirees aged 62 to 66 are subject to an _earnings test_ that effectively limits the amount of income they can earn before they start losing some (or all) of their SS benefits.

In 2015 the limit was $15,720 per year (the limit rises annually with wage inflation). In 2019 this was $17,640. You lose $1 in benefits for every $2 you earn above the limit.

This **only applies to early retirees**. Once you reach full retirement age you can earn as much as you like. This also only applies on earned income through wages/salaries. Does not apply to interest, dividends, rents or profits from securities transactions.


![Get a rough estimate of your future social security benefits](img/ycdin_retirement.png)

#### Taxes on benefits

Even though SS contributions are made in after-tax dollars, you may actually have to pay taxes (again) on at least some of your SS benefits.

SS retirement benefits are subject to federal income taxes if the beneficiary's annual income exceeds one of the following base amounts: $25K for a single taxpayer, $32K for married taxpayers filing jointly, and 0 for married taxpayers filing separately.

Determine the amount of income that must be counted:

- Start with your adjusted gross income (AGI) as defined by current tax law
- Add all nontaxable interest income (e.g. income from municipal bonds) + a stipulated portion of the SS benefits received. 


Continue on page 571.


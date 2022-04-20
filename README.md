# Loan-Repayment-Challenge
Advanced Analytics and model built for predicting the loan risk or quality (loan repayment) on a given applicant.

The following challenge asks you to work with a data set of loan repayment. It is intentionally meant to be open ended. The
point is not to arrive at a predetermined answer or search for the lowest possible standard error. Rather, the hope
is that it will force you to ask relevant questions about the data, do some preliminary exploration, perform the
necessary manipulations or aggregations, generate visualizations, and reach conclusions or insights.

You are provided with 3 files: loan.csv, payment.csv and clarity_underwriting_variables.csv. The files are
comma separated, with the column names in the first row.

Data Scientist Assessment Data Dictionary:

1. loan.csv:
In this file there are 18 columns:
1. loanId:
    a. This is a unique loan identifier. Use this for joins with the payment.csv file
2. anon_ssn:
    a. This is a hash based on a client’s ssn. You can use this as if it is an ssn to compare if a
    loan belongs to a previous customer.
3. payFrequency:
    a. This column represents repayment frequency of the loan:
      i. B is biweekly payments
      ii. I is irregular
      iii. M is monthly
      iv. S is semi monthly
      v. W is weekly
4. apr:
    a. This is the loan apr %
5. applicationDate:
a. Date of application (start date)
6. originated:
a. Whether or not a loan has been originated (first step of underwriting before loan is
funded)
7. originatedDate:
a. Date of origination- day the loan was originated
8. nPaidOff:
a. How many MoneyLion loans this client has paid off in the past
9. approved:
a. Whether or not a loan has been approved (final step of underwriting before a loan
deposit is attempted)
10. isFunded:
a. Whether or not a loan is ultimately funded. –a loan can be voided by a customer
shortly after it is approved, so not all approved loans are ultimately funded.
11. loanStatus:
a. Current loan status. Most are self explanatory. Below are the statuses which need
clarification:
i. Returned Item: missed 1 payment (but not more), due to insufficient funds
ii. Rejected: Rejected by automated underwriting rules – not by human
underwriters
iii. Withdrawn Application – application abandoned for more than 2 weeks, or is
withdrawn by a human underwriter or customer

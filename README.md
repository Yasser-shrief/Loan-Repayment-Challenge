# Loan-Repayment-Challenge
Advanced Analytics and model built for predicting the loan risk or quality (loan repayment) on a given applicant.

The following challenge asks you to work with a data set of loan repayment. It is intentionally meant to be open ended. The
point is not to arrive at a predetermined answer or search for the lowest possible standard error. Rather, the hope
is that it will force you to ask relevant questions about the data, do some preliminary exploration, perform the
necessary manipulations or aggregations, generate visualizations, and reach conclusions or insights.

You are provided with 3 files: loan.csv, payment.csv and clarity_underwriting_variables.csv. The files are
comma separated, with the column names in the first row.

* **Language :** Python
* **Classification model :** LigthGBM
* **evaluation metrics :** AUC -validation set- = 0.99 

Data Scientist Assessment Data Dictionary:

## 1. loan.csv:<br>
In this file there are 18 columns:<br>
1. loanId:<br>
    a. This is a unique loan identifier. Use this for joins with the payment.csv file
2. anon_ssn:<br>
    a. This is a hash based on a client’s ssn. You can use this as if it is an ssn to compare if a
    loan belongs to a previous customer.
3. payFrequency:<br>
    a. This column represents repayment frequency of the loan:
      i. B is biweekly payments<br>
      ii. I is irregular<br>
      iii. M is monthly<br>
      iv. S is semi monthly<br>
      v. W is weekly<br>
4. apr:<br>
    a. This is the loan apr %<br>
5. applicationDate:<br>
    a. Date of application (start date)<br>
6. originated:<br>
    a. Whether or not a loan has been originated (first step of underwriting before loan is
    funded)<br>
7. originatedDate:<br>
    a. Date of origination- day the loan was originated
8. nPaidOff:<br>
    a. How many MoneyLion loans this client has paid off in the past<br>
9. approved:<br>
    a. Whether or not a loan has been approved (final step of underwriting before a loan<br>
        deposit is attempted)
10. isFunded:<br>
    a. Whether or not a loan is ultimately funded. –a loan can be voided by a customer
    shortly after it is approved, so not all approved loans are ultimately funded.
11. loanStatus:<br>
    a. Current loan status. Most are self explanatory. Below are the statuses which need
    clarification:<br>
        i. Returned Item: missed 1 payment (but not more), due to insufficient funds<br>
        ii. Rejected: Rejected by automated underwriting rules – not by human
        underwriters<br>
        iii. Withdrawn Application – application abandoned for more than 2 weeks, or is
        withdrawn by a human underwriter or customer<br>
        iv. Statuses with the word “void” in them mean a loan that is approved but
        cancelled. (One reason is the loan failed to be debited into the customer’s
         account).<br>
12. loanAmount:<br>
    a. Principal of loan – for non-funded loans this will be the principal in the loan
    application
13. originallyScheduledPaymentAmount:<br>
    a. This is the originally scheduled repayment amount (if a customer pays off all his
    scheduled payments, this is the amount we should receive)<br>
14. state<br>
    a. Client’s state<br>
15. Lead type<br>
    a. The lead type determines the underwriting rules for a lead.<br>
    i. bvMandatory: leads that are bought from the ping tree – required to perform<br>
    bank verification before loan approval
    ii. lead: very similar to bvMandatory, except bank verification is optional for
    loan approval<br>
    iii. california: similar to (ii), but optimized for California lending rules<br>
    iv. organic: customers that came through the MoneyLion website<br>
    v. rc_returning: customers who have at least 1 paid off loan in another loan
    portfolio. (The first paid off loan is not in this data set).<br>
    vi. prescreen: preselected customers who have been offered a loan through
    direct mail campaigns<br>
    vii. express: promotional “express” loans<br>
    viii. repeat: promotional loans offered through sms<br>
    ix. instant-offer: promotional “instant-offer” loans<br>
16. Lead cost<br>
    a. Cost of the lead<br>
17. fpStatus<br>
    a. Result of the first payment of the loan:<br>
    i. Checked – payment is successful<br>
    ii. Rejected – payment is unsuccessful<br>
    iii. Cancelled – payment is cancelled<br>
    iv. No Payments/No Schedule – loan is not funded<br>
    v. Pending – ACH attempt has been submitted to clearing house but no
    response yet<br>
    vi. Skipped – payment has been skipped<br>
    vii. None – No ACH attempt has been made yet – usually because the payment is
    scheduled for the future<br>
18. clarityFraudId:<br>
    a. unique underwriting id. Can be used to join with columns in the
    clarity_underwriting_variables.csv file  <br>
    
    Every row represents an accepted loan application/ successfully funded loan.

## 2. payment.csv:<br>
9 columns in this file:<br>
1. loanId:<br>
    a. This is a unique loan identifier. Use this for joins with the loan.csv file<br>
2. isCollection<br>
    a. A loan can have a custom made collection plan if the customer has trouble making
    repayments as per the original schedule. TRUE means the payment is from a custom
    made collection plan.<br>
3. installmentIndex<br>
    a. This counts the nth payment for the loan. First payment is 1, 2nd payment is 2 and so
on.<br>
    b. This index resets for collection payment plans. So some loans can have 2 payments
    with the same installmentIndex. One from the regular plan and one from the
    collection plan.<br>
4. paymentdate<br>
    a. Effective of payment<br>
5. prinicpal<br>
    a. principal component of the payment<br>
6. fees<br>
    a. Fee/ interest amount of the payment<br>
7. paymentAmount<br>
    a. Total amount of the payment<br>
    b. Usually equals to fees + principal<br>
8. paymentStatus<br>
    a. Checked – payment is successful<br>
    b. Rejected – payment is unsuccessful<br>
    c. Cancelled – payment is cancelled<br>
    d. Pending – ACH attempt has been submitted to clearing house but no response yet<br>
    e. Skipped – payment has been skipped<br>
    f. None – No ACH attempt has been made yet – usually because the payment is
    scheduled for the future<br>
    g. Rejected awaiting retry – retrying a failed ACH attempt.<br>
9. paymentReturnCode: these are ACH error codes to explain why the payment failed. You can
find more information about this at the end of this document, or visit the following link:
a. https://www.vericheck.com/ach-return-codes/

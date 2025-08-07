# SQL_Bank-Loan-Analysis
One of the primary purposes of analysing loan data is to assess the risk associated with lending to a particular individual or business. Banks use data to evaluate the creditworthiness of borrowers, predict default probabilities, and determine interest rates and lending terms.
# The below questions were answers using SQl queries:

```sql
--Q1) Total Loan Application
SELECT
	COUNT(id) AS total_loan_applications
FROM
	bank_loan_data
--Total loan application in the month of December (MTD loan applications)
SELECT
	COUNT(id) AS total_loan_applications
FROM
	bank_loan_data
WHERE
	MONTH(issue_date) = 12

--Total loan applications in the month of November (Previous MTD loan applications)
SELECT
	COUNT(id) AS total_loan_applications
FROM
	bank_loan_data
WHERE
	MONTH(issue_date) = 11

--Q2) Total Funded Amount
SELECT
	SUM(loan_amount) AS funded_amount
FROM
	bank_loan_data

--Total Funded Amount in the month of December (MTD total funded amount)
SELECT
	SUM(loan_amount) AS funded_amount
FROM
	bank_loan_data
WHERE
	MONTH(issue_date) = 12

--Total Funded Amount in the month of December (Previous MTD total funded amount)
SELECT
	SUM(loan_amount) AS funded_amount
FROM
	bank_loan_data
WHERE
	MONTH(issue_date) = 11

--Q3) Total Amount Received
SELECT
	SUM(total_payment) AS loan_amount_received
FROM
	bank_loan_data

	--MTD total amount received
	SELECT
		SUM(total_payment) AS loan_amount_received
	FROM
		bank_loan_data
	WHERE
		MONTH(issue_date) = 12

	--Previous MTD loan amounts received
	SELECT
		SUM(total_payment) AS loan_amount_received
	FROM
		bank_loan_data
	WHERE
		MONTH(issue_date) = 11

	--Difference between funded amount and the loan amounts received
		SELECT
			SUM(loan_amount) AS funded_amount,
			SUM(total_payment) AS loan_amount_received,
			SUM(loan_amount) - SUM(total_payment) AS Difference
		FROM
			bank_loan_data

--Q4) Average Interest Rate?
SELECT
	AVG(int_rate) * 100 AS avg_interest_rate
FROM
	bank_loan_data

--MTD avg interest rate
	SELECT
		AVG(int_rate) * 100 AS avg_interest_rate
	FROM
		bank_loan_data
	WHERE
		MONTH(issue_date) = 12

--Previous MTD avg interest rate
	SELECT
		AVG(int_rate) * 100 AS avg_interest_rate
	FROM
		bank_loan_data
	WHERE
		MONTH(issue_date) = 11

--Q5) Average debt-to-income (DTI) ratio?
SELECT
	AVG(dti) * 100 AS avg_dti
FROM
	bank_loan_data

--MTD avg DTI ratio
SELECT
	AVG(dti) * 100 AS avg_dti
FROM
	bank_loan_data
WHERE
	MONTH(issue_date) = 12

--Previous MTD avg DTI ratio
SELECT
	AVG(dti) * 100 AS avg_dti
FROM
	bank_loan_data
WHERE
	MONTH(issue_date) = 11

--Q6) Good loan percentage
SELECT
	(COUNT(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN id END) * 100) /
	COUNT(id) AS Good_loan_Percentag
FROM
	bank_loan_data

--Q7) Good loan applications
SELECT
	COUNT(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN id END) AS number_of_good_loans
FROM
	bank_loan_data

--Q8) Good loan funded amount
SELECT
	SUM(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN loan_amount END) as good_loan_funded_amount
FROM
	bank_loan_data

--Q9) Good loan total received amount
SELECT
	SUM(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN total_payment END) AS good_loan_amount_received
FROM
	bank_loan_data

--Q10) Bad loan application percentage
SELECT
	(COUNT(CASE WHEN loan_status = 'Charged Off' THEN id END) * 100) /
	COUNT(id) AS bad_loan_percentage
FROM
	bank_loan_data

--Q11) Bad loan applications
SELECT
	COUNT(CASE WHEN loan_status = 'Charged Off' THEN id END) AS bad_loan_applications
FROM
	bank_loan_data

--Q12) Bad loan funded amount
SELECT
	SUM(CASE WHEN loan_status = 'Charged Off' THEN loan_amount END) AS bad_loan_funded_amount
FROM
	bank_loan_data

--Q13) Bad loan total received amount
SELECT
	SUM(CASE WHEN loan_status = 'Charged Off' THEN total_payment END) AS bad_loan_received_amount
FROM
	bank_loan_data

--Q14) Summary Grid of the loan performance


SELECT
	loan_status,
	COUNT(id) AS loan_application,
	SUM(total_payment) AS total_amount_received,
	SUM(loan_amount) AS total_funded_amount,
	AVG(int_rate) * 100 AS avg_interest_rate,
	AVG(dti) * 100 AS avg_dti
FROM
	bank_loan_data
GROUP BY
	loan_status

--Q15) Monthly trend by issue date
SELECT
	MONTH(issue_date) AS month_number,
	DATENAME(MONTH, issue_date) AS month_name,
	COUNT(id) AS number_of_loan_applications,
	SUM(loan_amount) AS total_funded_amount,
	SUM(total_payment) AS total_amount_received
FROM
	bank_loan_data
GROUP BY
	MONTH(issue_date),
	DATENAME(MONTH, issue_date)
ORDER BY
	MONTH(issue_date) ASC

--Q16) Regional analysis
SELECT
	address_state AS state,
	COUNT(id) AS total_loan_applications,
	SUM(loan_amount) AS total_funded_amount,
	SUM(total_payment) AS total_amount_received
FROM
	bank_loan_data
GROUP BY
	address_state
ORDER BY
	address_state ASC

--Q17) Loan Term Analysis
SELECT * FROM bank_loan_data

SELECT
	term AS legth_of_term,
	COUNT(id) AS total_number_of_applications,
	SUM(loan_amount) AS total_funded_amount,
	SUM(total_payment) AS total_amount_received
FROM
	bank_loan_data
GROUP BY
	term

--Q18) Employee Length Analysis
SELECT
	emp_length AS employment_length,
	COUNT(id) AS total_number_of_applications,
	SUM(loan_amount) AS total_funded_amount,
	SUM(total_payment) AS total_amount_received
FROM
	bank_loan_data
GROUP BY
	emp_length
ORDER BY
	emp_length

--Q19) Loan purpose breakdown
SELECT
	purpose AS loan_purpose,
	COUNT(id) AS total_number_of_applications,
	SUM(loan_amount) AS total_funded_amount,
	SUM(total_payment) AS total_amount_received
FROM
	bank_loan_data
GROUP BY
	purpose
ORDER BY
	SUM(loan_amount) DESC

--Q20) Home ownership Analysis
SELECT
	home_ownership,
	COUNT(id) AS total_number_of_applications,
	SUM(loan_amount) AS total_funded_amount,
	SUM(total_payment) AS total_amount_received
FROM
	bank_loan_data
GROUP BY
	home_ownership
ORDER BY
	home_ownership


SELECT 
	purpose AS PURPOSE, 
	COUNT(id) AS Total_Loan_Applications,
	SUM(loan_amount) AS Total_Funded_Amount,
	SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
WHERE grade = 'A'
GROUP BY purpose
ORDER BY purpose



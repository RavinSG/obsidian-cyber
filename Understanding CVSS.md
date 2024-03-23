
The Common Vulnerability Scoring System ([[Common Vulnerability Scoring System|CVSS]]) is an industry standard for assessing the severity of security vulnerabilities. It provides a technique for scoring each vulnerability on a variety of measures. Cybersecurity analysts often use CVSS ratings to prioritise response actions.

Analysts scoring a new vulnerability begin by rating the vulnerability on *eight different measures*. Each measure is given both a descriptive rating and a numeric score. 

The *first four measures evaluate the exploitability of the vulnerability*, whereas the *last three evaluate the impact of the vulnerability*. The eighth metric discusses the scope of the vulnerability.

### CVSS Metrics
#### Attack Vector Metric
The attack vector metric describes *how an attacker would exploit the vulnerability* and is assigned according to the criteria shown below.

| Value                | Description                                                                                  | Score |
| -------------------- | -------------------------------------------------------------------------------------------- | ----- |
| Physical (P)<br>     | The attacker must physically touch the vulnerable device.<br>                                | 0.20  |
| Local (L)            | The attacker must have physical or logical access to the affected system.                    | 0.55  |
| Adjacent Network (A) | The attacker must have access to the local network that the affected system is connected to. | 0.62  |
| Network<br>(N)<br>   | The attacker can exploit the vulnerability remotely over a network.                          | 0.85  |
#### Attack Complexity Metric
The attack complexity metric describes the *difficulty of exploiting the vulnerability* and is assigned according to the criteria shown below.

| Value    | Description                                                                                         | Score |
| -------- | --------------------------------------------------------------------------------------------------- | ----- |
| High (H) | Exploiting the vulnerability requires “specialised” conditions that would <br>be difficult to find. | 0.44  |
| Low (L)  | Exploiting the vulnerability does not require any specialised conditions.                           | 0.77  |
#### Privileges Required Metric
The privileges required metric describes the *type of account access* that an attacker would need to exploit a vulnerability and is assigned according to the criteria below.

| Value       | Description                                                         | Score                               |
| ----------- | ------------------------------------------------------------------- | ----------------------------------- |
| High<br>(H) | Attackers require administrative privileges to conduct the attack.  | 0.270 (or 0.50 if Scope is Changed) |
| Low<br>(L)  | Attackers require basic user privileges to conduct the attack.      | 0.62 (or 0.68 if Scope is Changed)  |
| None<br>(N) | Attackers do not need to authenticate to exploit the vulnerability. | 0.85                                |
#### User Interaction Metric
The user interaction metric describes whether the attacker *needs to involve another human* in the attack. The user interaction metric is assigned according to the criteria below.

| Value           | Description                                                                          | Score |
| --------------- | ------------------------------------------------------------------------------------ | ----- |
| None<br>(N)     | Successful exploitation does not require action by any user other than the attacker. | 0.85  |
| Required<br>(R) | Successful exploitation does require action by a user other than the attacker.       | 0.62  |
#### Confidentiality Metric
The confidentiality metric describes the *type of information disclosure* that might occur if an attacker successfully exploits the vulnerability. The confidentiality metric is assigned according to the criteria below.

| Value       | Description                                                                                                          | Score |
| ----------- | -------------------------------------------------------------------------------------------------------------------- | ----- |
| None<br>(N) | There is no confidentiality impact.                                                                                  | 0.00  |
| Low<br>(L)  | Access to some information is possible, but the attacker does not have control over what information is compromised. | 0.22  |
| High<br>(H) | All information on the system is compromised.                                                                        | 0.56  |
#### Integrity Metric
The integrity metric describes the *type of information alteration* that might occur if an attacker successfully exploits the vulnerability. The integrity metric is assigned according to the criteria below.

| Value       | Description                                                                                                             | Score |
| ----------- | ----------------------------------------------------------------------------------------------------------------------- | ----- |
| None<br>(N) | There is no integrity impact.                                                                                           | 0.00  |
| Low<br>(L)  | Modification of some information is possible, but the attacker does not have control over what information is modified. | 0.22  |
| High<br>(H) | The integrity of the system is totally compromised, and the attacker may change any information at will.                | 0.56  |
#### Availability Metric
The availability metric describes the *type of disruption* that might occur if an attacker successfully exploits the vulnerability. The availability metric is assigned according to the criteria below.

| Value    | Description                                | Score |
| -------- | ------------------------------------------ | ----- |
| None (N) | There is no availability impact.           | 0.00  |
| Low (L)  | The performance of the system is degraded. | 0.22  |
| High (H) | The system is completely shut down.        | 0.56  |
#### Scope Metric
The scope metric describes whether the vulnerability can affect system components beyond the scope of the vulnerability. The scope metric is assigned according to the criteria below.

> [!note] Note
> The scope metric table does not contain score information. The value of the scope metric is **reflected in the values for the privileges** required metric

| Value            | Description                                                                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Unchanged<br>(U) | The exploited vulnerability can only affect resources managed by the same security authority.                                                    |
| Changed<br>(C)   | The exploited vulnerability can affect resources beyond the scope of the security authority managing the component containing the vulnerability. |
### Interpreting the CVSS Vector

The CVSS vector uses a single-line format to convey the ratings of a vulnerability on all six of the metrics described in the preceding sections. For example, consider the following CVSS vector:  `CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`

This vector contains nine components. The **first section**, “CVSS:3.0,” indicates that the vector was composed using *CVSS version 3*. The **next eight sections** correspond to each of the *eight CVSS metrics*.

- Attack Vector: Network (score: 0.85)
- Attack Complexity: Low (score: 0.77)
- Privileges Required: None (score: 0.85)
- User Interaction: None (score: 0.85)
- Scope: Unchanged
- Confidentiality: High (score: 0.56)
- Integrity: None (score: 0.00)
- Availability: None (score: 0.00)

### Summarising CVSS Scores

The CVSS vector provides good detailed information on the nature of the risk posed by a vulnerability, but the *complexity of the vector makes it difficult to use in prioritisation* exercises. For this reason, analysts can **calculate the CVSS base score**, which is a single number representing the overall risk posed by the vulnerability.

#### Calculating the Impact Sub-Score (ISS)
The first calculation analysts perform is computing the impact sub-score (ISS). This metric summarises the three impact metrics using the formula:

$$ISS = 1 - [(1-Confidentiality) \times (1 - Integrity) \times (1 - Availability)]$$

Plugging the values for the above vulnerability, we obtain

$$
\begin{align} ISS &= 1 - [(1-0.56) \times (1 - 0.00) \times (1 - 0.00)] \\ 
ISS &= 1 - [0.44 \times 1.00 \times 1.00 ] \\
ISS &= 1 - 0.44 \\
ISS &= 0.56
\end{align}
$$
#### Calculating the Impact Score
To obtain the impact score from the impact sub-score, we must take the value of the scope metric into account. If the **scope metric is Unchanged**, as it is in our example, we multiply the ISS by 6.42:

$$
\begin{align} Impact &= 6.42 \times ISS \\
Impact &= 6.42 \times 0.56 \\
Impact &= 3.60
\end{align}
$$

If the score metric is Changed, we use a more complex formula:

$$
Impact = 7.52 \times (ISS - 0.029) - 3.25 \times (ISS - 0.02)^2
$$

#### Calculating the Exploitability Score
Analysts may calculate the exploitability score for a vulnerability using this formula:

$$
Explotability = 8.22 \times \text{Attack Vector} \times \text{Attack Complexity} \times \text{Privileges Required} \times \text{User Interaction}
$$

Plugging in the values for the above vulnerability, we get.

$$\begin{align}
Exploitability &= 8.22 \times 0.85 \times 0.77 \times 0.85 \times 0.85 \\
Exploitability &= 3.89
\end{align}
$$

#### Calculating the Base Score
With all of this information at hand, we can now determine the CVSS base score using the following rules:

- If the impact is 0, the base score is 0.
- If the scope metric is **Unchanged**, calculate the base score by *adding together* the impact and exploitability scores.
- If the scope metric is **Changed**, calculate the base score by adding together the impact and exploitability scores and *multiplying the result by 1.08*.
- The highest possible base score is 10. If the calculated value is greater than 10, set the base score to 10.

For our above example, we get the value 7.5 by adding 3.6 and 3.9 together.

### Categorising CVSS Base Scores

Many vulnerability scanning systems further summarise CVSS results by *using risk categories* rather than numeric risk ratings. These are usually based on the CVSS Qualitative Severity Rating Scale, shown below.

| CVSS Score   | Rating   |
| ------------ | -------- |
| $0.0$        | None     |
| $0.1 - 3.9$  | Low      |
| $4.0 - 6.9$  | Medium   |
| $7 - 8.9$    | High     |
| $9.0 - 10.0$ | Critical |
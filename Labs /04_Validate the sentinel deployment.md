---
lab:
    title: Validate the Sentinel Deployment
    module: Configure Microsoft Sentinel Data Collection rules, NRT Analytic rule and Automation
---

>**Note**: To complete this lab, you will need an [Azure subscription.](https://azure.microsoft.com/free/?azure-portal=true) in which you have administrative access.

## General guidelines

- When creating objects, use the default settings unless there are requirements that require different configurations.
- Only create, delete, or modify objects to achieve the stated requirements. Unnecessary changes to the environment may adversely affect your final score.
- If there are multiple approaches to achieving a goal, always choose the approach the requires the least amount of administrative effort.

We need to configure Microsoft Sentinel to receive security events from virtual machines that run Windows.

## Architecture diagram

![Diagram of Windows Security Events via AMA using DCR](../Media/apl-5001-lab-diagrams-lab03.png)

## Skilling tasks

You need to validate the Microsoft Sentinel deployment to meet the following requirements:

- Configure the Windows Security Events via AMA connector to collect all security events from only a virtual machine named VM1.
- Create a near-real-time (NRT) query rule to generate an incident based on the following query.

```KQL
SecurityEvent 
| where EventID == 4732
| where TargetAccount == "Builtin\\Administrators"
```

- Create an automation rule that assigns Operator1 the Owner role for incidents that are generate by the NRT rule.

## Exercise instructions

>**Note**: In the following tasks, to access `Microsoft Sentinel`, select the `workspace` you created in Lab 01.

### Task 1 - Configure Data Collection rules (DCRs) in Microsoft Sentinel

Configure a Windows Security Events via AMA connector. Learn more about [Windows Security Events via AMA connector](https://learn.microsoft.com/azure/sentinel/data-connectors/windows-security-events-via-ama).

 1. In `Microsoft Sentinel`, go to the `Configuration` menu section and select **Data connectors**
 1. Search for and select **Windows Security Events via AMA**
 1. Select **Open connector page**
 1. In the `Configuration` area, select **+Create data collection rule**
 1. On the `Basics` tab enter a `Rule Name`
 1. On the `Resources` tab expand your subscription and the `RG1` resource group in the `Scope` column
 1. Select `VM1`, and then select **Next: Collect >**
 1. On the `Collect` tab leave the default of `All Security Events`
 1. Select **Next: Review + create >**, then select **Create**

### Task 2 - Create a near real-time (NRT) query detection

Detect threats with near-real-time (NRT) analytic rules in Microsoft Sentinel. Learn more about [NRT Analytic rules in Microsoft Sentinel](https://learn.microsoft.com/azure/sentinel/near-real-time-rules).

 1. In `Microsoft Sentinel`, go to the `Configuration` menu section and select **Analytics**
 1. Select **+ Create**, and **NRT query rule (Preview)**
 1. Enter a `Name` for the rule, and select **Privilege Escalation** from `Tactics and techniques`.
 1. Select **Next: Set rule logic >**
 1. Enter the KQL query into the `Rule query`form

    ```KQL
    SecurityEvent 
    | where EventID == 4732
    | where TargetAccount == "Builtin\\Administrators"
    ```

 1. Select **Next: Incident settings >**, and select **Next: Automated response >**
 1. Select **Next: Review + Create**
 1. When validation is complete select **Save**

### Task 3 - Configure automation in Microsoft Sentinel 

Configure automation in Microsoft Sentinel. Learn more about [Create and use Microsoft Sentinel automation rules](https://learn.microsoft.com/azure/sentinel/create-manage-use-automation-rules).

 1. In `Microsoft Sentinel`, go to the `Configuration` menu section and select **Automation**
 1. Select **+ Create**, and Automation rule
 1. Enter an `Automation rule name`, and select **Assign owner** from `Actions`
 1. Assign **Operator1** as the owner.
 1. Select **Apply**

```
Lets create valid user flow and moves om creating document validation rules for specific customer

  

1. First user go portal > AI Agent Hub > Customer SOPs

2. Then create Document validation SOP for specific customer

- Click on 'Create SOP' button

- Select Customer

- Click on 'Save' button ( Chat section Appear after created )

3. In the selected customer Chat section, prompt agent to create SOP, for e.g "Make POD required on driver arrved on deliver events" (Code implementation link with separate page)

# Agent will create plan with stepper

- Add playbook content

- Verify entities

- Review playbook test

- Show preview for confirmation

- Set test loads

- Save playbook

- Process new scnearios

  

4. After "Show preview for confirmation" this step, agent will ask user for using "test load for testing or provide different load reference number..", reply with -> go for test load (code link reference, explain code in separate page)

5. After "Save playbook" playbook saved succesfully, agent will ask user for "Please upload valid example documents for knowledge and verification in future with uploaded docs or skip option", say yes with upload sample example documents (code link reference, explain code in separate page)

6. All step completed successfully, goto admin portal '/admin/ai-sops-playbook',

- Search and select respective carrier, then you will see all the created playbooks

- Click 'View Rules' and approve the rules (approve code implementation link with separate page)

- Click 'Bussiness Approved' button (code implementation link with separate page), it will create plan scenarios in the background

- Click 'Engineering Approved' button (code implemenation link with separate page)

7. Goto customer portal > AI Agent Hub > Customer SOPs > Recently created playbook customer, then approve playbook

8. Now, whatever rules are created will apply for the all respective customer load if validation matched.

- For e.g, there is rules for 'POD required on driver arrived on deliver events', so when driver arrived on deliver event of respective customer (might trigger ai generated rules, find out if yes then share ref code link), then in the document section, there is one file upload drop zone box appear with required text.
```






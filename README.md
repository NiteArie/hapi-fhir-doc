# KMS Healthcare - HAPI FHIR 

## HAPI FHIR with OpenEMR

### Guideline

#### Registering the application to OpenEMR server instance

Requirement: Need to have a running OpenEMR server instance

##### Register the application

Use an API testing tool. I.e Postman, curl, Insomia, ...

Send a HTTP `POST` request to your OpenEMR instance URL with the path `oauth2/default/registration` with the specified data. In our case, it is `http://vox.kms-technology.com/oauth2/default/registration`.

Specified data:
```json
{
    "application_type": "private",
    "redirect_uris": [
        "http://localhost:8080"
    ],
    "initiate_login_uri": "http://localhost:8080",
    "client_name": "my-test-web-app",
    "scope": "openid api:oemr api:fhir api:port user/allergy.read user/allergy.write user/appointment.read user/appointment.write user/dental_issue.read user/dental_issue.write user/document.read user/document.write user/drug.read user/encounter.read user/encounter.write user/facility.read user/facility.write user/immunization.read user/insurance.read user/insurance.write user/insurance_company.read user/insurance_company.write user/insurance_type.read user/list.read user/medical_problem.read user/medical_problem.write user/medication.read user/medication.write user/message.write user/patient.read user/patient.write user/practitioner.read user/practitioner.write user/prescription.read user/procedure.read user/soap_note.read user/soap_note.write user/surgery.read user/surgery.write user/vital.read user/vital.write user/AllergyIntolerance.read user/CareTeam.read user/Condition.read user/Encounter.read user/Immunization.read user/Location.read user/Medication.read user/MedicationRequest.read user/Observation.read user/Organization.read user/Organization.write user/Patient.read user/Patient.write user/Practitioner.read user/Practitioner.write user/PractitionerRole.read user/Procedure.read patient/encounter.read patient/patient.read patient/Encounter.read patient/Patient.read"
}
```

`client_name`: Name of the application on OpenEMR instance, can be changed.

`scope`: Specifiy the permission of the application. Can be changed, if changed need to make sure that this `scope` and `openemr.scope` in `application.yaml` is the same.


Example curl request:

```sh
curl -X POST -k -H 'Content-Type: application/json' -i http://vox.kms-technology.com/oauth2/default/registration --data '{
    "application_type": "private",
    "redirect_uris": [
        "http://localhost:8080"
    ],
    "initiate_login_uri": "http://localhost:8080",
    "client_name": "my-test-web-app",
    "scope": "openid api:oemr api:fhir api:port user/allergy.read user/allergy.write user/appointment.read user/appointment.write user/dental_issue.read user/dental_issue.write user/document.read user/document.write user/drug.read user/encounter.read user/encounter.write user/facility.read user/facility.write user/immunization.read user/insurance.read user/insurance.write user/insurance_company.read user/insurance_company.write user/insurance_type.read user/list.read user/medical_problem.read user/medical_problem.write user/medication.read user/medication.write user/message.write user/patient.read user/patient.write user/practitioner.read user/practitioner.write user/prescription.read user/procedure.read user/soap_note.read user/soap_note.write user/surgery.read user/surgery.write user/vital.read user/vital.write user/AllergyIntolerance.read user/CareTeam.read user/Condition.read user/Encounter.read user/Immunization.read user/Location.read user/Medication.read user/MedicationRequest.read user/Observation.read user/Organization.read user/Organization.write user/Patient.read user/Patient.write user/Practitioner.read user/Practitioner.write user/PractitionerRole.read user/Procedure.read patient/encounter.read patient/patient.read patient/Encounter.read patient/Patient.read"
}'
```

Example response:

```json
{
    "client_id": "ijzjnwJ3zEf7P8dUKqJQIA7x5ey2FtUHr8k2U7JPaxU",
    "client_secret": "PYjpeHwrbmSIFYD4qm1ujWADB5w0mDQUUvu26jhbz20XmaKbsVcbxZc68438FH8IdvDOXrssBfXO75lUrRQhYg",
    "registration_access_token": "YGry9e2q2gXGoNDVTSmzpWH5BQWaxLLsl2xpAi0laZE",
    "registration_client_uri": "https://vox.kms-technology.com/oauth2/default/client/YIvB_6CQ7AMM4aUZUJWmrA",
    "client_id_issued_at": 1622003155,
    "client_secret_expires_at": 0,
    "client_role": "user",
    "application_type": "private",
    "client_name": "my-test-web-app",
    "redirect_uris": [
        "http://localhost:8080"
    ],
    "initiate_login_uri": "http://localhost:8080",
    "scope": "openid api:oemr api:fhir api:port user/allergy.read user/allergy.write user/appointment.read user/appointment.write user/dental_issue.read user/dental_issue.write user/document.read user/document.write user/drug.read user/encounter.read user/encounter.write user/facility.read user/facility.write user/immunization.read user/insurance.read user/insurance.write user/insurance_company.read user/insurance_company.write user/insurance_type.read user/list.read user/medical_problem.read user/medical_problem.write user/medication.read user/medication.write user/message.write user/patient.read user/patient.write user/practitioner.read user/practitioner.write user/prescription.read user/procedure.read user/soap_note.read user/soap_note.write user/surgery.read user/surgery.write user/vital.read user/vital.write user/AllergyIntolerance.read user/CareTeam.read user/Condition.read user/Encounter.read user/Immunization.read user/Location.read user/Medication.read user/MedicationRequest.read user/Observation.read user/Organization.read user/Organization.write user/Patient.read user/Patient.write user/Practitioner.read user/Practitioner.write user/PractitionerRole.read user/Procedure.read patient/encounter.read patient/patient.read patient/Encounter.read patient/Patient.read"
}
```



##### Enable the application

Access the instance URL in your browser. In our case it is `http://vox.kms-technolgy.com` (it may be different with yours), you will be presented with a login screen if the instance is configured correctly.

The default credential for administration is `admin` and `pass` seriated by username and password.

*The login screen may vary between different versions with different themes*

![OpenEMR Login Screen](https://user-images.githubusercontent.com/49594981/119623910-d53b2f00-be32-11eb-933c-abd486817582.png)

After logging in, you are presented with the homepage. Navigate to **Administration** > **System** > **API Clients**. 
You will be presented with the Client Registrations screen. Find your registered application with the **Client Name / Client ID** matches the `client_name` and `client_id` on the previous step.

If the **Enabled** field has the value `Enabled` for your application by default. Then you are good to the next step.

If not, click on **Edit** button on your application, click **Enable Client** and then check if your application is enabled.

![OpenEMR API Client Screen](https://user-images.githubusercontent.com/49594981/119623994-e8e69580-be32-11eb-9fbe-83e14f87e6bd.png)


#### Run the OpenEMR application

There is an `application.yaml` file that exists in `hapi-fhir-open-emr/src/main/resources`. This file contains many configurable properties, but we are only concenered with these properties:
```yaml
openemr:
  token_url: "http://vox.kms-technology.com/oauth2/default/token"
  api_url: "http://vox.kms-technology.com/apis/default/api/"
  client_id: "fyA8XMALYwtbuBkeVQtxB6-bG_d97X08L-cKeJOnv9Q"
  grant_type: "password"
  user_role: "users"
  scope: "openid api:oemr api:fhir api:port user/allergy.read user/allergy.write user/appointment.read user/appointment.write user/dental_issue.read user/dental_issue.write user/document.read user/document.write user/drug.read user/encounter.read user/encounter.write user/facility.read user/facility.write user/immunization.read user/insurance.read user/insurance.write user/insurance_company.read user/insurance_company.write user/insurance_type.read user/list.read user/medical_problem.read user/medical_problem.write user/medication.read user/medication.write user/message.write user/patient.read user/patient.write user/practitioner.read user/practitioner.write user/prescription.read user/procedure.read user/soap_note.read user/soap_note.write user/surgery.read user/surgery.write user/vital.read user/vital.write user/AllergyIntolerance.read user/CareTeam.read user/Condition.read user/Encounter.read user/Immunization.read user/Location.read user/Medication.read user/MedicationRequest.read user/Observation.read user/Organization.read user/Organization.write user/Patient.read user/Patient.write user/Practitioner.read user/Practitioner.write user/PractitionerRole.read user/Procedure.read patient/encounter.read patient/patient.read patient/Encounter.read patient/Patient.read"
  username: "admin"
  password: "pass"
```

`token_url` is the required URL for the application to get an OpenEMR access token. Depends on your running OpenEMR instance, this value can be changed accordingly.

`api_url` is the base OpenEMR API route that the application interacts with. Depends on your running OpenEMR instance, this value can be changed accordingly.

`client_id` is the one you replace its with the `client_id` value you got from the previous step.

`scope` specifies the permission of the the application. If the registration `scope` was changed, change this value accordingly.

#### OpenEMR Data Generation

There is an `application.properties` file that exists in `hapi-fhir-open-emr-generate/src/main/resources`. Within this field contains the configuration for the patient generate application.

```properties
openemr.population = 10
openemr.token_url = http://vox.kms-technology.com/oauth2/default/token
openemr.fhir_url = http://vox.kms-technology.com/apis/default/fhir/
openemr.client_id = azw42uKKfA3Cu4XvVmRo21MYyyzQo_lVYeJUU7HdD08
openemr.grant_type = password
openemr.user_role = users
openemr.scope = openid api:oemr api:fhir api:port api:pofh user/allergy.read user/allergy.write user/appointment.read user/appointment.write user/dental_issue.read user/dental_issue.write user/document.read user/document.write user/drug.read user/encounter.read user/encounter.write user/facility.read user/facility.write user/immunization.read user/insurance.read user/insurance.write user/insurance_company.read user/insurance_company.write user/insurance_type.read user/list.read user/medical_problem.read user/medical_problem.write user/medication.read user/medication.write user/message.write user/patient.read user/patient.write user/practitioner.read user/practitioner.write user/prescription.read user/procedure.read user/soap_note.read user/soap_note.write user/surgery.read user/surgery.write user/vital.read user/vital.write user/AllergyIntolerance.read user/CareTeam.read user/Condition.read user/Encounter.read user/Immunization.read user/Location.read user/Medication.read user/MedicationRequest.read user/Observation.read user/Organization.read user/Organization.write user/Patient.read user/Patient.write user/Practitioner.read user/Practitioner.write user/PractitionerRole.read user/Procedure.read patient/encounter.read patient/patient.read patient/Encounter.read patient/Patient.read
openemr.username = admin
openemr.password = pass
```

`population` stands for the number of patients that will be created. Change this according to your needs

`token_url` is the required URL for the application to get an OpenEMR access token. Depends on your running OpenEMR instance, this value can be changed accordingly.

`fhir_url` is the base OpenEMR FHIR API route that the application interacts with. Depends on your running OpenEMR instance, this value can be changed accordingly.

`client_id` is the application id on the OpenEMR system. This can be found by re-doing the registration step or re-using existed `client_id` on the system.

`scope` specifies the permission of the data generation permission. Can be changed accordingly, but require one specific scope `api:fhir` to be present regardless the cases.
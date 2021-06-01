
### Linked issue
https://github.com/Rashmi-K-A/gsoc/issues/1


- [Setting up](https://github.com/Rashmi-K-A/gsoc/blob/main/issues/Week2.md#work-done)
- [Answering questions](https://github.com/Rashmi-K-A/gsoc/blob/main/issues/Week2.md#questions)
### Work done

1. Start by creating a new individual. This individual will have a profile and can have at least two identities.
    #### Created two identities with the following query:

    - Create 2 Identity identities as follows:
        ```
            mutation {
                addIdentity(email: "harrypotter@hogwarts.com", name: "Harry Potter", username: "hpotter", source: "github") {
                    uuid
                }
            }
        ```

        Response:

        ```
            {
                "data": {
                    "addIdentity": {
                    "uuid": "2853249208a6dc4f6a4c1002caa1ea0fabfda7ab"
                    }
                }
            }

            {
                "data": {
                    "addIdentity": {
                    "uuid": "2c85dfc55fd09fc55f04dc0a7585f22267198241"
                    }
                }
            }

        ```
    - Create Profile
        ```
            mutation {
                updateProfile(
                    uuid: "2c85dfc55fd09fc55f04dc0a7585f22267198241",
                    data: {name: "Harry James Potter", gender: "Male"}
                ) 
                {
                    uuid
                    individual {
                        identities {
                                uuid
                                name
                                email
                         }
                    }
                }
            }
        ```

        Response:
        ```
            {
                "data": {
                    "updateProfile": {
                    "uuid": "2c85dfc55fd09fc55f04dc0a7585f22267198241",
                    "individual": {
                        "identities": [
                        {
                            "uuid": "2c85dfc55fd09fc55f04dc0a7585f22267198241",
                            "name": "Harry James Potter",
                            "email": "hpotter@hogwarts.com"
                        }
                        ]
                    }
                    }
                }
            }
        ```

    - Merge Identities
        ```
            mutation {
            merge(fromUuids: ["2853249208a6dc4f6a4c1002caa1ea0fabfda7ab"], toUuid: "2c85dfc55fd09fc55f04dc0a7585f22267198241") {
                individual {
                identities {
                    name
                    email
                }
                }
            }
            }
        ```

        Response:
        ```
            {
            "data": {
                "merge": {
                "individual": {
                    "identities": [
                    {
                        "name": "Harry Potter",
                        "email": "harrypotter@hogwarts.com"
                    },
                    {
                        "name": "Harry James Potter",
                        "email": "hpotter@hogwarts.com"
                    }
                    ]
                }
                }
            }
        }
        ```
            



2. Create a new organization and link a domain


    - Create organization
        ```
           mutation {
                addOrganization(name: "Hogwarts") {
                    organization {
                    name
                    }
                }
            }
        ```


        Response:
         ```
            {
            "data": {
                "addOrganization": {
                        "organization": {
                            "name": "Hogwarts"
                        }
                    }
                }
            }
        ```

    - Create domain
        ```
            mutation {
                addDomain(domain:"hogwarts.com", organization: "Hogwarts"){
                    domain {
                        organization {
                            name
                        }
                    }
                }
            }
        ```

        Response:
        ```
        {
            "data": {
                "addDomain": {
                "domain": {
                    "organization": {
                    "name": "Hogwarts"
                    }
                }
                }
            }
        }
        ```
                
3. Enroll the new individual to the new organization
    ```
        mutation {
            enroll (
                organization: "DupeGoogle",
                uuid: "6d84a5904d68c6b13e5349776fd06f7c4cf2e3f5"
            ){
                    individual {
                        enrollments{
                            organization {
                                name
                            }
                            start
                            end
                        }
                    }
            }
        }
    ```
    Response:
    ```
        {
            "data": {
                "enroll": {
                "individual": {
                    "enrollments": [
                    {
                        "organization": {
                        "name": "Hogwarts"
                        },
                        "start": "1900-01-01T00:00:00+00:00",
                        "end": "2100-01-01T00:00:00+00:00"
                    }
                    ]
                }
                }
            }
        }
    ```

4. Update that enrollment by changing the start and end date
    ```
        mutation {
            updateEnrollment(uuid: "2c85dfc55fd09fc55f04dc0a7585f22267198241", fromDate: "1991-01-01T00:00:00+00:00", newFromDate: "1992-01-01T00:00:00+00:00", toDate: "2100-01-01T00:00:00+00:00", newToDate: "2002-01-01T00:00:00+00:00", organization: "Hogwarts") {
                individual {
                enrollments {
                    organization {
                    name
                    }
                    start
                    end
                }
                }
            }
        }
    ```

    Response:
    ```
        {
            "data": {
                "updateEnrollment": {
                "individual": {
                    "enrollments": [
                    {
                        "organization": {
                        "name": "Hogwarts"
                        },
                        "start": "1992-01-01T00:00:00+00:00",
                        "end": "2002-01-01T00:00:00+00:00"
                    }
                    ]
                }
                }
            }
        }
    ```



### Questions
1. Can a given organization have more than one domain?
      - Yes, it can
2.  Can a given individual be enrolled to many organizations?
      - Yes
    What about overlapping date ranges?
        - Yes
3. Can we have the same domain linked to different organizations?
      - No, we get a domain already exists error if a given domain is added again
4. Is the affiliation information from a given individual related with its Profile?
      - The affiliation information is linked between the Individual model and an Organization and not the profile of the individual


Help needed:

1. I need more understanding of the fields in Enroll:
    
    -   Adding a query as follows, updates the toDate as well. In that case, why is the newToDate field required?
      ```
        mutation {
        updateEnrollment(uuid: "2c85dfc55fd09fc55f04dc0a7585f22267198241", 
            fromDate: "1994-01-01T00:00:00+00:00", 
            newFromDate: "1992-01-01T00:00:00+00:00", 
            toDate: "2009-01-01T00:00:00+00:00", 
            organization: "Hogwarts") {
            individual {
            enrollments {
                organization {
                name
                }
                start
                end
            }
            }
        }
        }
    ```

    Response:
    ```
        {
            "data": {
                "updateEnrollment": {
                "individual": {
                    "enrollments": [
                    {
                        "organization": {
                        "name": "Hogwarts"
                        },
                        "start": "1992-01-01T00:00:00+00:00",
                        "end": "2009-01-01T00:00:00+00:00"
                    }
                    ]
                }
                }
            }
        }
    ```




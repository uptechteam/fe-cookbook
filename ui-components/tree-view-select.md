# Tree view select Component

1. [General information](#general-info)
3. [Usage](#usage)

## General information

A tree view select component refers to a user interface element used in software development and web applications. It is a graphical representation of hierarchical data in a tree-like structure, where items can be organized into parent-child relationships.

The tree view select component typically displays a collapsible and expandable tree structure, allowing users to navigate through the hierarchy and select items. Each item in the tree can have child items, forming a nested structure. When an item is selected, its children, if any, are revealed, and the user can continue to drill down the tree until they find the desired item.

It could look like this component below:
![tree view select](https://github.com/uptechteam/fe-cookbook/assets/13544983/a7ae7a13-e6c9-4b78-990f-9b2ca55b5cba)

## Usage

Here is a [demo](https://codesandbox.io/p/sandbox/aged-field-4jwtrq) of this component in CodeSandbox.
We use react-hook-form and MUI here.

```typescript

// Input data sample:

  const locations = [
    {
      id: 1,
      city: 'Berlin',
      country: 'Germany',
      name: 'Test 1',
    },
    {
      id: 2,
      city: 'WASHINGTON, DC',
      country: 'USA',
      name: 'Test 2',
    },
    {
      id: 3,
      city: 'WASHINGTON, DC',
      country: 'USA',
      name: 'Test 3',
    },
    {
      id: 4,
      city: 'Tel Aviv',
      country: 'Israel',
      name: 'Test 4',
    },
    {
      id: 5,
      city: 'Warsaw',
      country: 'Poland',
      name: 'Test 5',
    },
    {
      id: 6,
      city: 'London',
      country: 'Great Britain',
      name: 'Test 6',
    },
    {
      id: 7,
      city: 'UTRECHT',
      country: 'Netherlands',
      name: 'Test 7',
    },
    {
      id: 8,
      city: 'Dusseldorf',
      country: 'Germany',
      name: 'Test 8',
    },
    {
      id: 9,
      city: 'Kyiv',
      country: 'Ukraine',
      name: 'Test 9',
    },
    {
      id: 11,
      city: 'Miami',
      country: 'USA',
      name: 'Test 10',
    },
  ];

// We need to transform this data to the tree structure using this function

export interface ILocation {
  id: number;
  city: string;
  country: string;
  name: string;
}

export interface Option {
  id: string;
  name: string;
  city?: string;
  country?: string;
  suboptions?: Option[];
}

const transformLocationsStructure = (data: ILocation[]): Option[] => {
  const res = [];
  data?.forEach((item) => {
    const foundCountry = res.find((country) => item.country === country.name);
    if (foundCountry) {
      const foundCity = foundCountry.suboptions.find(
        (city) => item.city === city.name
      );
      if (foundCity) {
        const foundLocation = foundCity.suboptions.find(
          (location) => item.name === location.name
        );
        if (!foundLocation) {
          foundCity.suboptions.push({
            id: item.id,
            name: item.name,
            city: item.city,
            country: item.country,
          });
        }
      } else {
        foundCountry.suboptions.push({
          id: uuidv4(),
          name: item.city,
          suboptions: [{ id: item.id, name: item.name }],
          city: item.city,
          country: item.country,
        });
      }
    } else {
      res.push({
        id: uuidv4(),
        name: item.country,
        city: item.city,
        country: item.country,
        suboptions: [
          {
            id: uuidv4(),
            name: item.city,
            city: item.city,
            country: item.country,
            suboptions: [
              {
                id: item.id,
                name: item.name,
                city: item.city,
                country: item.country,
              },
            ],
          },
        ],
      });
    }
  });
  return res;
};

// Output data sample:

const outputLocationsData = [
    {
        "id": "c5a5462b-668a-48e4-b2ed-875626cf834d",
        "name": "Germany",
        "city": "Berlin",
        "country": "Germany",
        "suboptions": [
            {
                "id": "722c71db-1933-46fa-8f37-4a1705cc6a9d",
                "name": "Berlin",
                "city": "Berlin",
                "country": "Germany",
                "suboptions": [
                    {
                        "id": 1,
                        "name": "Test 1",
                        "city": "Berlin",
                        "country": "Germany"
                    }
                ]
            },
            {
                "id": "3ea4c72d-1c9a-4b90-8fc8-eaff5de64fe6",
                "name": "Dusseldorf",
                "suboptions": [
                    {
                        "id": 8,
                        "name": "Test 8"
                    }
                ],
                "city": "Dusseldorf",
                "country": "Germany"
            }
        ]
    },
    {
        "id": "ae0d2d52-f6aa-4817-a6bf-75e4890fe508",
        "name": "USA",
        "city": "WASHINGTON, DC",
        "country": "USA",
        "suboptions": [
            {
                "id": "64692fc8-0ff9-4175-be96-90053780f5d7",
                "name": "WASHINGTON, DC",
                "city": "WASHINGTON, DC",
                "country": "USA",
                "suboptions": [
                    {
                        "id": 2,
                        "name": "Test 2",
                        "city": "WASHINGTON, DC",
                        "country": "USA"
                    },
                    {
                        "id": 3,
                        "name": "Test 3",
                        "city": "WASHINGTON, DC",
                        "country": "USA"
                    }
                ]
            },
            {
                "id": "d274cfe2-73cf-4f48-ae84-2fc748cc9e68",
                "name": "Miami",
                "suboptions": [
                    {
                        "id": 11,
                        "name": "Test 10"
                    }
                ],
                "city": "Miami",
                "country": "USA"
            }
        ]
    },
    {
        "id": "acfc4962-cd18-4549-b391-b52aa3c10da9",
        "name": "Israel",
        "city": "Tel Aviv",
        "country": "Israel",
        "suboptions": [
            {
                "id": "f24fef40-8df3-4086-89d4-00fd83a0fdfa",
                "name": "Tel Aviv",
                "city": "Tel Aviv",
                "country": "Israel",
                "suboptions": [
                    {
                        "id": 4,
                        "name": "Test 4",
                        "city": "Tel Aviv",
                        "country": "Israel"
                    }
                ]
            }
        ]
    },
    {
        "id": "ff1a0e7e-e3b0-46a0-b292-2cff8c065b59",
        "name": "Poland",
        "city": "Warsaw",
        "country": "Poland",
        "suboptions": [
            {
                "id": "fb231ba5-fc4e-402f-ad13-a32965d4a8df",
                "name": "Warsaw",
                "city": "Warsaw",
                "country": "Poland",
                "suboptions": [
                    {
                        "id": 5,
                        "name": "Test 5",
                        "city": "Warsaw",
                        "country": "Poland"
                    }
                ]
            }
        ]
    },
    {
        "id": "25bf8034-e076-4ca4-a6f0-f1b8b2df376a",
        "name": "Great Britain",
        "city": "London",
        "country": "Great Britain",
        "suboptions": [
            {
                "id": "9df31651-f22c-4a6d-a143-09ec00453e7a",
                "name": "London",
                "city": "London",
                "country": "Great Britain",
                "suboptions": [
                    {
                        "id": 6,
                        "name": "Test 6",
                        "city": "London",
                        "country": "Great Britain"
                    }
                ]
            }
        ]
    },
    {
        "id": "4f307222-210d-46b8-890b-a8ccf3e7d454",
        "name": "Netherlands",
        "city": "UTRECHT",
        "country": "Netherlands",
        "suboptions": [
            {
                "id": "fd112ddc-0376-4afe-8d93-828ccf723592",
                "name": "UTRECHT",
                "city": "UTRECHT",
                "country": "Netherlands",
                "suboptions": [
                    {
                        "id": 7,
                        "name": "Test 7",
                        "city": "UTRECHT",
                        "country": "Netherlands"
                    }
                ]
            }
        ]
    },
    {
        "id": "051e3689-d16d-4979-8481-d35cd3d91483",
        "name": "Ukraine",
        "city": "Kyiv",
        "country": "Ukraine",
        "suboptions": [
            {
                "id": "efab4b3c-31ce-4928-bce4-581b7857202b",
                "name": "Kyiv",
                "city": "Kyiv",
                "country": "Ukraine",
                "suboptions": [
                    {
                        "id": 9,
                        "name": "Test 9",
                        "city": "Kyiv",
                        "country": "Ukraine"
                    }
                ]
            }
        ]
    }
];

```

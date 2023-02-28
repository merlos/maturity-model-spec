# Maturity Model Specification V0.1-WIP

The goal of this specification is to define a standard exchange format for sharing values of maturity models.

## What is a maturity model

A maturity model is a structured approach that organizations use to assess their level of maturity in a particular area of interest, such as information technology, project management, or risk management. A maturity model typically consists of a set of stages or levels, with each level representing a progressively more advanced level of maturity.

Maturity models help organizations to take steps they need to take to improve their processes, practices, and capabilities to achieve their desired level of maturity.

Maturity models typically include a set of criteria or characteristics that define each level of maturity, along with guidelines or best practices that organizations can use to progress from one level to the next. The criteria may be based on industry standards, expert opinions, or research findings, and the model may be customized to the specific needs and goals of the organization.

Maturity models are often used as a tool for benchmarking, assessing progress over time, and identifying areas for improvement. They are used in a wide range of industries and disciplines, including software development, cybersecurity, project management, and quality assurance, among others.

### Abstract
There are many maturity models in the wild. These models help organizations and teams to achieve, in an incremental and iterative way to become more proficient in the specific area of the maturity model.
      
Most of these models can be represented in a share data model. The goal of this specification is to define a share exchange data format.
This format can be used for:

- Maturity model tracking applications 
- Maturity model benchmark websites.
- External evaluation and audit companyes
- Landscape analysis publications
    
### Scope of the definition
- Specification of the maturity model exchange format (json format)
- Basic API Endpoint definition (non-normative)
    
### Out of scope:
    - Authentication and authorization mechanisms, as they may vary for each use case.
    
## Concepts

- **Maturity Model** Consists in a set of indicators. For instance, a maturity model may measure how mature is an organization in terms of digital dexterity.

- **Indicator** is a property whose maturity is going to be evaluated. For instance, following the example of digital dexterity, an indicator may be the use of word processor advanced features within the organization.

- **Maturity Levels** represent the different maturity stages available in the maturity model. Typically maturity models use three levels or maturity: Low, Medium, High or Level 1, Level 2 and Level 3. 

- **Indicator Maturity Level** For each indicator, it indicates what are the conditions or criteria required in order to achieve or comply with that level of maturity. For example, for the indicator "use of word processor advanced features", the maturity level 1 can be defined as "less than 33% of the users of the organization have ever used the automatic index in a word document".

- **Maturity Evaluation** Is the evaluation under a maturity model for a particular entity. For example, when the digital dexterity maturity model is used to evaluate an organization such as UNICEF.

- **Indicator Evaluation** Is the level or levels that are assigned in a particular entity for a particular indicator within an evaluation. For example, the evaluation of the indicator "user of word processor advanced features" within UNICEF is level 3.

## Model Exchange format in JSON

```json
{
  "code": "MM01",
  "name": "Digital Resilience Maturity Model",
  "specVersion": "0.1",
  "version": 1,
  "maturityLevels": [
    {
      "code": "L1",
      "name": "Level 1",
      "description": "Lowest value",
      "weight": 1
    }
  ],
  "indicators": [
    {
      "code": "DP-LF",
      "name": "Legitimate and Fair",
      "shortDescription": "As part of the business analysis, requirement decisions it is ensured that personal  data collected for the limited specified purposes has a legitimate base that supports it.",
      "weight": 1,
      "indicatorMaturityLevels": [
        {
          "indicatorCode": "DP-LF",
          "maturityLevelCode": "L1",
          "description": "In order to comply with this level on this indicator A, B and C are in place.",
          "references": [
            {
              "name": "More information",
              "url": "http://github.com/merlos/maturity-model-spec"
            }
          ]
        }
      ],
      "attributes": [
        {
          "modelAttributeCode": "attr1"
        }
      ],
      "references": [
        {
          "name": "More information",
          "url": "http://github.com/merlos/maturity-model-spec"
        }
      ]
    }
  ],
  "indicatorMode": "UniLevel",
  "attributes": [
    {
      "code": "attr1",
      "name": "Analysis and design",
      "description": "Analysis and design is the phase in which the analysis of a solution is performed."
    }
  ],
  "references": [
    {
      "name": "More information",
      "url": "http://github.com/merlos/maturity-model-spec"
    }
  ]
}
```


## Model Exchange format in CSV

`maturitymodel.csv`
 - maturityModel.code
 - maturityModel.name
 - maturityModel.specVersion
 - maturityModel.version

 maturityModel.uid = code+version
 
`maturitylevels.csv`
 - maturityModel.uid
 - maturityLevel.code
 - maturityLevel.name
 - maturityLevel.shortDescription
 - maturityLevel.weight

`indicators.csv`
 - maturityModel.uid
 - indicator.code
 - indicator.name
 - indicator.shortDescription
 
 `indicatorattributes.csv`
 - maturityModel.uid
 - indicator.code
 - indicator.attribute

`indicatormaturitylevels.csv`
 - maturityModel.uid
 - indicator.code, 
 - maturityLevel.code
 - description


`references.csv`
 - indicator.code 
 - maturitylevel.code
 - reference.name
 - reference.url



## Evaluation Exchange format in JSON

```json
{
  "id": 0,
  "name": "string",
  "description": "string",
  "maturityModelCode": "string",
  "computedScore": 0,
  "selectedIndicators": [
    {
      "id": 100,
      "maturityEvaluationId": 0,
      "indicatorCode": "DP-LF",
      "applies": true,
      "comment": "string",
      "evaluatedLevels": [
        {
          "id": 1,
          "maturityModelValueCode": "MM01",
          "indicatorCode": "DP-LF",
          "indicatorEvaluationId": 100,
          "isSelected": false
        }
      ]
    }
  ]
}
```

# Evaluation exchange format in CSV

`evaluation.csv`
- id
- name
- description
- maturityModelCode
- computedScore

`evaluatedIndicators.csv`
- id,
- maturityEvaluationId,
- indicatorCode
- applies
- comment

`evaluatedLevels.csv`
- id
- maturityModelValueCode
- indicatorCode
- indicatorEvaluationId
- isSelected

## General considerations about the specification

 * **Custom extensions**
    There may be needs or specific situation that are not curently covered by the specification. For example, a particular maturity model may force to have an specific order or the indicators other than the alphabetical code or additional properties may be needed to calculate the weight of an indicator. In those cases, to comply with the specification the additional properties shall be prepended by the letter "x". For example, if the indicators require a new attribute to specify the order in which they should be displayed (other than the default which is alphabetically by code) the `xPosition` attribute can be included in the JSON.

    Implementations shall not _break_ or display errors if an unknown attribute that starts with `x` is part of the data model. If it is not supported, it should be ignored.



 * **Categories/Subcategories**. Typically, models are organized in some degreerdering indicators and 


## Governance

Currently, the specification is in a very early stage and is based on the observation of different maturity model.

##
## License
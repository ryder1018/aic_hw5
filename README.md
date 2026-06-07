# Homework 5: Ontology-based Semantic Grounding

## 1. Project Title and Group Members

**Project Title:** Ontology-based Semantic Grounding for Robot Manipulation Tasks  
**Course:** Artificial Intelligence Capstone Spring 2026  

**Group 15 Members**

112550144 王韋凱  
112550152 馬晨瑜  
112550185 張晉睿  
112550044 吳宇藤  
112550042 葉哲睿  
112550138 呂柏翰  

---

## 2. Selected Tasks

This ontology covers all baseline tasks required by the homework specification:

### Cup Stacking

* Blue Cup  
* Pink Cup  

### Cutlery Arrangement

* Knife  
* Fork  
* Plate  

### Toy Block Collection

* Toy Blocks  
* Basket  

Each item corresponds to an instance defined in the ontology.

---

## 3. Ontology Design

The ontology is designed to support semantic grounding and graspability reasoning for robot manipulation tasks.

The model separates four semantic layers:

| Layer | Description |
|------|-------------|
| Object Type | Physical categories such as Cup, Knife, Fork, Plate, ToyBlock, and Basket |
| Task Role | Roles played in a task, such as TargetObject, ReferenceObject, CollectableObject, and ContainerTarget |
| Affordance | Manipulation capabilities such as GraspingAffordance, StackabilityAffordance, SupportAffordance, and ContainmentAffordance |
| Instance | Individual objects observed in the environment |

This design allows the reasoner to infer semantic properties, such as graspability, from object affordances rather than manually assigning them.

---

## 4. Modeled Objects and Affordances

| Object | Type | Task Role | Affordance | Inferred Graspable | Reasoning Basis |
|--------|------|----------|------------|--------------------|-----------------|
| blueCup01 | Cup | TargetObject | Grasping, Stackability | Yes | hasAffordance some GraspingAffordance |
| pinkCup01 | Cup | TargetObject | Grasping, Stackability | Yes | hasAffordance some GraspingAffordance |
| knife01 | Knife | TargetObject | Grasping | Yes | hasAffordance some GraspingAffordance |
| fork01 | Fork | TargetObject | Grasping | Yes | hasAffordance some GraspingAffordance |
| plate01 | Plate | ReferenceObject | Support | No | lacks GraspingAffordance |
| block01 | ToyBlock | CollectableObject | Grasping | Yes | hasAffordance some GraspingAffordance |
| block02 | ToyBlock | CollectableObject | Grasping | Yes | hasAffordance some GraspingAffordance |
| block03 | ToyBlock | CollectableObject | Grasping | Yes | hasAffordance some GraspingAffordance |
| basket01 | Basket | ContainerTarget | Containment | No | lacks GraspingAffordance |

---

## 5. Namespace Policy

The ontology uses separate namespaces for the shared course ontology and the group-specific ontology.

### Course Ontology Namespace

```ttl
@prefix cap: <https://hcis.io/ontology/aicapstone/2026/> .
```

This namespace contains the shared vocabulary provided by the course, including:

* PhysicalObject
* GraspableObject
* TaskRole
* Affordance
* hasAffordance
* hasTaskRole

### Group Ontology Namespace

```ttl
@prefix g15: <https://hcis.io/ontology/aicapstone/2026/group15/> .
```

This namespace contains ontology elements created by Group 15, including:

* blueCup01
* pinkCup01
* knife01
* fork01
* plate01
* block01
* block02
* block03
* basket01
* Group-specific role individuals
* Group-specific affordance individuals
* Ontology metadata describing Group 15

All group-specific instances are consistently serialized under the `cap:group15/` namespace for compatibility with the course ontology naming convention.

The ontology imports the course ontology through:

```ttl
owl:imports <https://hcis.io/ontology/aicapstone/2026> .
```

allowing reuse of the shared vocabulary while maintaining a separate namespace for project-specific entities.

---

## 6. Instructions for Running the Query

### Protege Workflow

1. Open `ontology/group-ontology.ttl` in Protege
2. Start the HermiT reasoner
3. Verify that graspable objects are inferred as `cap:GraspableObject`
4. Export the inferred ontology as `ontology/inferred-results.ttl`
5. Execute the SPARQL query in:

```
queries/graspable_objects.rq
```

The query returns all inferred instances of `cap:GraspableObject`.

---

## 7. Expected Query Output

The query retrieves all objects that are inferred to be instances of `cap:GraspableObject`.

Example output:

```
cap:group15/block01     toy block 01@en     cap:group15/collectableObjectRole
cap:group15/block02     toy block 02@en     cap:group15/collectableObjectRole
cap:group15/block03     toy block 03@en     cap:group15/collectableObjectRole
cap:group15/blueCup01   blue cup 01@en      cap:group15/targetObjectRole
cap:group15/fork01      fork 01@en          cap:group15/targetObjectRole
cap:group15/knife01     knife 01@en         cap:group15/targetObjectRole
cap:group15/pinkCup01   pink cup 01@en      cap:group15/targetObjectRole
```

These objects are inferred as graspable because they satisfy the necessary and sufficient conditions defined in the ontology.

Note: The output format is a simplified representation for readability and is not the raw RDF serialization format.

---

## 8. What Is Inferred (Not Merely Asserted)

This section summarizes the semantic distinction between asserted and inferred knowledge.

The ontology does not directly assert that objects are instances of `cap:GraspableObject`.

Instead, objects are classified as `cap:GraspableObject` when they satisfy the necessary and sufficient conditions defined in the ontology.

Using this reasoning rule, the ontology automatically classifies:

* block01
* block02
* block03
* blueCup01
* pinkCup01
* fork01
* knife01

The following objects are not inferred as graspable:

* plate01
* basket01

because they do not satisfy the graspability definition in the ontology.

---

## 9. Generation of ontology/inferred-results.ttl

The ontology was loaded into an OWL reasoner and classified using OWL reasoning.

The reasoner inferred additional class memberships, including `cap:GraspableObject`.

The resulting inferred graph was exported and saved as:

```
ontology/inferred-results.ttl
```

This file contains both asserted facts and inferred triples generated during reasoning.

---

## 10. Repository Resources

| Resource                 | Location                                 |
| ------------------------ | ---------------------------------------- |
| Main Ontology File       | `ontology/group-ontology.ttl`            |
| Inferred Ontology        | `ontology/inferred-results.ttl`          |
| Imported Course Ontology | `ontology/imports/course-affordance.ttl` |
| SPARQL Query             | `queries/graspable_objects.rq`           |
| Query Result Output      | `results/graspable_objects_output.txt`   |
| Report                   | `report.md`                              |

---

## Repository Structure

The following structure reflects the repository organization used in this submission:

```
.
├── README.md
├── report.md
├── ontology
│   ├── group-ontology.ttl
│   ├── inferred-results.ttl
│   └── imports
│       └── course-affordance.ttl
├── queries
│   └── graspable_objects.rq
├── results
└── src
```

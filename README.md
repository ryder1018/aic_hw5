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

---

## 3. Ontology Design

The ontology is designed to support semantic grounding and graspability reasoning for robot manipulation tasks.

The model separates four semantic layers:

| Layer       | Description                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------- |
| Object Type | Physical categories such as Cup, Knife, Fork, Plate, ToyBlock, and Basket                                                  |
| Task Role   | Roles played in a task, such as TargetObject, ReferenceObject, CollectableObject, and ContainerTarget                      |
| Affordance  | Manipulation capabilities such as GraspingAffordance, StackabilityAffordance, SupportAffordance, and ContainmentAffordance |
| Instance    | Individual objects observed in the environment                                                                             |

This design allows the reasoner to infer semantic properties, such as graspability, from object affordances rather than manually assigning them.

---

## 4. Modeled Objects and Affordances

| Object    | Type     | Task Role         | Affordance             | Inferred Graspable |
| --------- | -------- | ----------------- | ---------------------- | ------------------ |
| blueCup01 | Cup      | TargetObject      | Grasping, Stackability | Yes                |
| pinkCup01 | Cup      | TargetObject      | Grasping, Stackability | Yes                |
| knife01   | Knife    | TargetObject      | Grasping               | Yes                |
| fork01    | Fork     | TargetObject      | Grasping               | Yes                |
| plate01   | Plate    | ReferenceObject   | Support                | No                 |
| block01   | ToyBlock | CollectableObject | Grasping               | Yes                |
| block02   | ToyBlock | CollectableObject | Grasping               | Yes                |
| block03   | ToyBlock | CollectableObject | Grasping               | Yes                |
| basket01  | Basket   | ContainerTarget   | Containment            | No                 |

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

This namespace is used for:

* Object instances
* Group-specific individuals
* Additional task definitions
* Local ontology extensions

---

## 6. Instructions for Running the Query

### Using Apache Jena

Run the following command:

```bash
arq \
--data ontology/inferred-results.ttl \
--query queries/graspable_objects.rq
```

### Using Protege

1. Open `ontology/group-ontology.ttl`
2. Run an OWL reasoner (e.g., HermiT)
3. Export the inferred ontology
4. Execute the SPARQL query in the SPARQL tab

---

## 7. Expected Query Output

The query retrieves inferred instances of `cap:GraspableObject`.

Expected results:

```text
blueCup01
pinkCup01
knife01
fork01
block01
block02
block03
```

The exact output format may vary depending on the SPARQL engine.

---

## 8. What Is Inferred (Not Merely Asserted)

The ontology does not manually assert:

```ttl
blueCup01 rdf:type cap:GraspableObject .
```

Instead, graspability is inferred through the OWL definition:

```text
GraspableObject ≡
PhysicalObject
⊓ ∃ hasAffordance.GraspingAffordance
```

Objects that possess a GraspingAffordance are automatically classified as GraspableObject by the reasoner.

Examples of inferred classifications:

* blueCup01
* pinkCup01
* knife01
* fork01
* block01
* block02
* block03

---

## 9. Generation of ontology/inferred-results.ttl

The ontology was loaded into an OWL reasoner and classified using OWL reasoning.

The reasoner inferred additional class memberships, including memberships in `cap:GraspableObject`.

The resulting inferred graph was exported and saved as:

```text
ontology/inferred-results.ttl
```

This file contains both asserted facts and inferred triples generated during reasoning.

---

## 10. Repository Resources

| Resource                 | Location                               |
| ------------------------ | -------------------------------------- |
| Main Ontology File       | ontology/group-ontology.ttl            |
| Inferred Ontology        | ontology/inferred-results.ttl          |
| Imported Course Ontology | ontology/imports/course-affordance.ttl |
| SPARQL Query             | queries/graspable_objects.rq           |
| Query Result Output      | results/graspable_objects_output.txt   |
| Source Code              | src/                                   |
| Report                   | report.md                              |

---

## Repository Structure

```text
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
│   └── graspable_objects_output.txt
└── src
```

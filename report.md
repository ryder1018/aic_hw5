# Homework 5 Report

## Ontology-based Semantic Grounding

### Group 15

**Members**

* 吳宇藤
* 呂柏翰
* 張晉睿
* 王韋凱
* 葉哲睿
* 馬晨瑜

---

# 1. Repository Contents

The repository contains all artifacts required for Homework 5.

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
│   ├── graspable_objects_output.txt
│   └── task_objects_output.txt
```

The ontology directory contains the main ontology and the inferred ontology generated after reasoning. The queries directory contains the SPARQL query used for evaluation, while the results directory stores the corresponding query outputs.

---

# 2. Ontology Design

The ontology models the baseline manipulation environment used throughout the AI Capstone course. The design focuses on representing objects, task roles, and manipulation affordances in a way that supports semantic reasoning.

The ontology is organized into four conceptual layers:

1. Object Types
2. Task Roles
3. Affordances
4. Environment Instances

Object types describe what an object physically is, task roles describe how an object participates in a task, and affordances describe the possible interactions a robot can perform on the object.

This separation allows semantic reasoning to infer higher-level concepts such as graspability.

---

# 3. Design Rationale

The primary goal of the ontology is to enable ontology-based semantic grounding rather than simple object labeling.

Instead of directly asserting that an object is graspable, the ontology represents manipulation knowledge through affordances. A reasoner can then determine whether an object should be classified as a graspable object.

This design improves modularity and reusability because the same object may participate in different tasks while retaining the same affordance information.

For example:

* Cups are modeled as physical objects with grasping and stacking affordances.
* Toy blocks are modeled as collectable objects with grasping affordances.
* Plates provide support affordances.
* Baskets provide containment affordances.

---

# 4. Namespace Policy

The ontology uses two namespaces.

## Course Ontology Namespace

```ttl
@prefix cap: <https://hcis.io/ontology/aicapstone/2026/> .
```

This namespace contains the vocabulary provided by the course ontology, including:

* PhysicalObject
* GraspableObject
* Affordance
* TaskRole
* hasAffordance
* hasTaskRole

## Group Ontology Namespace

```ttl
@prefix g15: <https://hcis.io/ontology/aicapstone/2026/group15/> .
```

This namespace contains all entities introduced by Group 15, including:

* Environment instances
* Task-role individuals
* Affordance individuals
* Group metadata

The ontology imports the course ontology using:

```ttl
owl:imports <https://hcis.io/ontology/aicapstone/2026> .
```

allowing the group ontology to reuse the shared course vocabulary while maintaining a separate namespace for project-specific entities.

---

# 5. Reused and Newly Introduced Terms

Reused Terms

The ontology reuses concepts defined in the course ontology.

Classes
cap:PhysicalObject
cap:Cup
cap:Knife
cap:Fork
cap:Plate
cap:ToyBlock
cap:Basket
cap:TargetObject
cap:ReferenceObject
cap:ContainerTarget
cap:CollectableObject
cap:GraspableObject
Properties
cap:hasAffordance
cap:hasTaskRole
Affordance Classes
cap:GraspingAffordance
cap:StackabilityAffordance
cap:SupportAffordance
cap:ContainmentAffordance
Newly Introduced Terms

Group 15 introduces ontology individuals for task roles, affordances, and environment objects.

Object Instances
g15:blueCup01
g15:pinkCup01
g15:knife01
g15:fork01
g15:plate01
g15:block01
g15:block02
g15:block03
g15:basket01
Task Role Individuals
g15:targetObjectRole
g15:referenceObjectRole
g15:containerTargetRole
g15:collectableObjectRole

These individuals are typed as the corresponding course ontology role classes and are used as values of the cap:hasTaskRole property.

Affordance Individuals
g15:graspingAffordance
g15:stackingAffordance
g15:supportAffordance
g15:containmentAffordance

These individuals are typed as subclasses of cap:Affordance and are shared among multiple environment objects.

---

# 6. Key Axioms and Restrictions

The ontology relies on the OWL definitions provided by the course ontology.

The most important concept is the definition of `cap:GraspableObject`.

Objects possessing a grasping affordance satisfy the semantic conditions required to be classified as graspable.

The ontology uses object-property assertions such as:

```ttl
g15:blueCup01
    cap:hasAffordance g15:graspingAffordance .
```

and

```ttl
g15:graspingAffordance
    rdf:type cap:GraspingAffordance .
```

to enable OWL reasoning.

As a result, graspability is derived rather than manually assigned.

---

# 7. Reasoning Pattern

The ontology follows a semantic classification workflow:

```text
Object Instance
      ↓
Assigned Affordance
      ↓
OWL Reasoning
      ↓
GraspableObject Classification
```

For example:

```text
blueCup01
      ↓
hasAffordance
      ↓
GraspingAffordance
      ↓
Inferred as GraspableObject
```

The same reasoning pattern applies to forks, knives, and toy blocks.

This demonstrates ontology-based semantic grounding because semantic properties emerge from the ontology structure rather than manual labeling.

---

# 8. Query and Results

The repository includes the following SPARQL query:

```text
queries/graspable_objects.rq
```

The query retrieves all objects inferred to be instances of `cap:GraspableObject`.

The resulting objects are:

* block01
* block02
* block03
* blueCup01
* pinkCup01
* fork01
* knife01

These results are stored in:

```text
results/graspable_objects_output.txt
```

The results confirm that the ontology reasoner successfully classified graspable objects according to their affordances.

---

# 9. Design Choices

Several design decisions were made during ontology development.

## Separation of Object Type and Task Role

An object's physical type is modeled independently from its role in a task.

For example:

* plate01 is a Plate.
* plate01 also plays the role of ReferenceObject.

This avoids mixing physical identity with task-specific semantics.

## Explicit Affordance Modeling

Affordances are represented as explicit ontology entities rather than encoded directly in object classes.

This allows affordances to be reused and extended.

## Reasoning-Based Classification

The ontology relies on OWL reasoning to derive classifications.

This approach better demonstrates semantic grounding than manually asserting object categories.

---

# 10. Limitations

The ontology focuses only on the baseline environment specified in the homework.

Several limitations remain:

* No geometric information is represented.
* No object dimensions or weights are modeled.
* No robot-specific gripper constraints are represented.
* No temporal task sequencing is included.
* No uncertainty or perception confidence values are modeled.

These aspects would be necessary in a full robotic knowledge representation system.

---

# 11. Additional Discussion

One challenge during development was deciding how task roles should be represented.

The course ontology provides role concepts, while our ontology introduces role individuals that are assigned to environment objects. This approach keeps task-role assignments explicit and easy to query.

Another challenge was ensuring that graspability was inferred rather than manually asserted. The final ontology successfully demonstrates reasoning-based classification, which is the primary objective of this homework.

---

# 12. Conclusion

This project demonstrates an ontology-based semantic grounding workflow for robotic manipulation tasks.

The ontology models object types, task roles, and affordances, and uses OWL reasoning to infer graspable objects automatically. The resulting semantic classifications are successfully retrieved using SPARQL queries.

The project satisfies the homework requirements while illustrating how ontology engineering can support interpretable and reusable robotic knowledge representations.

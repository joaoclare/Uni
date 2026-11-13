# Practical Lesson 8 - Hierarchical Modeling (continued)

## Introduction

This session is a second take on [Practical Lesson 7](./lab07-en.md). There
are no new exercises: use it to finish ex23-ex28, to clear up whatever was
left unclear, and to make sure that hierarchical modeling is solid before
you need it in Project 2, which relies on it heavily.

If you have not started yet, begin with the
[startup code](../../src/labs/ex23-step0/) and the Preparation section of
Lab 7.

## Where to resume

Pick up from the first exercise you have not finished:

- **ex23-ex24** (Sun, Mercury and Venus): the minimum for this session is a
  working solar system with these three bodies, drawn from a scene graph that
  you drew on paper first.
- **ex25-ex26** (Earth and Moon): the first graph with more than two levels.
  This is the one that most often goes wrong; see the checklist below.
- **ex27** (complete solar system): mostly repetition once ex26 works.
- **ex28** (robot arm): the most important one for Project 2. If time is
  short, skip ex27 and do this one.

## Checklist

Most problems in Lab 7 come from a handful of mistakes. Before asking for
help, check your code against each of them:

1. **Draw the graph first.** Every node holds one transformation, every edge
   is a parent-child relation. If the graph is right, the code is a direct
   translation of it; if the code is wrong, compare it with the graph, node by
   node.
2. **Order of the transformations.** With the matrix stack, the
   transformation you write *last* is the one applied *first* to the object's
   vertices. To put a planet in orbit, its vertices must first be moved out
   to its distance from the Sun and only then rotated around the Sun: in the
   code, `multRotationY` comes before `multTranslation`.
3. **`pushMatrix()` and `popMatrix()` come in pairs.** Push before each child
   (each branch of the graph), pop after it. A missing pop shows up as bodies
   that inherit transformations they should not have, such as a Venus that
   orbits Mercury.
4. **Scale only at the leaves.** The scale that gives a body its size must not
   reach its children: apply it inside a push/pop, just before drawing the
   primitive. Otherwise the Moon's orbit grows with the Earth's diameter.
5. **Rotation of a body vs. its orbit.** A planet spinning around its own axis
   and a planet orbiting the Sun are two different rotations, at two different
   levels of the graph, with two different periods. The Moon must orbit the
   Earth's centre, not follow the Earth's spin.
6. **Units.** `time` is in days. Each angle is `360 * time / period`, with the
   period in days too.
7. **Viewing volume.** If something disappears as you add planets, it is
   probably outside the volume set by the projection: adjust the limits
   (and the near/far planes) to the size of the scene.

## For those who have finished

Extend your robot arm (ex28):

- Enforce limits on every degree of freedom and make sure the controls
  respect them.
- Add a gripper with two fingers that open and close symmetrically.
- Compute, in world coordinates, the position of the tip of the gripper (the
  origin of its coordinate system), and show it on screen. Then do the
  opposite: given a point in world coordinates (for example, the centre of a
  small cube placed on the floor), find its coordinates in the gripper's
  coordinate system. Both conversions are needed in Project 2.

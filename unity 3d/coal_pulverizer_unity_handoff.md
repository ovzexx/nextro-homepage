# Unity 3D Coal Pulverizer Handoff Report

Date: 2026-07-07
Project path: `/Users/topjh/Desktop/codex office/CoalPulverizer3D`
Target handoff folder: `/Users/topjh/Desktop/넥스트로 /unity 3d`

## Current Goal

Build a higher-quality Unity 3D educational model of a thermal power plant coal pulverizer, specifically a vertical spindle / bowl mill style pulverizer. The model should visually communicate the actual grinding structure, not just a rough diagram.

The user has been carefully reviewing visual accuracy. The most important current correction is the roll tire geometry and orientation.

## Important User Feedback

The user rejected earlier versions for these reasons:

- Roll tires looked like only one tire, or were hidden by camera/layout.
- Roll tires were incorrectly modeled as torus-like rings or generic grinding rolls.
- Roll tires were incorrectly laid flat like horizontal disks.
- Roll tires were also incorrectly too vertical at one point.
- The correct principle: three roll tires contact and roll on the bowl/bull grinding ring. The outer circumference of each roll tire is the contact surface.
- The roll tire upper side should lean inward/forward toward the center, so the three upper parts nearly approach each other.
- Raising the classifier cone and upper housing is necessary to avoid interference after increasing roll tire inward tilt.
- The model should be verified critically against real coal pulverizer/bowl mill structure, not adjusted only by agreeing with the user.

## Learned Structure

Reference machine family:

- Vertical spindle coal pulverizer
- Bowl mill / roll wheel pulverizer
- B&W Roll Wheel / E/EL style concepts
- Raymond / CE bowl mill style concepts

Core operating principle:

1. Raw coal enters through a central raw coal feed pipe.
2. Coal drops onto the rotating bowl/grinding table.
3. The bowl rotation pushes coal outward by centrifugal force.
4. Three roll wheel / roll tire assemblies press onto the bull grinding ring.
5. The outer circumference of each roll tire contacts the bull ring and grinds coal by rolling/pressing.
6. Primary air enters below the bowl through windbox/throat/nozzle ring and carries pulverized coal upward.
7. Classifier separates fine coal from coarse coal.
8. Fine coal exits through multiple outlet pipes.
9. Coarse coal/reject falls back to the grinding zone.
10. Heavy pyrites/tramp material drops below the throat and is removed by scraper/reject system.

## Critical Geometry Rules

### Roll Tire

- There must be three roll tires, usually 120 degrees apart.
- Roll tire is not a flat disk lying on the bowl.
- Roll tire is not a decorative torus.
- Roll tire contact must occur at the tire's outer cylindrical/crowned circumference.
- Side faces of the tire should not be the contact surface.
- Roll wheel axis follows the journal direction from the outer housing side toward the bowl center.
- Current implementation uses a radial axis with inward/upper tilt:
  `rollAxis = (radial + Vector3.up * 0.48f).normalized`
- This was added because the user wanted the upper parts of all three tires to lean forward/inward, almost toward one another.

### Bowl / Bull Ring

- Bowl rotating assembly contains bowl hub, bowl table, bull ring, throat/nozzle ring, scraper, and coal bed.
- Bull ring should be an annular track near the outer upper bowl.
- Current name in code:
  `Sloped High Chrome Bull Ring - roll tire running track`
- Roll tire lower outer circumference should visually meet this bull ring.

### Classifier Cone

- Previous cone direction was wrong.
- Current upper classifier cone was raised to avoid interference with strongly tilted roll tires.
- Current names:
  - `Raised Ceramic Lined Inner Classifier Cone - wide top narrow bottom`
  - `Raised Coarse Coal Return Cone Below Feed Pipe`

### Upper Body

- After increasing roll tire inward/forward tilt, upper housing and classifier were moved upward.
- Current names include:
  - `Tall Thick Cutaway Pressure Housing Shell`
  - `Tall Transparent Rear Inspection Envelope`

## Current Unity Files

Main implementation:

- `CoalPulverizer3D/Assets/Scripts/CoalPulverizerBuilder.cs`

Camera:

- `CoalPulverizer3D/Assets/Scripts/OrbitCamera.cs`

Editor scene creation:

- `CoalPulverizer3D/Assets/Scripts/Editor/CoalPulverizerSceneCreator.cs`

Unity project:

- `CoalPulverizer3D`

## Current Important Code Points

### Roll placement

In `CoalPulverizerBuilder.cs`, `BuildGrindingZone()`:

```csharp
float[] rollAngles = { 270f, 30f, 150f };
for (int i = 0; i < rollAngles.Length; i++)
{
    AddRollWheelAssembly(parent, i + 1, rollAngles[i]);
}
```

This intentionally creates three visible roll tire assemblies.

### Roll axis and tilt

In `AddRollWheelAssembly()`:

```csharp
Vector3 radial = Direction(angle);
Vector3 tangent = new Vector3(-radial.z, 0f, radial.x).normalized;
Vector3 rollAxis = (radial + Vector3.up * 0.48f).normalized;
Vector3 wheelCenter = radial * 1.48f + Vector3.up * 4.02f;
Vector3 journalCenter = wheelCenter + rollAxis * 0.82f;
```

The `Vector3.up * 0.48f` is the most recent correction. It makes the top of the roll wheel lean inward/forward toward the other rolls.

### Roll mesh

Roll tire uses a lathe mesh, not a torus:

```csharp
Transform tire = AddLathe(
    assembly,
    "Replaceable Asymmetric Roll Tire " + index,
    wheelCenter,
    MeshFactory.RollTireProfile(),
    yellow,
    96
);
tire.localRotation = Quaternion.FromToRotation(Vector3.up, rollAxis);
```

Mesh profile:

```csharp
public static Vector2[] RollTireProfile()
{
    return new[]
    {
        new Vector2(-0.38f, 0.42f),
        new Vector2(-0.34f, 0.52f),
        new Vector2(-0.24f, 0.61f),
        new Vector2(0f, 0.66f),
        new Vector2(0.24f, 0.61f),
        new Vector2(0.34f, 0.52f),
        new Vector2(0.38f, 0.42f)
    };
}
```

This creates a crowned/barrel roll tire.

## How To Open And Rebuild

1. Open Unity Hub.
2. Open project:
   `/Users/topjh/Desktop/codex office/CoalPulverizer3D`
3. Wait for scripts to compile.
4. Use menu:
   `Tools > Coal Pulverizer > Rebuild Current Model`
5. Open/play `Assets/Scenes/CoalPulverizerDemo.unity`.

The scene creator intentionally recreates the demo scene to avoid stale missing-script components from previous procedural generations.

## Current Known Risks

- Unity batch validation from terminal often stalls on Unity licensing client, so final verification has been done mostly by source/log inspection and user visual feedback.
- Previous scenes had `The referenced script (Unknown) on this Behaviour is missing!` in Editor logs. The rebuild path now recreates the scene, but if Unity still opens an old backup scene, user may need to close/reopen Unity and run rebuild.
- The model is still procedural and approximate. It is not a CAD-accurate replica.
- The next quality jump should be based on direct visual comparison with real bowl mill / B&W roll wheel cutaway references.
- Roll tire contact height may still need fine tuning in Unity viewport after the latest tilt change.

## Strict Next-Step Checklist

Before claiming success, verify visually in Unity:

- [ ] Exactly three roll tires are visible.
- [ ] Roll tires are 120-ish degrees apart.
- [ ] Each tire axis follows journal direction from outer housing toward bowl center.
- [ ] The tire's outer circumference, not side face, is the contact surface.
- [ ] Tire lower outer circumference visually touches bull ring.
- [ ] Tire upper portions lean inward/forward toward center.
- [ ] The three upper tire regions appear closer to one another than the lower contact regions.
- [ ] Classifier cone and upper housing do not intersect the tilted tires.
- [ ] Bowl/bull ring/throat rotate together in Play mode.
- [ ] Roll tires rotate around their own tilted axes in Play mode.
- [ ] Camera clearly shows the three roll tires and bull ring contact geometry.
- [ ] Labels do not obscure the roll tire contact geometry.

## Useful Reference URLs

- Babcock & Wilcox pulverizer/mill upgrades:
  https://www.babcock.com/home/products/pulverizer-mill-upgrades

- POWER Magazine pulverizer performance article:
  https://www.powermag.com/blueprint-your-pulverizer-for-improved-performance/

- Bowl mill structure overview:
  https://engineeringworlds.com/bowl-mill/

- Pulverized coal-fired boiler / vertical spindle overview:
  https://en.wikipedia.org/wiki/Pulverized_coal-fired_boiler

## Important Conversation Context

The user expects a critical, research-based engineering/design approach. Do not simply agree with the user and patch randomly. When changing geometry, state the mechanical reason and verify against bowl mill principles.

The most recent user request before this handoff was:

- The roll tire upper part was leaning backward.
- It should lean forward/inward.
- All three tire tops should nearly approach each other.
- To avoid interference, stretch the upper cone and cylinder/housing upward.

This has been implemented with:

- `rollAxis = (radial + Vector3.up * 0.48f).normalized`
- Raised classifier cone/cage/top assembly/outlet pipes.
- Taller cutaway housing.

# Project 3: CAD Agent

> 本地 CAD Agent 最小实现：[项目说明](README_DEMO.md) · [安装与启动](SETUP_GUIDE.md)。

English | [简体中文](README-zh.md)

## Objective

Develop a CAD modeling agent based on SimpleCADAPI (SCA). Given a user's natural-language request, it should understand requirements, plan the modeling work, invoke tools, inspect the result, and revise it when necessary. The outcome should be a viewable, parameter-editable, exportable 3D model produced through a runnable and reproducible interactive workflow.

## Basic tasks

- **Understand requirements:** Accept a modeling request through a command line or simple interface. Extract its structure, dimensions, units, and spatial relationships. If key conditions are missing or contradictory, ask the user to confirm them before delivering a model.
- **Execute modeling:** Have a language model plan the steps, then use tool calls or generated programs to drive SCA. Create solids, perform Boolean operations, and use at least one feature operation such as extrusion, filleting, or patterning. Parameterize key dimensions. Returning code alone or loading a prefabricated model is insufficient.
- **Inspect and repair:** Check solid validity and measure key dimensions and spatial relationships. Feed runtime errors or inspection results back to the agent so it can revise and rerun the model. Set a retry limit; the absence of an exception does not establish success.
- **Modify interactively:** Support natural-language changes to dimensions or local features within the same session, preserving requirements the user has not changed. Show the model before and after modification, save the parametric modeling source, export a STEP file, and retain the conversation, tool calls, and execution feedback.
- **Test and analyze:** Test at least three structurally different modeling tasks that cover requirement clarification, multi-turn modification, and repair after error feedback. Define acceptance criteria and dimensional tolerances. Record success, dimension deviations, runtime, and repair attempts, then analyze failures.

## Advanced tasks

The basic task may be limited to text-driven modeling of a single part with one agent. Training a language model, developing a geometry kernel, and implementing complex assemblies are not required. Optional directions include combined image-and-text input, assembly modeling, and reuse of modeling experience. The complexity of the models produced is also part of the evaluation.

You may consult SCA documentation, Agent Skills, and open-source agent frameworks, but you must implement the tool wrappers, modeling workflow control, and result checks yourself. If you use an existing framework, explain which modules you developed or improved and provide validation results. Merely running an existing example or calling an existing CAD agent does not complete the assignment.

## Tool selection

CAD modeling must use SCA; see its [official repository and API documentation](https://github.com/PhySpace/SimpleCADAPI). Use **GLM-5.3-Flash** to complete the course tasks. You may additionally demonstrate results from other models on the same tasks and provide a comparative analysis. Choose your own interaction interface and model viewer; a custom 3D renderer is not required. Record software and model versions, run generated code in a restricted environment, and set an execution timeout.

## Submission

Submit the agent code, prompts and tool definitions, test requests and acceptance results, parametric modeling source files and STEP models, execution logs, configuration and run instructions, and a demonstration video. A four-to-six-page experimental report is recommended. The video should show request input, automatic modeling, feedback-driven repair, and multi-turn modification. The report should describe the system design, test results, original work, and team responsibilities.

## Example task: phone stand

Begin with the vague request “Make a small stand for watching videos on a desk.” Clarify the device dimensions, placement, support angle, and charging needs through interaction, then produce a phone stand viewable in an editor. The following images show selected steps.

The user first describes the intended use. The agent asks what device the stand will support.

<p align="center">
  <img src="images/initial-request.png" alt="User&#x27;s initial request for a small stand"><br>
  <em>Figure 1: The user&#x27;s initial request</em>
</p>

<p align="center">
  <img src="images/device-clarification.png" alt="Agent asking which device the stand should support"><br>
  <em>Figure 2: Clarifying the device type</em>
</p>

The user specifies a phone with its case, approximately 160 × 78 × 12 mm, and asks for both portrait and landscape use. The agent suggests a support angle of about 65° and makes clear that this suggestion can be changed.

<p align="center">
  <img src="images/dimensions-and-angle.png" alt="Device dimensions and suggested support angle"><br>
  <em>Figure 3: Completing dimensions and suggesting an angle</em>
</p>

The user adds a portrait charging scenario, requests a clearance notch in the bottom lip, and confirms a fixed, one-piece structure.

<p align="center">
  <img src="images/charging-clearance.png" alt="Additional charging and local-geometry requirements"><br>
  <em>Figure 4: Adding a use case and local feature requirements</em>
</p>

The agent builds the model from those requirements and displays the result in an editor. Inspect the relationship among the inclined support, base, bottom lip, and charging clearance to check that the geometry matches the request.

<p align="center">
  <img src="images/phone-stand-result.png" alt="Completed phone stand in the CAD editor"><br>
  <em>Figure 5: Completed phone stand in the editor</em>
</p>

The project should demonstrate the process from clarification to a completed model, distinguish user-specified parameters from agent suggestions, and continue with a parameter change and inspection feedback.

# References

External repositories imported as git submodules for Clanker research, integration planning, and implementation guidance.

## Boundary

Treat everything under `references/` as read-only input.
Do not edit code in these reference working trees during normal Clanker development.
Implement Clanker-specific adaptations in Clanker-owned repositories or submodules.

## Imported References (April 2026)

### references/hardware_exercises

- Why imported: Raspberry Pi hardware integration exercises and bring-up examples.
- Consult first: hardware setup docs, display/peripheral driver examples, and integration scripts.

### references/sd1.5-lcm.axera

- Why imported: AXERA Stable Diffusion 1.5 LCM model/runtime reference for image generation.
- Consult first: model packaging/export notes, runtime invocation examples, and acceleration-specific setup files.

### references/raspberry_pi_hailo_ai_services

- Why imported: Hailo-enabled Raspberry Pi service patterns and AI pipeline references.
- Consult first: service entrypoints, pipeline orchestration modules, and hardware/runtime setup docs.

### references/ollama_gateway

- Why imported: local gateway patterns for routing model requests to Ollama-based backends.
- Consult first: API surface definitions, request/response handling modules, and deployment configuration.

### references/pi_axera_sd_interactive

- Why imported: interactive Pi + AXERA Stable Diffusion integration reference.
- Consult first: interactive loop/controller code, generation workflow scripts, and model/runtime configuration.

### references/ax650_raspberry_pi_services

- Why imported: AX650-oriented Raspberry Pi service architecture and operational examples.
- Consult first: service composition/orchestration code, runtime wrappers, and device-specific setup docs.

### references/axcl-docs

- Why imported: AXCL platform documentation and API behavior reference.
- Consult first: API docs, runtime/operator guidance, and hardware/software compatibility notes.

### references/ax-llm

- Why imported: AXERA-focused LLM runtime integration examples and interfaces.
- Consult first: model loading/inference interfaces, runtime adapters, and usage examples.

### references/IT8951

- Why imported: IT8951 e-paper controller reference implementation and hardware protocol examples.
- Consult first: controller communication code, display update routines, and hardware interface docs.

### references/notyou

- Why imported: project-specific reference material for patterns potentially relevant to Clanker workflows.
- Consult first: architecture notes, service interfaces, and integration scripts relevant to Pi node workflows.

### references/axcl-samples

- Why imported: AXCL sample applications for platform capabilities and expected usage.
- Consult first: sample app entrypoints, API usage patterns, and build/run instructions.

# Updating the FreeRTOS kernel

## Import the release

1. Download the latest stable release asset from the [FreeRTOS-Kernel releases](https://github.com/FreeRTOS/FreeRTOS-Kernel/releases).
   Compare the bundled and extracted `include/task.h` versions and review the changes in `History.txt`.
2. Directory `Source/` is the destination for FreeRTOS kernel asset files. Replace its contents
   with the extracted release and remove obsolete files.
   Throughout the import exclude: `.git*`, `*.spdx*`.
   Verify that the remaining files match the release byte for byte.

## Review compatibility and update metadata

- Check API and behavior changes, configuration and object layouts, and ports used by
  CMSIS-RTOS2, including TrustZone and Event Recorder. If an incompatibility affects
  the wrapper, show the trigger and impact with code references and ask the user
  before changing wrapper behavior. Continue independent work.
- Update kernel versions and release notes in `ARM.CMSIS-FreeRTOS.pdsc`, the version
  and download link in `README.md`, and the version in `CMSIS/RTOS2/FreeRTOS/FreeRTOS.scvd`.
  Keep the pack at `-dev` until release. Change the CMSIS API version only for an API revision.
- Bump a PDSC `attr="config"` file version only when that file's configuration options change.
- Update affected sources in `Documentation/Doxygen/src/`. The pack workflow runs
  `Documentation/Doxygen/gen_doc.sh` to generate the documentation.

## Validate the pull request in GitHub Actions

- Verify the PDSC paths and include all intended changes and new release files in
  the update pull request. Validate its latest revision using the existing workflows.
  - [Build pack](.github/workflows/pack.yml) generates the documentation and CMSIS pack and checks the pack with `packchk`.
  - [Build examples](.github/workflows/Build_Examples.yml) builds the example targets against the updated local pack.
  - [RTOS2 Validation](.github/workflows/cmsis_rv2.yml) builds and runs the CMSIS-RTOS2 test suite against the updated local pack.
- If GitHub asks for approval on a Copilot pull request, ask a maintainer to approve
  and run the workflows. Report validation as pending until results are available.
- Check the published Test Results check and JUnit reports even when the workflow is
  green. Compare failures and skips with `Documentation/Doxygen/src/technical_data.md`
  and `cmsis_rtos2_validation.txt`. Check why a test was skipped before documenting
  a feature as unsupported.
- Summarize the old and new versions, compatibility findings, and CI outcomes on
  the pull request. Link the runs and call out any failed, skipped, or pending checks.

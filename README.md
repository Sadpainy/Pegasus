# Pegasus

![Build](https://img.shields.io/badge/Build-unstable-yellow?style=plastic&labelColor=555555)
![Tests](https://img.shields.io/badge/Tests-passing-brightgreen?style=plastic&labelColor=555555)

# A Technical Research Artifact

Licensed under the GNU Affero General Public License version 3.0, with additional terms as set forth in LICENSE.Pegasus.

---

# Overview

Pegasus is a technical research artifact. It contains the reconstructed, readable source code of a multi-stage Android software suite that was obtained and analyzed during the course of security research. The artifact is published for the purposes of education, defensive research, and the advancement of the public understanding of advanced mobile threats.

The suite that is documented by this artifact is not a conventional application. It is a layered system composed of several interdependent components, each of which performs a distinct function in the overall operation of the suite. The artifact presents these components in a form that is intended to be readable by a competent reverse engineer, with the original obfuscation removed and the control flow rendered in a form that reflects the intent of the original author.

The artifact is not a working exploit. It is a documentation of an exploit. The distinction is important. The artifact is intended to support defensive research, not offensive operations.

---

# Purpose

The purpose of this artifact is threefold.

First, the artifact exists to document the technical mechanisms by which an advanced Android threat achieves privilege escalation, persistence, process injection, and data exfiltration. Such documentation is a prerequisite for the development of effective defensive measures.

Second, the artifact exists to support the education of security researchers who wish to understand the techniques that are used in advanced mobile threats. The techniques that are documented in this artifact are not novel, but their combination in a single suite is instructive.

Third, the artifact exists to support the development of detection and mitigation strategies. The indicators of compromise that are documented in this artifact are intended to be used by defenders to identify the presence of the suite or of related threats on a compromised device.

The artifact is not intended to support the development of new offensive capabilities. The copyright holder expressly prohibits the use of this artifact for the purposes of surveillance, espionage, or the development of malicious software, as set forth in the additional terms of the license.

---

# Contents

The artifact is organized into a set of source files, each of which corresponds to a component of the analyzed suite. The files are presented in the language and form that is most appropriate for the component in question. Java components are presented as Java source. Native components are presented as C or C++ source.

The components are as follows.

The SMS receiver component is presented as SmsReceiver.java. This component registers for the android.intent.action.DATA_SMS_RECEIVED broadcast and dispatches command processing to a background thread. It is the entry point for remote commands.

The command processor is presented as SmsReceiver$1.java. This component parses the incoming protocol data unit and interprets the command identifier contained within.

The main activity is presented as SkeletonActivity.java. This component presents a diagnostic user interface and orchestrates privilege escalation, filesystem access, and file transfer.

The file utilities are presented as SystemUtils.java. This component provides file and directory copy operations.

The logging utility is presented as Logger.java. This component provides a diagnostic console and uses the log tag "pegasus".

The kernel exploit is presented as exploit.c. This component targets the Exynos memory device and replaces the sys_setresuid syscall table entry to achieve privilege escalation.

The process injector is presented as addk.c. This component implements a ptrace-based shared-library injector for ARM32 processes.

The C++ runtime is presented as WindowsShell.c. This component is a standard libgcc unwinding runtime that is bundled with the native payload to support C++ exception handling.

The Binder hook is presented as binder_hook.c. This component hooks android::IPCThreadState::transact and extracts input-method data from incoming Binder parcels.

The encoding utilities are presented as y_q.java, z_q.java, and z_r.java. These components provide Base64 encoding with custom alphabets and object serialization.

---

# Architecture

The suite is organized in layers. Each layer assumes a distinct responsibility, and the layers are combined in a manner that achieves a level of access that no single layer could achieve alone.

At the outermost layer, the SMS receiver and the command processor provide a command channel that is independent of IP connectivity. The channel is implemented over Port-0 binary SMS, which is delivered by the network infrastructure and is not surfaced in the standard messaging user interface.

At the middle layer, the main activity provides orchestration. It performs recursive filesystem permission modification, file copying, system remounting, and invocation of the custom privilege-elevation binary.

At the innermost layer, the kernel exploit, the process injector, and the Binder hook provide the capabilities that are required to obtain root privileges, to place code into privileged processes, and to intercept inter-process communication.

The encoding utilities and the C++ runtime are supporting components that are used by the other layers.

---

# Analysis

The analysis that is documented by this artifact was conducted using static reverse engineering techniques. The artifact was produced by decompiling Java bytecode, disassembling native ARM32 machine code, and reconstructing readable source from the resulting intermediate representations.

No live execution of the sample was performed. All observations derive from code inspection and structural reasoning. Where runtime behavior is inferred, it is explicitly identified as such in the accompanying documentation.

The artifact is intended to be readable by a competent reverse engineer. The original obfuscation has been removed where possible, and the control flow has been rendered in a form that reflects the intent of the original author. Where obfuscation could not be removed without losing information, it has been documented in comments.

The artifact is not a byte-for-byte reconstruction of the original. It is a functional reconstruction. Where the original used reflection, the artifact uses direct calls. Where the original used encoded strings, the artifact uses decoded strings. Where the original used randomized names, the artifact uses descriptive names.

---

# Indicators of Compromise

The following artifacts are associated with the analyzed suite. They are provided to support detection and incident response.

Filesystem paths that are associated with the suite include /system/csk, which is the custom privilege-elevation binary, and /data/local/tmp/ktmu/, which contains the staging files for intercepted input. Staging files carry the names ulmndd.tmp and finidk.<timestamp>.

The broadcast action android.intent.action.DATA_SMS_RECEIVED is monitored by the suite. The presence of a receiver for this action in an application that does not legitimately implement an SMS-based service is anomalous.

The interface identifier com.android.internal.view.IInputContext is referenced by the Binder hook component. Reference to this interface within an application that is not part of the platform input-method framework is anomalous.

The log tag pegasus is used by the logging component. The presence of this tag in logcat output is a strong indicator of the suite's presence.

The device node /dev/exynos-mem is opened by the exploit component. The presence of an open file descriptor to this device in a non-privileged process is anomalous.

---

# Usage

This artifact is intended for use by security researchers, defensive analysts, and educators. It is not intended for use in production systems, in offensive operations, or in the development of malicious software.

To use this artifact, clone the repository and review the source files. The source files are self-contained and do not require a build system. Where a component depends on another component, the dependency is documented in comments.

The artifact does not include any executable code. It does not include any build scripts. It does not include any configuration files. It is a documentation of a threat, not a threat itself.

---

# Warning

The techniques that are documented in this artifact are used by advanced threats to compromise mobile devices. The techniques are documented for the purposes of defense and education. The techniques are not documented for the purposes of offense.

Do not use this artifact to compromise devices that you do not own. Do not use this artifact to compromise devices that you do not have permission to test. Do not use this artifact to develop malicious software.

The copyright holder expressly prohibits the use of this artifact for the purposes of surveillance, espionage, or the development of malicious software. Violation of this prohibition constitutes a material breach of the license and may constitute a violation of applicable law.

If you are not a security researcher, a defensive analyst, or an educator, this artifact is not for you. If you do not understand the implications of the techniques that are documented in this artifact, this artifact is not for you.

---

# Contributing

Contributions to this artifact are welcome. Contributions must be consistent with the purpose of the artifact, which is to document the threat for the purposes of defense and education. Contributions that are intended to enhance the offensive capabilities of the artifact will not be accepted.

Contributions must be made in accordance with the license. Contributions must preserve the copyright notice and the additional terms. Contributions must include a description of the changes that have been made.

---

# License

This artifact is licensed under the GNU Affero General Public License version 3.0, with additional terms as set forth in the LICENSE.Pegasus file. The additional terms prohibit the use of this artifact for the purposes of surveillance, espionage, or the development of malicious software, and impose certain other obligations on parties who modify or convey the artifact.

The copyright holder is Sadpainy. The copyright notice "Copyright (C) 2026 Sadpainy" must be preserved in all copies, modified versions, and derivative works of this artifact.

See the LICENSE.Pegasus file for the full text of the license and the additional terms.

---

# Acknowledgments

**pussycat0x**

End of README.

**Copyright (C) 2026 Sadpainy. All rights reserved.**

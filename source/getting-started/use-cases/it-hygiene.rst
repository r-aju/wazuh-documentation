.. Copyright (C) 2015, Wazuh, Inc.

.. meta::
   :description: Wazuh collects system inventory data that includes hardware and operating system information, installed software, network interfaces, ports, and running processes. Find more information in this use case.

IT hygiene
==========

IT hygiene refers to the measures that organizations and individuals undertake to maintain the health and security of their IT assets. IT hygiene requires continuous adaptation of practices and processes to counter emerging cybersecurity threats and challenges, fostering a secure and resilient IT environment. Organizations implement robust IT hygiene practices to prevent cyberattacks, data breaches, and other security concerns that may result in data loss, service disruption, reputational harm, or financial instability.

.. _system_inventory_gs_use_case:

System inventory
----------------

An up-to-date :doc:`system inventory </user-manual/capabilities/system-inventory/index>` helps organizations optimize asset visibility in their environment, and is essential for maintaining good IT hygiene. Wazuh collects system inventory data that includes hardware and operating system information, installed software, network interfaces, ports, and running processes. Wazuh agents use the :doc:`Syscollector </user-manual/capabilities/system-inventory/configuration>` module to collect inventory data from monitored endpoints and send them to the Wazuh server.

You can generate system inventory reports from the **Inventory data** tab on the Wazuh dashboard. The information contained in the report helps identify unwanted applications, processes, services, and malicious artifacts.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/inventory-data-dashboard.png
   :title: Inventory data on the Wazuh dashboard
   :alt: Inventory data on the Wazuh dashboard
   :align: center
   :width: 80%

You can also generate property-specific reports for a monitored endpoint. For example, you can get a report containing the list of installed software or a list of running processes on a monitored endpoint.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/inventory-data-download.png
   :title: Inventory data download
   :alt: Inventory data download
   :align: center
   :width: 80%

The inventory data collected can be queried using the `Wazuh API <https://documentation.wazuh.com/|WAZUH_CURRENT_MINOR|/user-manual/api/reference.html#tag/Syscollector>`__, which retrieves nested data in JSON format. For example, you can query the package inventory to check for the ``wazuh-agent`` package on a monitored endpoint using the **API Console** on the Wazuh dashboard. Command line tools like :ref:`cURL <inventory_wazuh_api_curl>` can also be used to query the inventory database.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/inventory-querying-api.png
   :title: Querying the package inventory using the API console
   :alt: Querying the package inventory using the API console
   :align: center
   :width: 80%

Security Configuration Assessment
---------------------------------

One of the objectives of implementing good IT hygiene is to reduce the attack surface of your organization. The :doc:`Wazuh SCA </user-manual/capabilities/sec-config-assessment/index>` module periodically scans monitored endpoints against policies based on the Center for Internet Security (CIS) benchmarks to identify security misconfigurations and flaws. The CIS benchmarks are essential guidelines for establishing a secure baseline configuration for critical assets. This minimizes vulnerabilities resulting from misconfigurations and reduces the risk of security breaches.

The **Security configuration assessment** module on the Wazuh dashboard provides each agent's SCA scan result. The results show the number of checks performed on the endpoint, how many failed, and the number of checks that passed. It also shows a score calculated based on the number of tests passed, giving you an overview of the level of compliance.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/sca-results.png
   :title: Security configuration assessment results
   :alt: Security configuration assessment results
   :align: center
   :width: 80%

You can gain more insights from the Wazuh dashboard to view the passed and failed checks. Also, you can generate a CSV report to aid remediation activities, thereby improving the endpoint security posture.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/sca-results-details.png
   :title: SCA results details and download
   :alt: SCA results details and download
   :align: center
   :width: 80%

You can see information such as rationale, remediation steps, and description of the checks performed on the endpoint on the Wazuh dashboard. This information is included in the report generated by Wazuh.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/sca-check-result-details.png
   :title: SCA check result details
   :alt: SCA check result details
   :align: center
   :width: 80%

The SCA scan result above indicates a failure because the endpoint allows you to mount the cramfs file system. You can implement the remediation suggested in the report to improve the security posture.

Vulnerability management
------------------------

Vulnerability management aims to identify and remediate vulnerabilities to prevent cyber attacks. By taking proactive steps to remediate vulnerabilities, your organization can significantly reduce its attack surface, thereby improving its IT hygiene.

The Wazuh :doc:`Vulnerability Detector </user-manual/capabilities/vulnerability-detection/index>` module identifies vulnerable applications by using the information collected from operating system vendors and :doc:`vulnerability databases </user-manual/capabilities/vulnerability-detection/how-it-works>`. The Vulnerability Detector module scans and generates alerts for vulnerabilities discovered on monitored endpoints. This provides a comprehensive view of vulnerabilities identified across all monitored endpoints, allowing you to view, analyze, fix, and track the remediation of vulnerabilities.

The vulnerabilities discovered are grouped into severity levels, and a summary is provided based on the application name, CVE, and CVSS3 score on the Wazuh dashboard.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/vulnerabilities-inventory-dashboard.png
   :title: Vulnerabilities inventory dashboard
   :alt: Vulnerabilities inventory dashboard
   :align: center
   :width: 80%

You can download a report that contains security events related to discovered and resolved vulnerabilities on a monitored endpoint from the Wazuh dashboard. This feature allows you to identify endpoints with unresolved vulnerabilities and keep track of remediation activities.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/vulnerabilities-data-download.png
   :title: Vulnerabilities data download
   :alt: Vulnerabilities data download
   :align: center
   :width: 80%

The Wazuh Vulnerability Detector module also enables you to track remediation activities, which could serve as a progress report on improving or maintaining IT hygiene. For example, when a vulnerability is remediated, an alert is generated on the Wazuh dashboard. This feature detects when a patch or software upgrade resolves a previously detected vulnerability.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/remediation-alerts.png
   :title: Remediation alerts
   :alt: Remediation alerts
   :align: center
   :width: 80%

Malware detection
-----------------

Malware detection is essential for safeguarding computer systems and networks from cyber threats. Organizations can improve their IT hygiene by identifying and mitigating malicious software that can cause data breaches, system compromises, and financial losses.

Wazuh offers an out-of-the-box ruleset designed to recognize malware patterns and trigger alerts for quick response. Wazuh also allows security analysts to create :doc:`custom rules </user-manual/ruleset/custom>` tailored to their environment, thereby optimizing their malware detection efforts. For example, we created custom rules to detect `Vidar infostealer malware using Wazuh <https://wazuh.com/blog/detecting-vidar-infostealer-with-wazuh/>`__.

.. code-block:: xml

   <group name="windows,sysmon,vidar_detection_rule,">
   <!-- Vidar downloads malicious DLL files on victim endpoint -->
     <rule id="100084" level="10">
       <if_sid>61613</if_sid>
       <field name="win.eventdata.image" type="pcre2">(?i)\\\\.+(exe|dll|bat|msi)</field>
       <field name="win.eventdata.targetFilename" type="pcre2">(?i)\\\\ProgramData\\\\(freebl3|mozglue|msvcp140|nss3|softokn3|vcruntime140)\.dll</field>
       <description>Possible Vidar malware detected. $(win.eventdata.targetFilename) was downloaded on $(win.system.computer)</description>
       <mitre>
         <id>T1056.001</id>
       </mitre>
     </rule>
   <!-- Vidar loads malicious DLL files -->
     <rule id="100085" level="12">
       <if_sid>61609</if_sid>
       <field name="win.eventdata.image" type="pcre2">(?i)\\\\.+(exe|dll|bat|msi)</field>
       <field name="win.eventdata.imageLoaded" type="pcre2">(?i)\\\\programdata\\\\(freebl3|mozglue|msvcp140|nss3|softokn3|vcruntime140)\.dll</field>
       <description>Possible Vidar malware detected. Malicious $(win.eventdata.imageLoaded) file loaded by $(win.eventdata.image)</description>
       <mitre>
         <id>T1574.002</id>
       </mitre>
     </rule>
   <!-- Vidar deletes itself or a malicious process it creates -->
     <rule id="100086" level="7" frequency="5" timeframe="360">
       <if_sid>61603</if_sid>
       <if_matched_sid>100085</if_matched_sid>
       <field name="win.eventdata.image" type="pcre2">(?i)\\\\cmd.exe</field>
       <match type="pcre2">cmd.exe\\" /c timeout /t \d{1,}.+del /f /q \\".+(exe|dll|bat|msi)</match>
       <description>Possible Vidar malware detected. Malware deletes $(win.eventdata.parentCommandLine)</description>
       <mitre>
         <id>T1070.004</id>
       </mitre>
     </rule>
   </group>

The rules above detect specific behaviors of the Vidar infostealer malware and trigger alerts on the dashboard.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/vidar-malware-alerts.png
   :title: Vidar malware alerts
   :alt: Vidar malware alerts
   :align: center
   :width: 80%

Wazuh boosts its malware detection capabilities by :doc:`integrating with threat intelligence </user-manual/capabilities/malware-detection/virus-total-integration>` sources such as VirusTotal, MISP, and more. Wazuh also offers support for integrating third-party malware detection tools such as :doc:`ClamAV </user-manual/capabilities/malware-detection/clam-av-logs-collection>` and :doc:`Windows Defender </user-manual/capabilities/malware-detection/win-defender-logs-collection>`. By collecting and analyzing logs from third-party malware detection tools, Wazuh provides security analysts with a centralized monitoring platform. Wazuh increases the efficiency in detecting malware by combining diverse threat intelligence from third-party tools, thereby improving the organization's IT hygiene.

The image below shows an alert of an event from VirusTotal processed by the Wazuh server.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/virustotal-finding-alert.png
   :title: VirusTotal finding alert
   :alt: VirusTotal finding alert
   :align: center
   :width: 80%

Wazuh uses :doc:`CDB lists </user-manual/ruleset/cdb-list>` (constant databases) containing indicators of compromise (IOCs) to detect malware. These lists contain known malware IOCs such as file hashes, IP addresses, and domain names. Wazuh proactively identifies malicious files by comparing the identified IOCs with the information stored in the CDB lists.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/malware-detected-alert.png
   :title: Malware detected alert
   :alt: Malware detected alert
   :align: center
   :width: 80%

Regulatory compliance
---------------------

Regulatory standards provide a global benchmark for best business practices to help improve customer trust and business reputation. Compliance with regulatory standards also helps organizations to enhance their IT hygiene.

Wazuh streamlines the process of meeting :doc:`regulatory compliance </compliance/index>` obligations by offering a robust solution that addresses requirements of industry standards such as PCI DSS, HIPAA, GDPR, and others.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/regulatory-compliance-module.png
   :title: Regulatory compliance module
   :alt: Regulatory compliance module
   :align: center
   :width: 80%

Wazuh uses its capabilities such as the :doc:`SCA </user-manual/capabilities/sec-config-assessment/index>`, :doc:`Vulnerability Detector </user-manual/capabilities/vulnerability-detection/index>`, :doc:`FIM </user-manual/capabilities/file-integrity/index>`, and more to identify and report compliance violations. It also provides dedicated compliance dashboards to help monitor compliance status, identify improvement areas, and take appropriate remediation actions.

For example, you can get a general overview of the PCI DSS requirement of a monitored endpoint on the Wazuh dashboard.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/pci-dss-dashboard.png
   :title: PCI DSS dashboard
   :alt: PCI DSS dashboard
   :align: center
   :width: 80%

You can drill down to the individual PCI DSS requirement from the **Controls** tab to discover where the policy violations occurred.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/pci-dss-requirement-violations.png
   :title: PCI DSS requirement violations
   :alt: PCI DSS requirement violations
   :align: center
   :width: 80%

The image below shows alerts generated for vulnerabilities that violate the *PCI DSS Requirement 11.2.1*.

.. thumbnail:: /images/getting-started/use-cases/it-hygiene/pci-dss-requirement-violation-details.png
   :title: PCI DSS requirement violation details
   :alt: PCI DSS requirement violation details
   :align: center
   :width: 80%

This feature is also available for other compliance standards such as GDPR, TSC, HIPAA, and  NIST-800-53.
### Threat Model for the Web Platform

#### Use Scenario

The Web Platform is a collection of open (royalty-free) technologies that enable the Web. As a platform, users interact with websites using their user agent (e.g., a Web Browser).

Websites contain a series of file formats, such as HTML, CSS, multimedia files, and scripts, that are transmitted from the server to the user's device, interpreted, and rendered by the browser so the user can use them.   
The web browser is a critical and widely used gateway for accessing the web. It is increasingly relied upon as the single most important application for work, forming the basis of browser-centric workflows.

However, the Web Platform presents significant security and privacy challenges. The browser, designed to request and execute instructions from arbitrary locations on the Internet, must surrender considerable control to web servers to render content correctly, as it runs code from untrusted sources.

#### High-level threats and threat sources

*   **Browser Extensions**: Browser extensions introduce numerous security vulnerabilities despite their utility. Malicious actors exploit them for sophisticated attacks like phishing, keylogging, spying, data theft, and session hijacking. They need to be installed and configured to have the permission to access a specific origin, which increases the complexity of the attack.
*   **Plugins/Add-ons**: Plugins add substantial attack surface and expose additional functionality and targets for attackers.
*   **Applications/Websites**: Websites can be compromised and malicious. Different attacks can be directed to compromise the user's session on the same website (e.g., stealing cookies or generating arbitrary requests), bypass the same origin and get information from different websites (i.e. Cross-Site Leaks - XS Leaks), or compromise the Browser itself (e.g., running arbitrary code into the browser processes to obtain control of the data inside the browser or to compromise the user device). Attackers exploit existing browser functionality.
*   **Tracking/Privacy Loss**: Browser fingerprinting is a method to identify a user, correlate browsing activity within and across sessions, and track users without their knowledge or consent. This raises privacy concerns, allowing parties to develop user profiles or histories across different sites, often without knowledge or consent. When correlated with identifying information, fingerprinting can identify otherwise pseudonymous users. Techniques like clearing cookies or using a VPN may not prevent this correlation. Data exposed by specifications, especially information about the underlying platform or state that persists, can contribute to fingerprinting.


#### External Dependencies

The web browser operates within an ecosystem that includes several external dependencies, systems, or entities it interacts with or relies upon:

*   **Operating System (OS)**: The browser runs with privileges granted by the OS, equivalent to user privileges. Browser security goals must manifest correctly between tabs, websites, and the OS. The OS provides a higher level of security against direct network connections than browsers.
*   **Network/Internet Infrastructure**: The browser's primary function involves requesting instructions and content from arbitrary locations on the Internet. This relies on the underlying network protocols like TCP/IP, which the browser presumably trusts. The decentralised nature of the internet means unencrypted data is likely being read by someone.
*   **Web Servers and their backend**: These are the primary source of content, instructions (scripts, HTML), and resources for the browser. The browser must surrender control and execute commands provided by the server. A web server can also reduce a browser's security. Websites interact with databases, off-site storage, and corporate data centres. Data flows between the browser context and these backend systems.
*   **Third Parties/Other Origins**: Modern web applications often include resources and scripts from other origins, which must also be executed. Websites may depend on third parties for ads, analytics, or authentication databases.
*   **Native Applications**: Other applications installed on the user's system can potentially access browser assets directly (e.g., via file system or memory inspection).
*   **External Components/Systems**: Any external dependency on components or systems can impact the browser's security.

#### Entry Points

Entry points are interfaces or mechanisms through which an adversary can interact with or supply data to the system. For a web browser, these include:

*   **The Browser Interface**: The user interface itself, where user input is provided.
*   **Web Content (HTML, CSS, Scripts, Resources)**: Malicious code, scripts, or content delivered from web servers. The browser runs code from untrusted sources when presented with scripts. An attacker can convince the browser to render malicious content.
*   **Network Interfaces**: Ports and protocols the browser uses to communicate over the network (e.g., Sockets, RPC, HTTP/HTTPS ports).
*   **User Input Fields**: Any area where users can input data, which can be manipulated maliciously (e.g., forms, URLs).
*   **Browser Extensions/Plugins APIs**: The browser exposes interfaces for extensions to interact with the browser and web content.
*   **Web APIs**: Both standard web APIs and potentially new, interesting APIs the browser exposes.
*   **Data Flow Paths**: Any path through which data flows through the application at every entry point. Critical feature areas include processing user-supplied data or interacting with the end user.
*   **Underlying Platform**: Features allowing an origin to send data to the underlying platform.

#### Assets

The assets that need to be protected when considering the web browser threat model are diverse and critical to user security and privacy:

*   **User Data**: General user data, sensitive data, and personally identifying information.
*   **User Data Privacy**: The ability of users to control and protect their private information and activities from surveillance, correlation, and identification. This includes avoiding identifying or correlating within and across browsing sessions without transparency or control.
*   **User Credentials**: Login information, usernames, and passwords.
*   **User Cookies and Session Information**: Data stored in cookies, session IDs, and session state. These can be used to recognize users across visits.
*   **User Device/Computer**: The user's machine and its resources, including the confidentiality and integrity of the user’s file system.
*   **Local Storage**: Data stored locally by the browser, including cookies, localStorage, indexedDB, cache, history, and passwords.
*   **User Browsing History**: Information about sites visited.
*   **User Environment Information**: Operating system configuration, browser configuration, hardware capabilities.
*   **User Activities and Interests**: Purchasing preferences, personal characteristics, and prior activities.
*   **Protected Resources**: Any resource the user can access that should not be accessed by an adversary.
*   **Private Network Resources**: These are different network resources that the browser can access without the user's explicit request.
*   **Credentials and Encryption Keys**: The browser manages Public and secret keys for encryption and signing.
*   **Confidential Information on Websites**: Data displayed or accessible on websites.

#### Security Features

Web browsers employ a variety of security features and protection mechanisms to defend against threats:

*   **Browser Security and Privacy Models**: Underlying models define how the browser handles security and privacy boundaries and interactions.
*   **Sandboxing**: Sandboxing is a key mechanism for confining components like the rendering engine, restricting their access to the operating system and user files. It acts as a barrier between OS privileges and subprocess privileges.
*   **Same Origin Policy (SOP)**: A fundamental security control that restricts how a document or script loaded from one origin can interact with a resource from another origin.
*   **Content Security Policy (CSP)**: A security standard to prevent XSS, clickjacking, and other code injection attacks by specifying which dynamic resources can load.
*   **HTTPS/SSL/TLS**: Protocols for secure, encrypted communication between the browser and web servers, ensuring data integrity and confidentiality in transit. HTTP Strict Transport Security (HSTS) is related.
*   **Input Validation and Output Encoding**: These are crucial practices for handling user-supplied data to prevent injection attacks like XSS.
*   **Security Headers**: HTTP headers instruct browsers on handling content securely.
*   **Reflected XSS Filtering**: Mechanisms to detect and block reflected XSS attacks.
*   **Anti-Phishing and Anti-Malware**: Features like Safe Browsing warn users about malicious websites.
*   **Mixed Content Handling**: How the browser treats secure (HTTPS) pages that load insecure (HTTP) resources.
*   **Renderer Isolation**: Architectures that place complex, error-prone components like the rendering engine in a separate, sandboxed process or protection domain from the browser kernel. The browser kernel handles sensitive OS, network, and storage interactions.
*   **Inter-process Communication (IPC)**: Secure channels for communication between isolated browser components (e.g., kernel and renderer).
*   **Download Manager**: Component handling file downloads, potentially including security checks.
*   **Data Minimization and Default Privacy Settings**: Design principles and configurations to reduce the exposure of potentially identifying information.
*   **Trusted UI**: Ensuring sensitive user interactions occur within browser interfaces that web content cannot easily spoof or manipulate.

#### Data Flow Diagram

The diagram illustrates the data flow and interactions between core browser components, external entities, and specific elements like storage, extensions, and device sensors.

```
+--------------+      (1) Request/Data   +-------------+
|    User      | <---------------------> |  Web Server |
+--------------+                         | (Internet)  |
       ^                                 +-------------+
       | (11) Rendered Content/UI                ^
       | (12) Input (Keyboard, Mouse)            | (2) HTML/Scripts/Resources
       |                                         | (3) Data (Form submission, etc.)
+------+-------+                         +-------+-----+
| Web Browser  | <---------------------> | Browser     | (Interacts with OS, Network, Storage)
|   (Overall)  |   (5) Kernel API/IPC    | Kernel      |
+------+-------+                         +-------+-----+
       ^                                         ^     ^
       | (4) Raw Content/Instructions            |     | (6) Reads/Writes (Files, Settings, etc.)
       |                                         |     | (7) Network Access
+------+-------+                         +-------+-----+
| Rendering    | <---------------------> | Sandboxed   | (Parses, Renders, Executes Scripts)
| Engine       |   (5) Kernel API/IPC    | Rendering   |
+--------------+                         | Engine      |
       ^                                 +-------+-----+
       | (8) Script/Rendering Output             ^     ^
       | (9) Data (DOM interaction, etc.)        |     | (10) Reads/Writes (Cookies, Cache, etc.)
+------+-------+                         +-------+-----+
| Browser      | <---------------------> |  Local      | (Cookies, localStorage, indexedDB, Cache, History, Passwords)
| Extensions   |   (8) Extension APIs    |  Storage    |
+--------------+                         +-------+-----+
       ^                                         ^
       | (13) Sensor Requests/Data               | (14) Reads/Writes (e.g., User Preferences, Offline Data)
+------+-------+                         +-------------+
| Device       | <---------------------> | Offsite/    | (Databases, Corporate Data Centers, Cloud Storage)
| Sensors      |   (13) Device APIs      | Database    |
| (Geolocation,|                         | Storage     |
| Camera, Mic) |                         +-------------+
+--------------+
```

**Description of Data Flow:**

1.  The **User** interacts with the **Web Browser**, providing input (12) and receiving rendered content (11).
2.  The **Web Browser** (specifically the Browser Kernel) sends requests (1) to the **Web Server** over the Internet (7).
3.  The **Web Server** responds with web content (HTML, scripts, resources) (2) and receives data from the browser (e.g., form submissions) (3).
4.  The **Browser Kernel** receives the raw web content and passes it 4 to the **Sandboxed Rendering Engine**.
5.  The **Rendering Engine** processes the content (parsing, layout, script execution). It interacts with the **Browser Kernel** using a controlled Kernel API or Inter-Process Communication (IPC) (5) to access privileged resources like the network (7) or file system (6).
6.  Both the **Browser Kernel** (for settings, history, passwords) and the **Rendering Engine** (for caching, cookies, local data) interact with **Local Storage** (10). The Kernel might handle more persistent or sensitive storage access (6).
7.  **Browser Extensions** interact with the **Web Browser** (potentially the Kernel or Rendering Engine via APIs) (8), receiving data about the rendered page, modifying content, or performing actions.
8.  Extensions or the core Browser features may request access to **Device Sensors** (13) (e.g., Geolocation, Camera, Microphone).
9.  **Device Sensors** return data to the requesting browser component or extension (13).
10.  The **Browser Kernel** or **Rendering Engine** (via the Kernel API) can interact with **Offsite/Database Storage** (14), reading or writing data as the web application requires.

Trust boundaries exist between many of these components, particularly between the sandboxed **Rendering Engine** and the **Browser Kernel**, the browser and Browser Extensions, and the browser and external entities like the **Web Server** and **Offsite Storage**. The **User** is the ultimate entity the browser aims to protect, receiving input (12) and potentially being tricked by malicious content/UI (11).

Other elements, such as the Credentials API and Notification API, can also provide access to users' assets.

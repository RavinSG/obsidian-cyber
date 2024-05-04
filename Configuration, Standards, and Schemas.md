
To harden systems in an enterprise environment, you'll need to manage the configuration of systems through your organization. In fact, **configuration management tools** are one of the *most powerful options security professionals and system administrators have* to ensure that the multitude of systems in their organisations have the right security settings and to help keep them safe.

Configuration management tools *often start with baseline configurations* for representative systems or operating system types throughout an organization. For example, you might choose to configure a baseline configuration for Windows 11 desktops, Windows 11 laptops, and macOS laptops. Those standards can *then be modified for specific groups, teams, divisions, or even individual* users as needed by placing them in groups that have those modified settings.

**Baseline configurations are an ideal starting place** to build from to help reduce complexity and make configuration management and system hardening possible across multiple machines—even thousands of them.

Once baselines are set, tools support *configuration enforcement*, a process that not only monitors for changes but makes changes to system configurations as needed to ensure that the configuration remains in its desired state.

Baselines can be considered through three phases of a baseline's life cycle:

- **Establishing** a baseline, which is most often done using an existing industry standard like the CIS benchmarks with modifications and adjustments made to fit the organisation's needs.
  
- **Deploying** the security baseline using central management tool or even manually depending on the scope, scale, and capabilities of the organisation.
  
- **Maintaining** the baseline by using central management tools and enforcement capabilities as well as making adjustments to the organisation's baseline if required by functional needs or other changes

### Patching and Patch Management

Ensuring that systems and software are up to date helps ensure endpoint security by removing known vulnerabilities. *Timely patching decreases how long exploits and flaws can be used* against systems, but patching also has its own set of risks. Patches may introduce new flaws, or the *patching process itself can sometimes cause issues*.

Patch management for operating systems can be done using tools like Microsoft's Endpoint Configuration Manager for Windows, but third-party tools provide support for patch management across a wide variety of software applications. *Tracking versions and patch status* using a systems management tool can be important for organisational security, particularly because third-party updates for the large numbers of applications and packages that organisations are likely to have deployed can be a daunting task.

>[!tip] Tip
>A common practice for many organisations is to *delay the installation of a patch* for a period of a *few days* from its release date. That allows the patch to be installed on systems around the world, hopefully providing insight into any issues the patch may create. In cases where a known exploit exists, organisations have to choose between the risk of patching and the risk of not patching and having the exploit used against them.

Managing software for *mobile devices remains a challenge*, but mobile device management tools also include the ability to validate software versions. These tools often have the ability to update applications and the device operating system in addition to controlling security settings.

Key features like **reporting**, the **ability to determine which systems** or applications get updates and when, the ability to **block an update**, and of course being able to **force updates** to be installed are all critical to enterprise patch management over the many different types of devices in our organisations today.
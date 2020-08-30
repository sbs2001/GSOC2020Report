# Google Summer of Code 2020 Report

## Organisation : AboutCode

## Project Title : Enhancement of VulnerableCode

**Quick primer on VulnerableCode**
VulnerableCode is Django project which aggregates data about software vulnerabilities from multiple sources and transforms it into an easy to use format.

**Things acheived in GSoC**

1.  Implemented a OVAL document parser. During this task I noticed that the element lookup method used in CIS's implementation was unusually slow. After some tweaks and algorithmic changes over 600% increase in performance was acheived. The improved implementation is now used by CIS. 
**Related PRs :**
    - [PR at CIS](https://github.com/CISecurity/OVALRepo/pull/1820) . 
    - https://github.com/nexB/vulnerablecode/pull/179
    - https://github.com/nexB/vulnerablecode/pull/182
    - https://github.com/nexB/vulnerablecode/pull/183
 
2.  Added over 8 data pipelines  AKA importers.  This included security advisories provided by NVD, GitHub, Gentoo, Debian OVAL, Ubuntu OVAL, Ubuntu USN,  SUSE Backports and RedHat RHSAs .
**Related PRs :**  
    - https://github.com/nexB/vulnerablecode/pull/203
    - https://github.com/nexB/vulnerablecode/pull/194
    - https://github.com/nexB/vulnerablecode/pull/193
    - https://github.com/nexB/vulnerablecode/pull/202
    - https://github.com/nexB/vulnerablecode/pull/200
    - https://github.com/nexB/vulnerablecode/pull/213
    - https://github.com/nexB/vulnerablecode/pull/204
    - https://github.com/nexB/vulnerablecode/pull/243
    - https://github.com/nexB/vulnerablecode/pull/194
  
3. Changes in data structures and schemas.
    1. Use JSONField instead of CharField to store qualifiers of a Package URL. This allows lookup of packages via Package URLs without requiring to normalize the qualifiers. 
    **Related PRs :**
       - https://github.com/nexB/vulnerablecode/pull/201
      
    2.  Combine ResolvedPackage and ImpactedPackage model into a single PackageRelatedVulnerablility model using a simple boolean field. This allows database level constraints to avoid conflicting relationships between  packages and vulnerabilities. Also added a basic mechanism to  store these conflicts and later be manually reviewed and resolved. Leverage some basic heuristics to  reduce database queries and allow bulk inserts while maintaining data integrity this reduced the time to perform data imports .
    **Related PRs :**
        -  https://github.com/nexB/vulnerablecode/pull/219
        - https://github.com/nexB/vulnerablecode/pull/239
     3. Pass reference urls and reference ids of a vulnerability as a mapping using `Reference` dataclass . This preserves the relationship between the urls and ids which was not possible earlier when urls and ids where passed discretely.
     **Related PRs :** 
        - https://github.com/nexB/vulnerablecode/pull/227
       
4. Designing and implementing the UI for community curation. Implemented views for creation, searching, deletion and modification of packages, vulnerabilities and the relationships between them. 
     **Related PRs :** 
        - https://github.com/nexB/vulnerablecode/pull/230
     
![zz_comp](https://user-images.githubusercontent.com/28975399/89056138-2c8a8300-d379-11ea-882e-f28f38789cdc.png)

5. Refactor the whole codebase to make use of asynchronous API calls to various package manager APIs. This gave a massive speed boost for data pipelines which were dependent on making API calls to resolve version ranges of packages.
     **Related PRs :** 
     - https://github.com/nexB/vulnerablecode/pull/236
     
 6.  API redesign : 
     1. Added API endpoint to search for packages using their package url.
     2. Added API endpoint to search for vulnerabilities using various vulnerability ids.
     3. Made the API more RESTful by adding hyperlinks which provide navigation experience comparable to the web interface.
     4. Added swagger API documentation which makes the API easy to use and understand 
   **Related PRs :**
          - https://github.com/nexB/vulnerablecode/pull/247
   
7. Miscellaneous improvements :
     1. Added mechanism for importers to check whether the data obtained is new, using either checking last date modified or ETags. This prevents duplicate work if the data is not changed  since the previous import.
     **Related PRs :**
           -  https://github.com/nexB/vulnerablecode/pull/191
           -  https://github.com/nexB/vulnerablecode/pull/237
  
     2. Switched to using a registry and a class yielding objects containing the seed data for importers. This prevented a lot of boiler plate code compared to previous approach of using migration scripts to provide initial seed data.
       **Related PRs :**
           - https://github.com/nexB/vulnerablecode/pull/221
     
      3. Added documentation to enable easy understanding of the project and it's features and enable easy onboarding of new contributors.
           **Related PRs :**
           - https://github.com/nexB/vulnerablecode/pull/246
           - https://github.com/nexB/vulnerablecode/pull/214
           - https://github.com/nexB/vulnerablecode/pull/240
    
  **Closing Thoughts**
    
As expected and warned by Haiko,  my time estimates on the proposal went wrong. it tooked too long to get the models right and refactor the codebase, but on the brighter side implementing the frontend was quicker than estimated so it all balanced out. There were many unexpected hiccups but documentation + Stack Overflow + Mentors  allowed me overcome these with ease.  Thanks to Philippe, Steven, Thomas, Haiko for their invaluable guidance and making this GSoC smooth. Summer well spent.

    

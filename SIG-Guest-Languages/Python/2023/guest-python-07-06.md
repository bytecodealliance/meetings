## Agenda
- Status updates
- How can we make retrieving and using WASI wheels as easy as possible for app developers?
    - E.g. which dependency managers to support and/or integrate with?
    - Can/should we do it without requiring `wasi-sdk` to be installed?
    - Can/should we make `componentize-py` download wheels itself as needed?
- Anything else?

## Notes
- two major installers: pip and conda
    - several workflow tools (like pipenv), but just defer to e.g. pip
- you can tell pip to cross-install: https://snarky.ca/testing-a-project-using-the-wasi-build-of-cpython-with-pytest/
    - not sure if workflow tool will necessarily pass through
    - maybe via env variable?  Need platform tag and target directory
    - or only support pip directly to start with?
- platform is really wasi-libc version until backward compatibility is practical
- alternative repos besides PyPI where we can publish?
    - Yes: concept of indexes
    - can configure via CLI or pypi.ini
    - PEP in progress to clarify how deps are found
- dependencies stored in a file which is workflow-tool-specific
    - py-project.toml oriented towards libraries
    - requirements.txt used for apps; pip-specific
    - working to standardize and related library-ify pip features and use py-project.toml for everything
- showcase project
    - can host dependency wheels on own server
- need to work with community on target name, compatibility (non)guarantees
- alternative to pip: conda and condaforge

## Action Items

- Joel: move meeting notes to https://github.com/bytecodealliance/meetings/tree/main/SIG-Guest-Languages
- Joel: add invitation to https://groups.google.com/g/ba-sig-guest-languages

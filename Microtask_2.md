# Microtask 2:
Identify new issues you encounter during installation.

For GrimoireLab, see Microtask 0.

---
For Augur, the most relevant issue occurred at the end of make install dev was this sqlalchemy error, occuring at the end of `make install-dev` I was able to work around this error by selecting "No" to overwriting the API key, and using the existing API key. Another thing I noticed is the bottom of this error is the first time my current API key was displayed to me. 
It also seems a bit contradictory that "No Augur API key found." but at the same time "We noticed you have an Augur API key already". 
```
CLI: [db.get_api_key] [ERROR] No Augur API key found.
then it says
We noticed you have an Augur API key already. Would you like to overwrite it with a new one?

sqlalchemy.exc.IntegrityError: (psycopg2.errors.UniqueViolation) duplicate key value violates unique constraint "augur_settings_pkey"
DETAIL:  Key (id)=(1) already exists.

[SQL:
        INSERT INTO augur_operations.augur_settings (setting,VALUE) VALUES ('augur_api_key','[new API key]') ON CONFLICT (setting)
        DO
        UPDATE
        SET VALUE='[new API key]';
        --UPDATE augur_operations.augur_settings SET VALUE = %(api_key)s WHERE setting='augur_api_key';
    ]
[parameters: {'api_key': '[old API key]'}]
(Background on this error at: http://sqlalche.me/e/13/gkpj)
make: *** [install-dev] Error 1
```

Other Augur Quickstart issues I ran into (see Microtask 0):
- During `make install-dev` npm not found ```Error: Installing...
scripts/install/frontend.sh: line 6: command: npm: not found
make: *** [Makefile:37: install-dev] Error 1```. At the time I did the tutorial, there weren't any extra installation steps in the Quickstart, however I was able to install node.js to get the installation to continue. 
- During `make install-dev` during installation of frontend dependencies, the following popped up `pkg_resources.DistributionNotFound: The 'gyp==0.1' distribution was not found and is required by the application`. This only occurred on Ubuntu 20.04 cloud compute instance using Quickstart. Fixed using https://github.com/nodejs/node-gyp/issues/2273 

---
Augur Getting Started issues I ran into (see Microtask 0): 
- During `make install-dev` I ran into this traceback (cut off for length)
```
setup.py:491: UserWarning: Unrecognized setuptools command ('dist_info --egg-base /private/var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/pip-install-yea0y51j/scipy/pip-wheel-metadata'), proceeding with generating Cython sources and expanding templates
  warnings.warn("Unrecognized setuptools command ('{}'), proceeding with "
Running from SciPy source directory.
/private/var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/pip-build-env-p_qfrju7/overlay/lib/python3.8/site-packages/numpy/distutils/system_info.py:1712: UserWarning:
    Lapack (<http://www.netlib.org/lapack/>) libraries not found.
    Directories to search for the libraries can be specified in the
    numpy/distutils/site.cfg file (section [lapack]) or by setting
    the LAPACK environment variable.
  if getattr(self, '_calc_info_{}'.format(lapack))():
/private/var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/pip-build-env-p_qfrju7/overlay/lib/python3.8/site-packages/numpy/distutils/system_info.py:1712: UserWarning:
    Lapack (<http://www.netlib.org/lapack/>) sources not found.
    Directories to search for the sources can be specified in the
    numpy/distutils/site.cfg file (section [lapack_src]) or by setting
    the LAPACK_SRC environment variable.
  if getattr(self, '_calc_info_{}'.format(lapack))():
Traceback (most recent call last):
  File "/Users/mabel/.virtualenvs/augur_env/lib/python3.8/site-packages/pip/_vendor/pep517/_in_process.py", line 207, in <module>
    main()
  File "/Users/mabel/.virtualenvs/augur_env/lib/python3.8/site-packages/pip/_vendor/pep517/_in_process.py", line 197, in main
    json_out['return_val'] = hook(**hook_input['kwargs'])
  File "/Users/mabel/.virtualenvs/augur_env/lib/python3.8/site-packages/pip/_vendor/pep517/_in_process.py", line 69, in prepare_metadata_for_build_wheel
    return hook(metadata_directory, config_settings)
  File "/private/var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/pip-build-env-p_qfrju7/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 166, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/private/var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/pip-build-env-p_qfrju7/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 258, in run_setup
    super(_BuildMetaLegacyBackend,
  File "/private/var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/pip-build-env-p_qfrju7/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 150, in run_setup
    exec(compile(code, __file__, 'exec'), locals())
  File "setup.py", line 631, in <module>
    setup_package()
  File "setup.py", line 627, in setup_package
    setup(**metadata)
  File "/private/var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/pip-build-env-p_qfrju7/overlay/lib/python3.8/site-packages/numpy/distutils/core.py", line 137, in setup
    config = configuration()
  File "setup.py", line 529, in configuration
    raise NotFoundError(msg)
numpy.distutils.system_info.NotFoundError: No BLAS/LAPACK libraries found. Note: Accelerate is no longer supported.
To build Scipy from sources, BLAS & LAPACK libraries need to be installed.
See site.cfg.example in the Scipy source directory and
<https://docs.scipy.org/doc/scipy/reference/building/index.html> for details.

ERROR: Command errored out with exit status 1: /Users/mabel/.virtualenvs/augur_env/bin/python3 /Users/mabel/.virtualenvs/augur_env/lib/python3.8/site-packages/pip/_vendor/pep517/_in_process.py prepare_metadata_for_build_wheel /var/folders/6x/5sk2b7h140l6phl07rtlg_1w0000gn/T/tmpizu9_bkt Check the logs for full command output.
make: *** [install-dev] Error 1
```
I was able to fix this by `export SYSTEM_VERSION_COMPAT=1` and trying the installation again. 

---
Augur Community Reports issues I ran into:
I noticed a lot of the SQL queries weren't running due to column ambiguity. For example in `new_contributor_CHAOSS.ipynb` there was this error:

<img width="1172" alt="Screen Shot 2022-04-17 at 6 48 07 PM" src="https://user-images.githubusercontent.com/70232089/163741989-d47d0fb5-f0c5-425c-afa9-7bfa6ff38b03.png">

I tried to debug these errors by guessing at what the intended columns were. But mostly, I relied on my own SQL queries. 

--- 


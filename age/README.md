# age
<!--
SPDX-FileCopyrightText: Copyright © contributors to CloudNativePG, established as CloudNativePG a Series of LF Projects, LLC.
SPDX-License-Identifier: Apache-2.0
-->

[Apache AGE](https://age.apache.org/) (A Graph Extension) is a PostgreSQL
extension that adds graph database functionality, enabling openCypher query
language support alongside standard SQL. It allows querying property graphs
stored directly in PostgreSQL without a separate graph database. For more
information, see the [official documentation](https://age.apache.org/age-manual/master/intro/overview.html).

## Usage

The `age` extension must be loaded via `shared_preload_libraries` before it
can be used.

### 1. Add the age extension image to your Cluster

Define the `age` extension under the `postgresql.extensions` section of your
`Cluster` resource. For example:

```yaml
apiVersion: postgresql.cnpg.io/v1
kind: Cluster
metadata:
  name: cluster-age
spec:
  imageName: ghcr.io/cloudnative-pg/postgresql:18-minimal-trixie
  instances: 1

  storage:
    size: 1Gi

  postgresql:
    shared_preload_libraries:
    - age
    extensions:
    - name: age
      image:
        # renovate: suite=trixie-pgdg depName=postgresql-18-age
        reference: ghcr.io/cloudnative-pg/age:1.7.0-18-trixie
```

### 2. Enable the extension in a database

You can install `age` in a specific database by creating or updating a
`Database` resource. For example, to enable it in the `app` database:

```yaml
apiVersion: postgresql.cnpg.io/v1
kind: Database
metadata:
  name: cluster-age-app
spec:
  name: app
  owner: app
  cluster:
    name: cluster-age
  extensions:
  - name: age
    # renovate: suite=trixie-pgdg depName=postgresql-18-age
    version: '1.7.0'
```

### 3. Verify installation

Once the database is ready, connect to it with `psql` and run:

```sql
\dx
```

You should see `age` listed among the installed extensions.

## Contributors

This extension is maintained by:

- Erling Kristiansen (@egkristi)

The maintainers are responsible for:

- Monitoring upstream releases and security vulnerabilities.
- Ensuring compatibility with supported PostgreSQL versions.
- Reviewing and merging contributions specific to this extension's container
  image and lifecycle.

---

## Licenses and Copyright

This container image contains software that may be licensed under various
open-source licenses.

All relevant license and copyright information for the `age` extension
and its dependencies are bundled within the image at:

```text
/licenses/
```

By using this image, you agree to comply with the terms of the licenses
contained therein.

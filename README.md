# id-to-uuid

Easily migrate from an auto incremented integer id to a [uuid](https://en.wikipedia.org/wiki/Universally_unique_identifier) in a project using [DoctrineMigrationsBundle](https://github.com/doctrine/DoctrineMigrationsBundle).
Autodetect your foreign keys and update them. **Works only on MySQL**.

## Installation

```
composer require habbim/id-to-uuid
```

## Usage

1. Update your `id` column from `integer` to `guid`:

```diff
# User.orm.xml
<entity name="AppBundle\Entity\User" table="user">
---    <id name="id" column="id" type="integer">
---        <generator strategy="AUTO" />
+++    <id name="id" column="id" type="uuid_binary_ordered_time">
+++        <generator strategy="CUSTOM"/>
+++        <custom-id-generator class="Ramsey\Uuid\Doctrine\UuidOrderedTimeGenerator"/>           
    </id>
 #...
</entity>
```

2. Config your symfony:

[Click here](https://github.com/ramsey/uuid-doctrine#innodb-optimised-binary-uuids)

3. Add a new migration:

```php
// app/DoctrineMigrations/VersionXYZ.php
<?php

namespace Application\Migrations;

use Doctrine\DBAL\Schema\Schema;
use CapCollectif\IdToUuid\IdToUuidMigration;

class VersionXYZ extends IdToUuidMigration
{
    public function postUp(Schema $schema)
    {
        $this->migrate('user');
    }
}
```

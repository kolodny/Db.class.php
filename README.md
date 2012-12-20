Usage
===

    $users = Db::query("SELECT * FROM user")->all();
    print_r($users);

outputs:

    array(
        [0] => array(
            id => 1,
            name => 'Admin',
        ),
        [1] => array(
            id => 2,
            name => 'Bob'
        )
    )

You can also get one row at a time:

    $result = Db::query("SELECT * FROM user");
    foreach ($result->one() as $row) {
        // more stuff here
    }

You can also use prepared statements:
    
    $sql = "SELECT * FROM user WHERE id = :id";
    $user = Db::prepare($sql)->bindParam('id', $_GET['id'])->execute()->one();

    $sql = "SELECT * FROM user WHERE id > :id";
    $user = Db::prepare($sql)->execute(  array('id' => $_GET['id'])  )->all();

It uses PDO's prepare methods in the background so you don't have to worry about sanitizing input.

----

Docs (briefly)
===


#### `Db::connected()`
> use this as a boolean to find out if a connection is currenly open (it acually returns the connection if it is in case you need it).

Returns `PDO` or `false`

------

####`Db::connect()` 
> Connects to the database. You need to set up `DB_HOST`, `DB_NAME`, `DB_USERNAME`, and `DB_PASSWORD` constants.

Returns `PDO`

---

####`Db::disconnect()`
> Sets the connection to null which effectively closes it.

----

####`Db::query($sql)`
> Performs a sql query

Returns `Db instance` (which from now on we will refer to as `$db`).

---

####`$db->one($pdo_fetch_flags = PDO::FETCH_ASSOC)`
> Return a single row of the last query using any fetch flags which `PDO` takes as a parameter.

---

####`$db->column($column_number = 0)`
> Returns column `$column_number` from the last query.

---

####`$db->all($flags = PDO::FETCH_ASSOC)`
>Return all the rows of the last query same deal with the flags as before.

----

####`$db->get($number_of_rows = null, $flags = PDO::FETCH_ASSOC)`
>Return `$number_of_rows` rows of the last query (or all of set to null), same deal with the flags as before


----

####`$db->lastInsertId()`
>Returns the last inserted id (autoincremented number)

----

####`Db::prepare($sql, $values = array())`
>Prepares a statement so you can use (resue and) execute later

Returns `$db`

----

####`$db->bindParam($key, $value)`
>Bind `$value` to `$key` (used on a prepared object)

Returns `$db`

----

####`$db->bindParams(array $values)`
>bind the the keys and values of `$values` (used on a prepared object)

Returns `$db`

----

####`$db->execute($values = array()`

>Execute the prepared statement with whatever values were already bound plus the values of `$values`

Returns `$db`

----

####`Db::columnNames($table)`
>Returns an array of `$tables` column names (ex `array('id', 'name')`)


----

####`Db::getEnums($tbl, $clmn)`, `Db::getEnums($tbl, $clmn)`, `Db::getEnumsOrSet($tbl, $clmn)`
> Returns an array of enum or set values in the `$clmn` column of table `$tbl` (ex `array('No', 'Yes', 'Maybe')`)


----

####`Db::parseList($list)`
> This is the actual function that does the processing on the Type column that show columns returns to get the enum or list values, `$list` is a string in the form of `"enum('No','Yes','Maybe')"`

----

####`Db::escapeTable($table_name)`
> Escapes $table_name so it's safe to use with user data (this just strips out anything not `[a-zA-Z0-9_$]`)

----

####`Db::escapeColumn($column_name)`
>Same as `Db::escapeTable`

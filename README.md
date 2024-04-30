# IDZ - Human readable ID

Provide human-readable ID with checksum. It avoids similar looking chars and
add a cheksum (crc8) at the end. When using random generation, it tries to avoid
having two same characters in a row.

Random generator is inspired/comes from [nanoid](https://github.com/ai/nanoid).

It is a _trait_ with only static methods so it can be used to compose a class 
and be used without any class instances.

Use this to generate ID like customer ID that would be exchange on the phone. ID
can be used in [Creditor Reference](https://en.wikipedia.org/wiki/Creditor_Reference)
for billing. Can be used for ticketing system or anything where the ID is exposed
to a human and might be manipulated by human.

## Usage

```php

class MyUserClass {
    use idz;
    function create ($firstname, $lastname) {
        /* 44 bits long id */
        $id = $this->get(9); /* for example 4XT3ACHCA */
        $this->pdo->prepare('INSERT INTO usertable (id, lastname, firstname) VALUES(:id, :lastname, :firstname)');
        /* db id is int 12265547877 */
        $this->pdo->bindValue(':id', $this->toint($id), PDO::PARAM_INT);
        /* ... */

        return $this->format($id); /* return 4XT-3AC-HCA */
    }
}

```

## Documentation

Source code is documented

## License

[MIT license](https://opensource.org/license/mit), see LICENSE file.

## Authors

  * Etienne Bagnoud <etienne@artnum.ch>
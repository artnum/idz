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

## Why use this kind of ID ?

This has been developped to solve a problem that people living in a single-language
country might not meet. But when your country is made of three languages, like
german, french and italian, ID made of numbers can be confusing.

For example, in french, number like 97 can be said "ninety seven" (90 + 7) or
"four twenty seventeen" (4 * 20 + 17). And when you are over the phone speaking 
to a german-speaking person trying to say "018-197-4" you could say "zero eighteen
one hundred four twenty seventeen four" or "zero eighteen one hundred nine seven
four" or goes "zero one eight one nine seven four", it might become quite messy :
german-speaking people could have learned either "ninety seven" or "four twenty 
seventeen" version of 97 (depending of the school).

Going the version by spelling out each number individually can be a pain when you
have number like "0010-1777-3770-0000", repeating the same number several time
goes like "zero zero one zero one seven seven seven, yes three times seven, three
seven zero zero zero zero zero, yes that five times zero".

With that kind of ID, you have 28 bits integer encoded in string like 
"DCA-3U9-2CZ", you have 7 ID charactes, 2 checksum characters which give us 
a number of possibilites, as chars can be repeated in row, of 182'250'000, 16 *
(15 ^ 6). Which is large enough for many application.

For example, Swiss social security number is in a form of _756.xxxx.xxxx.xcc_, 9 
random int (x), a checksum (c) and 756 as country code ID, it is 1'000'000'000 
posibilities. You can get more than twice the number of ID with the shorter
_CH-xxxx-xxxx-cc_ : 16 * (15 ^ 7) = 2'733'750'000 and still be used in billing
with Creditor Reference. Unfortunately, the Swiss social security number was
introduced in 2019 so it's unlikely it will be soon replaced by this solution.

## The alphabet

The choosen alphabet is

  * **2** => 0x0, 0
  * **3** => 0x1, 1
  * **4** => 0x2, 2
  * **5** => 0x3, 3
  * **9** => 0x4, 4
  * **A** => 0x5, 5
  * **C** => 0x6, 6
  * **D** => 0x7, 7
  * **H** => 0x8, 8
  * **L** => 0x9, 9
  * **R** => 0xA, 10
  * **T** => 0xB, 11
  * **U** => 0xC, 12
  * **X** => 0xD, 13
  * **Y** => 0xE, 14
  * **Z** => 0xF, 15

## Usage

```php

class MyUserClass {
    use idz;
    function create ($firstname, $lastname) {
        /* 44 bits long id */
        $id = $this->generate(9); /* for example 4XT3ACHCA */
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

## Todo

  * Unit test.
  * Certainly bug fix.

## License

[MIT license](https://opensource.org/license/mit), see LICENSE file.

## Authors

  * Etienne Bagnoud <etienne@artnum.ch>
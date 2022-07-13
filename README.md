# Cadence_BC---quest-submission

# Chapter 1 Day 1

## Q1
I understand a blockchain as a database made a series of blocks which contain information. These blocks are created and stored so that they are available to the public. There is a set of rules to define how the block creation takes place, in case this rules change we can assume that the blockchain is at least an evolution of the first one, most times a different one.

## Q2
Smart contracts are automated programs stored in a blockchain that run if certain conditions are met. An example of that would be a decentralized exchange that when sending one token to a liquidity pool, it will swap it for another token and send it back.

## Q3
A transaction is some data which is processed by the blockchain and changes some of the information stored on it - like the balance of a wallet after paying for something. It costs gas tp execute.
A script is a program written to read the information available on the blockchain - like reading the balance of a certain wallet on a certain date. it is free to execute.

# Chapter 1 Day 2

## Q1
     Safety and Security
     Clarity
     Approachability
     Developer Experience
     Resource Oriented Programming
     
## Q2 
They are a very good approach to making smart contracts development better - easier to write, review, debug and maintain than other crypto programming languages and accesible to a wider group of people (not only developers). By doing this, it settles strong foundations for mainstream adoption.


# Chapter 2 Day 1

## Contract

![imagen](https://user-images.githubusercontent.com/107128136/173880586-bc5025cc-47b9-4dac-a32c-1418312ed16a.png)


## Script

```cadence 
import JakobTucker from 0x03

pub fun main(): String{
  return JakobTucker.is
}
```

![imagen](https://user-images.githubusercontent.com/107128136/173701460-bc3919cb-e163-4fd0-af04-99730d6555c7.png)

# Chapter 2 Day 2

## Q1
Since a script only reads data from the blockchain, calling the `changeGreeting` function which aims to modify the data would probably give back an error

## Q2
It allows the transaction to access data in your account by signing in the transaction.

## Q3 
The `prepare` phase aims at accessing the account data while the `execute` phase calls functions to modify the data on the blockchain. This separates the logic in two different phases but the execution coudl theoretically also work under the `prepare` phase. So takeaway is:
 - `prepare` allows you to access data in a account
 - `execute` is a different logical phase where data is modify by calling functions

## Q4

### Contract 

```cadence
pub contract JakobTucker {
  
  pub var is: String

  pub var myNumber: Int

  pub fun changeis(nowis: String) {
    self.is = nowis
  }

  pub fun updateMyNumber(newNumber: Int){
    self.myNumber = newNumber
  }

  init(){ 
    self.is = "the best" 
    self.myNumber = 0
  }
  
}
```
### Script

```cadence
import JakobTucker from 0x03

pub fun main(): Int{
  return JakobTucker.myNumber
}
```
![imagen](https://user-images.githubusercontent.com/107128136/173939896-9dfcb9ac-b384-4e99-bfea-2576ac2277f7.png)

### Transaction

```cadence
import JakobTucker from 0x03

transaction (myNewNumber: Int){

  prepare(signer: AuthAccount) {}

  execute {
    JakobTucker.updateMyNumber(newNumber: myNewNumber)
  }
}
```
![imagen](https://user-images.githubusercontent.com/107128136/173941710-14155659-6e54-4801-9a52-b3f6aacdce82.png)

# Chapter 2 Day 3

## Q1

```cadence
pub fun main() {

    let favorite: [String] = ["Bob my neighbour", "Greg my baker", "Momma"]

    log(favorite)
}
```
![imagen](https://user-images.githubusercontent.com/107128136/173955469-303fdd55-2a34-4c58-b573-e3c059e55dc3.png)

## Q2

```cadence
pub fun main() {
    var socialMedia:{String: UInt64} = {"Facebook": 0, "Instagram": 0, "Twitter": 1, "Reddit": 0, "LinkedIn": 0}
    
    log (socialMedia)
}
```

![imagen](https://user-images.githubusercontent.com/107128136/173956058-7d50260b-71e9-43fc-aa7f-c18b23c5f6c7.png)


## Q3

The force unwrap operator is used as a way of returning an error if the type of a variable is nil. 

The following script will return "Caramba" as the value tied to the key 0x04:
```cadence
pub fun main(): String {

  let thing: {Address: String} = {0x01: "One", 0x02: "Two", 0x03: "Three", 0x04: "Caramba"}
  return thing[0x04]!
}
```

On the contrary, the script below will compile but return an error because the function returns a nil (it couldn't find the key 0x05:
```
cadence
pub fun main(): String {

  let thing: {Address: String} = {0x01: "One", 0x02: "Two", 0x03: "Three", 0x04: "Caramba"}
  return thing[0x05]
}
```
![imagen](https://user-images.githubusercontent.com/107128136/173958516-b23d1c85-a0ba-446c-ae42-50706a371c3c.png)

The alterntive would be to allow the optionals and in case of a missing key, it will return an empty value:
```cadence
pub fun main(): String? {

  let thing: {Address: String} = {0x01: "One", 0x02: "Two", 0x03: "Three", 0x04: "Caramba"}
  return thing[0x05]
}
```
![imagen](https://user-images.githubusercontent.com/107128136/173958806-85b7a7f4-3c66-4949-a535-128cc9211891.png)

## Q4 
- the error message means that a type String was expected as the result of the function return, but instead got an optional
- I guess with dictionaries the syntax makes the use of optionals standard and if not handled correctly, it prevents the script to run
- the error can be solved either by forcing the unwrapping on return `return thing[0x03]!` or by declaring the main as an optional `pub fun main(): String?` 

![imagen](https://user-images.githubusercontent.com/107128136/173957725-3b7f3ddf-2547-40cd-a901-00d96490c828.png)


# Chapter 2 Day 4

## Contract

```cadence
//Struct Contrac

pub contract library {

    pub var booklocation: {String: Bookfile}

    pub struct Bookfile {
        pub let title: String
        pub let author: String
        pub let year: Int
        pub let location: String

        // Pass 4 arguments when creating the Struct

        init (_title: String, _author: String, _year: Int, _location: String){
            self.title = _title
            self.author = _author
            self.year = _year
            self.location = _location
        }
    } 

    pub fun addBook (title: String, author: String, year: Int, location: String){
        let newBook = Bookfile(_title: title, _author: author, _year: year, _location: location)
        self.booklocation[location] = newBook
    }

    init() {
        self.booklocation = {}
    }
}
```

## Transaction

```cadence
import library from 0x01

transaction (title: String, author: String, year: Int, location: String) {
    prepare (signer: AuthAccount){}

    execute {
        library.addBook(title: title, author: author, year: year, location: location)
        log("Everything is fine")
    }
}
```
![imagen](https://user-images.githubusercontent.com/107128136/174136753-df112d08-132e-47e0-bd69-424ebc0d4259.png)


## Script

```cadence
import library from 0x01

pub fun main(location: String):library.Bookfile {
  return library.booklocation[location]!
}
```

![imagen](https://user-images.githubusercontent.com/107128136/174136853-9c6bb293-05e3-447a-a480-fd8b39e61823.png)

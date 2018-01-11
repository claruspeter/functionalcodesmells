---
title: Null Propagation
description: the nothing that devours all
---

Many functional languages avoid the use of null values as a core value that __meaningless state should be unrepresentable__.  
Instead of using a null value as a default subsitute for any missing value, there is usually an extra explicit state that a value may have that can indicate missing or invalid data. An obvious example is the use of the `option` type where a value may either be `None` or `Some x`.

However, interaction with systems that _do_ allow nulls can cause the data flowing from these systems to contain nulls.  For example, working with a REST resource that has optional values, looking up a resource from a query that is overly filtered, or interacting with libraries written in other languages such as C# or Java.

When the null data pervades the solution because the incoming data stream contains nulls, then this is the smell __Null Propagation__.

## Example
Data with an optional value represented as a null.

This data contains a table of Members of the NZ Parliament, but not all MPs have a value for the _Electorate_ field, as some are List MPs (that's the [MMP](https://en.wikipedia.org/wiki/Mixed-member_proportional_representation) system for you).  Just passing that data along in this structure passes on the inherent nulls too. Unless the receiver knows intimate details about which fields may contain nulls, then she has to check ALL fields or risk an exception.

The solution is to deal with the possible null values _once_ and _immediately_, by converting the null-riddled data into a model that is explicit about fields that are optional.

```fsharp





    let memberData = loadDataFrom("https://catalogue.data.govt.nz/api/3/action/datastore_search?resource_id=89069a40-abcf-4190-9665-3513ff004dd8")












    // consumer beware - may contain nulls!
    let addresses = 
        memberData.result.records 
        |> Seq.map AddrModule.getElectorateAddress 
    // and we have to deal with the null too 
    let printedAddresses = 
        addresses
        |> Seq.map 
            (fun addr ->
                if addr = null then
                    "No Address" 
                else 
                    addr
            )
    ...
```
```fsharp
    type LocatedMember = 
        {
            name: string; 
            electorate: Electorate option
        }
    let memberData = loadDataFrom("https://catalogue.data.govt.nz/api/3/action/datastore_search?resource_id=89069a40-abcf-4190-9665-3513ff004dd8")
    let modelledData = 
        memberData.result.records 
        |> Seq.map (fun m -> 
            {
                name = m.Member_of_Parliament
                // deal with the nulls ONCE
                electorate =
                  if m.Electorate = null then
                    None 
                  else
                    Some Electorate(m.Electorate)
            })
    // Calling another module with explicit model.
    let addresses = 
        modelledData
        |> Seq.map AddrModule.getElectorateAddress
    // Addresses are all printable due to the
    //  ability to create a rich model.
    ...








```
It can possibly make for some ugly code in order to create the model, but the important point is that it pushes any ugliness to the borders and protects the rest of the system's purity.

## Refactorings
- Internal Data Modelling at the interface with _nullable_ system.

## References
- [Public NZ Govt Data](https://catalogue.data.govt.nz/dataset/members-of-parliament/resource/89069a40-abcf-4190-9665-3513ff004dd8)
- [David Raab on Evil Nulls](https://sidburn.github.io/blog/2016/03/20/null-is-evil)
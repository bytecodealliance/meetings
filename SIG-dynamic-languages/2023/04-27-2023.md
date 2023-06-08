## Apr 27, 2023

|          |      | 
| -------- | -------- |
| Attending  | Kyle Brown, Calvin Prewitt, Bailey Hayes, Danny Macovei, Joel Dice, Saúl Cabrera, Kevin Smith, Till Schneidereit
| Note Taker |

* [Kyle] - Updates
* [Kevin] - PyCon
* [Bailey] GC in Components
    * Bailey: GC types can be added to Component Model additively. Kowasm is the furthest along and can be run in javascript which has GC and we can use JS to test/validate the GC support which is neat. It looks pretty straightforward and I've synced with luke.
    * Till: Are you looking at implmeneting lifting/lowering with GC types on the internal end and WIT on the external end without leaking GC out?
    * Bailey: Yep, this mostly involves work on the Canonical ABI and then Wasm tools. I just want to prove first that we can run a componentized GC'd component then we can validate this.
    * Till: I assume Luke mentioned, but if you do ..., you may want to talk with [Ivan Mikushin](mailto:imikushin@vmware.com) at VMWare.
    * Bailey: I'm trying to get something available as quick as possible. Adobe is a customer and they have a scary amount of Java.
    * Till: I don't think missing the boat for PReview 2 means you have to wait for Preview 3. Since it's additive, once we support it in tools it's ready
    * Bailey: ... brought up that there are 3 types of languages: dynamic languages (like we're working on), the go/kotlin/swift languages are maybe different or not?
* [Kyle] - Side-bar on whether to include Go/Kotlin/Swift
    * Joel: These languages have similar problems
    * Till: There are two ways people talk about dynamic languages: dynamic types and dynamically run (e.g. JIT/interpret). As far as what's in scope for us, I'm not sure the dynamic aspect is that interesting/important. It's more about the managed heavy runtime part where you want to move it outside and share it that is more interesting.
* [Kyle] - [Subgroup proposal](https://github.com/bytecodealliance/SIG-Dynamic-Languages/pull/1)

Action Items:
* Kyle: Announce vote on subgroup proposal for next call
* Till: Propose ammendment to charter to become SIG-Managed-Languages
* Till: Set up Google Group for SI


## Optimizer

This is a summary of the current status of the optimizer and some places where I'd like to move forward.

* Function optimizer:
	* Minor optimizations:
		* Peepholes
		* SROA
		* Memory pass (Load elim, store elim, SSA construction for locals and memory splitting for non-escaping stack allocations)
	* Loop optimizer:
		* Loop finding:
			* We generate loop nests by sorting the SCCs, within one we might find multiple natural loops.
		* Loop rotation:
			* Canonicalize loops such that they are unconditionally entered.
			* Clone the first block of the first iteration, this involves duplicate the loop "gate" and then moving the original loop "gate" to the bottom of the loop.
		* ==TODO: Loop idioms:==
			* Detect common loop patterns like memset, memcpy (probably others too).
		* ==TODO Loop predicate hoisting:==
			* If a check within the loop is always
		* ==TODO: Loop Unrolling:==
			* We can clone the body of a loop to improve the ILP, which can increase the chances of being vectorized.
		* ==TODO Loop splitting:==
			* When choosing to vectorize a loop, we should divide the "misaligned" (either by address or trip count) into a scalar pre-loop.
		* SLP
		* Peepholes (again)
		* IV strength reduction
	* Optimistic solver:
		* SCCP
		* GCF
		* Algebraic identities

## Machine Codegen

Once we've completed the optimizer phase, we move onto machine codegen.

Phases:
* Instruction selection
* GCM
* Local scheduling
* Register allocator
	* Rogers RA
	* Briggs RA
* BB placement
* Machine code emit
	* Peepholes
	* Bundling
	* Emit bytes
* Emit stubs

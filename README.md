# Circuit Anatomy

A Circuit Frontend is a tool allowing you to define circuit constraints.
A circuit Frontend with compile down to constraint systems by an arithmetization of circuit constraints.
Circuit constraint systems are then fed to a proving backend to generate a proof.
There are four well known constraint systems:

- Rank-1 Constraint Systems (R1CS)
- Plonkish
- Custom Constraint Systems (CCS)
- Algebraic Intermediate Representation (AIR)

CCS and Plonkish arithmetization schemes have the advantage of being able to support lookup arguments which we are interested in. We are not interest in ploink as 
We are particulartly interested in CCS as they support lookup arguments which we are interested in as they can give us performance benefits.

## Frontends

### [noir](https://github.com/noir-lang/noir)
Noir is a domain specific language for writing circuits. Noir is designed to be easy to use and understand. Noir supports Plonkish constraint systems out of the box. Noir does not support CCS out of the box.

### [Halo2](https://github.com/privacy-scaling-explorations/halo2)
Halo2 has a lot of versatility. Halo2 itself has a frontend that is what the user interacts with to write circuits. Halo2 also has a backend where you can implement your own constraint system you want to use. Halo2 supports Plonkish constraint systems our of the box with lookup arguments. Halo2 doesn't support CCS out of the box. However, there does seem to be a path forward to support CCS with a custom halo2 backend. There be dragons.

### [Circom](https://github.com/iden3/circom)
Circom supports R1CS. Circom does not support high degree gates or lookup arguments.

### [Arkworks](https://github.com/arkworks-rs/r1cs-std)
The arkworks frontend compiles circuits to R1CS.

### [Noname](https://github.com/zksecurity/noname)
Noname is a frontend for writing circuits that compiles to R1CS or Plonk. It is designed to work with Snarkjs and Kimchi. 

### [Bellpepper](https://github.com/argumentcomputer/bellpepper)
Bellpepper is a rust crate used for writing arithmetic circuits and generating witnesses for those circuits. 
Bellpepper supports R1CS out of the box and have a [constraint system trait](https://github.com/argumentcomputer/bellpepper/blob/7275595cc7121b0adc8ce5e88ce1281aa6aff601/crates/bellpepper-core/src/constraint_system.rs#L61) allowing for users to implement their own constraint systems. Bellpepper does not support CCS out of the box.

### [Plonky3](https://github.com/Plonky3/Plonky3)
Plonky3 compiles to AIR constraint systems.

## Incrementally Verifiable Computation (IVC) Backends
Most IVC Backends do not support Plonkish or CCS arithmetization schemes. The goal we are trying to achieve is to find / build an IVC stack that will support lookup arguments. 

### [Arecibo](https://github.com/argumentcomputer/arecibo)
Arecibo is a folding backend based on NOVA. It is referred to more generally as an incrementally verifiable computation (IVC) backend. Arecibo is shipped with a bellpeper implementation of R1Cs via the constraint system trait.

Arecibo, does also have a step circuit trait which can be implemented to seemingly support arbitrary constraint systems. 

### [Sonobe](https://github.com/privacy-scaling-explorations/sonobe)
Sonobe claims support for arkworks, circom and noname frontends. Sonobe supports Nova, CycleFold and HyperNova folding schemes. Interestingly, Sonobe seems to have one of the only [CCS implementations](https://github.com/privacy-scaling-explorations/sonobe/blob/main/folding-schemes/src/arith/ccs.rs). Sonobe implements proto-galaxy. 


### ProtoStar
Proto Star is a folding scheme for Plonkish constraint systems supporting lookup arguments. There is support for it in the nior pipline. And option for us would be to use this r1cs to ACIR pipeline to generate an ACIR artifact for use in a plonkish IVC backend. 

- [r1cs to ACIR](https://github.com/TomAFrench/circom-to-acir/blob/master/src/circuit.rs)
- [ACIR](https://github.com/noir-lang/noir/blob/master/acvm-repo/acir/README.md)

### [Nexus](https://github.com/nexus-xyz/nexus-zkvm)
Nexus is a ZKVM that has an IVC backend. Nexus has one of the two implementations of [CCS](https://github.com/nexus-xyz/nexus-zkvm/blob/main/nova/src/ccs/mod.rs), the other of which is in sonobe.

## Notes and Questions

Protogalaxy / ProtoStar vs HyperNova (Plonkish folding vs ccs folding)

ACIR (from noir) is a constraint system agnostic artifact.

PLAF => plonkish artifact for halo2frontend.

PIL => AIR artifact, part hermes.
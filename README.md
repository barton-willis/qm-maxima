Package qm
**********

1 Package qm
************

1.1 Introduction to package qm
==============================

The ‘qm’ package provides functions and standard definitions to solve
quantum mechanics problems in a finite dimensional Hilbert space.  For
example, one can calculate the outcome of Stern-Gerlach experiments
using the built-in definition of the Sx, Sy, and Sz operators for
arbitrary spin, e.g.  ‘s={1/2, 1, 3/2, ...}’.  For spin-1/2 the standard
basis states in the <x>, <y>, and <z>-basis are available as ‘{xp,xm}’,
‘{yp,ym}’, and ‘{zp,zm}’.  One can create general ket vectors with
arbitrary but finite dimension and perform standard computations such as
expectation value, variance, etc.  The angular momentum <|j,m>>
representation of kets is also available.  It is also possible to create
tensor product states for multiparticle systems and to perform
calculations on those systems.

   The ‘qm’ package was written by Eric Majzoub, University of Missouri.
(Email: majzoube-at-umsystem.edu) The package is loaded with:
‘load(qm);’

1.2 Functions and Variables for qm
==================================

 -- Variable: hbar
     Planck's constant divided by ‘2*%pi’.  ‘hbar’ is not given a
     floating point value, but is declared to be a real number greater
     than zero.

 -- Function: ket ([c_{1},c_{2},...])
     ‘ket’ creates a _column_ vector of arbitrary finite dimension.  The
     entries ‘c_{i}’ can be any Maxima expression.  The user must
     ‘declare’ any relevant constants to be complex.  For a matrix
     representation the elements must be entered as a list in ‘[...]’
     square brackets.  If no list is entered the ket is represented as a
     general ket, ‘ket(a)’ will return ‘|a>’.

     (%i4) kill(a);
     (%o4)                                done
     (%i5) ket(a);
     (%o5)                                 |a>
     (%i6) declare([c1,c2],complex);
     (%o6)                                done
     (%i7) ket([c1,c2]);
                                         [ c1 ]
     (%o7)                               [    ]
                                         [ c2 ]
     (%i8) facts();
     (%o8) [kind(hbar, real), hbar > 0, kind(c1, complex), kind(c2, complex)]

 -- Function: bra ([c_{1},c_{2},...])
     ‘bra’ creates a _row_ vector of arbitrary finite dimension.  The
     entries ‘c_{i}’ can be any Maxima expression.  The user must
     ‘declare’ any relevant constants to be complex.  For a matrix
     representation the elements must be entered as a list in ‘[...]’
     square bracbras.  If no list is entered the bra is represented as a
     general bra, ‘bra(a)’ will return ‘<a|’.

     (%i4) kill(c1,c2);
     (%o4)                                done
     (%i5) bra(c1,c2);
     (%o5)                              <c1, c2|
     (%i6) bra([c1,c2]);
     (%o6)                             [ c1  c2 ]
     (%i7) facts();
     (%o7)                    [kind(hbar, real), hbar > 0]

 -- Function: ketp (_vector_)
     ‘ketp’ is a predicate function that checks if its input is a ket,
     in which case it returns ‘true’, else it returns ‘false’.  ‘ketp’
     only returns ‘true’ for the matrix representation of a ket.

     (%i4) kill(a,b,k);
     (%o4)                                done
     (%i5) k:ket(a,b);
     (%o5)                               |a, b>
     (%i6) ketp(k);
     (%o6)                                false
     (%i7) k:ket([a,b]);
                                          [ a ]
     (%o7)                                [   ]
                                          [ b ]
     (%i8) ketp(k);
     (%o8)                                true

 -- Function: brap (_vector_)
     ‘brap’ is a predicate function that checks if its input is a bra,
     in which case it returns ‘true’, else it returns ‘false’.  ‘brap’
     only returns ‘true’ for the matrix representation of a bra.

     (%i4) b:bra([a,b]);
     (%o4)                              [ a  b ]
     (%i5) brap(b);
     (%o5)                                true

 -- Function: dagger (_vector_)
     ‘dagger’ is the quantum mechanical _dagger_ function and returns
     the ‘conjugate’ ‘transpose’ of its input.

     (%i4) dagger(bra([%i,2]));
                                        [ - %i ]
     (%o4)                              [      ]
                                        [  2   ]

 -- Function: braket (psi,phi)
     Given two kets ‘psi’ and ‘phi’, ‘braket’ returns the quantum
     mechanical bracket ‘<psi|phi>’.  The vector ‘psi’ may be input as
     either a ‘ket’ or ‘bra’.  If it is a ‘ket’ it will be turned into a
     ‘bra’ with the ‘dagger’ function before the inner product is taken.
     The vector ‘phi’ must always be a ‘ket’.

     (%i4) declare([a,b,c],complex);
     (%o4)                                done
     (%i5) braket(ket([a,b,c]),ket([a,b,c]));
     (%o5)          c conjugate(c) + b conjugate(b) + a conjugate(a)

 -- Function: norm (psi)
     Given a ‘ket’ or ‘bra’ ‘psi’, ‘norm’ returns the square root of the
     quantum mechanical bracket ‘<psi|psi>’.  The vector ‘psi’ must
     always be a ‘ket’, otherwise the function will return ‘false’.

     (%i4) declare([a,b,c],complex);
     (%o4)                                done
     (%i5) norm(ket([a,b,c]));
     (%o5)       sqrt(c conjugate(c) + b conjugate(b) + a conjugate(a))
     (%i6) norm(ket(a,b,c));
     (%o6)                 sqrt(braket(|a, b, c>, |a, b, c>))

 -- Function: magsqr (c)
     ‘magsqr’ returns ‘conjugate(c)*c’, the magnitude squared of a
     complex number.

     (%i4) declare([a,b,c,d],complex);
     (%o4)                                done
     (%i5) A:braket(ket([a,b]),ket([c,d]));
     (%o5)                   conjugate(b) d + conjugate(a) c
     (%i6) P:magsqr(A);
     (%o6) (conjugate(b) d + conjugate(a) c) (b conjugate(d) + a conjugate(c))

1.2.1 Spin-1/2 state kets and associated operators
--------------------------------------------------

Spin-1/2 particles are characterized by a simple 2-dimensional Hilbert
space of states.  It is spanned by two vectors.  In the <z>-basis these
vectors are ‘{zp,zm}’, and the basis kets in the <z>-basis are ‘{xp,xm}’
and ‘{yp,ym}’ respectively.

 -- Function: zp
     Return the <|z+>> ket in the <z>-basis.

 -- Function: zm
     Return the <|z->> ket in the <z>-basis.

 -- Function: xp
     Return the <|x+>> ket in the <z>-basis.

 -- Function: xm
     Return the <|x->> ket in the <z>-basis.

 -- Function: yp
     Return the <|y+>> ket in the <z>-basis.

 -- Function: ym
     Return the <|y->> ket in the <z>-basis.

     (%i4) zp;
                                          [ 1 ]
     (%o4)                                [   ]
                                          [ 0 ]
     (%i5) zm;
                                          [ 0 ]
     (%o5)                                [   ]
                                          [ 1 ]
     (%i4) yp;
                                       [    1    ]
                                       [ ------- ]
                                       [ sqrt(2) ]
     (%o4)                             [         ]
                                       [   %i    ]
                                       [ ------- ]
                                       [ sqrt(2) ]
     (%i5) ym;
                                      [     1     ]
                                      [  -------  ]
                                      [  sqrt(2)  ]
     (%o5)                            [           ]
                                      [     %i    ]
                                      [ - ------- ]
                                      [   sqrt(2) ]
     (%i4) braket(xp,zp);
                                            1
     (%o4)                               -------
                                         sqrt(2)

   Switching bases is done in the following example where a <z>-basis
ket is constructed and the <x>-basis ket is computed.

     (%i4) declare([a,b],complex);
     (%o4)                                done
     (%i5) psi:ket([a,b]);
                                          [ a ]
     (%o5)                                [   ]
                                          [ b ]
     (%i6) psi_x:'xp*braket(xp,psi)+'xm*braket(xm,psi);
                         b         a              a         b
     (%o6)           (------- + -------) xp + (------- - -------) xm
                      sqrt(2)   sqrt(2)        sqrt(2)   sqrt(2)

1.2.2 Pauli matrices and Sz, Sx, Sy operators
---------------------------------------------

 -- Function: sigmax
     Returns the Pauli <x> matrix.

 -- Function: sigmay
     Returns the Pauli <y> matrix.

 -- Function: sigmaz
     Returns the Pauli <z> matrix.

 -- Function: Sx
     Returns the spin-1/2 <Sx> matrix.

 -- Function: Sy
     Returns the spin-1/2 <Sy> matrix.

 -- Function: Sz
     Returns the spin-1/2 <Sz> matrix.

     (%i4) sigmay;
                                      [ 0   - %i ]
     (%o4)                            [          ]
                                      [ %i   0   ]
     (%i5) Sy;
                                 [            %i hbar ]
                                 [    0     - ------- ]
                                 [               2    ]
     (%o5)                       [                    ]
                                 [ %i hbar            ]
                                 [ -------      0     ]
                                 [    2               ]

 -- Function: commutator (X,Y)
     Given two operators ‘X’ and ‘Y’, return the commutator ‘X . Y - Y .
     X’.

     (%i4) commutator(Sx,Sy);
                                [        2             ]
                                [ %i hbar              ]
                                [ --------      0      ]
                                [    2                 ]
     (%o4)                      [                      ]
                                [                    2 ]
                                [             %i hbar  ]
                                [    0      - -------- ]
                                [                2     ]

1.2.3 SX, SY, SZ operators for any spin
---------------------------------------

 -- Function: SX (s)
     ‘SX(s)’ for spin ‘s’ returns the matrix representation of the spin
     operator ‘Sx’.  Shortcuts for spin-1/2 are ‘Sx,Sy,Sz’, and for
     spin-1 are ‘Sx1,Sy1,Sz1’.

 -- Function: SY (s)
     ‘SY(s)’ for spin ‘s’ returns the matrix representation of the spin
     operator ‘Sy’.  Shortcuts for spin-1/2 are ‘Sx,Sy,Sz’, and for
     spin-1 are ‘Sx1,Sy1,Sz1’.

 -- Function: SZ (s)
     ‘SZ(s)’ for spin ‘s’ returns the matrix representation of the spin
     operator ‘Sz’.  Shortcuts for spin-1/2 are ‘Sx,Sy,Sz’, and for
     spin-1 are ‘Sx1,Sy1,Sz1’.

   Example:

     (%i4) SY(1/2);
                                 [            %i hbar ]
                                 [    0     - ------- ]
                                 [               2    ]
     (%o4)                       [                    ]
                                 [ %i hbar            ]
                                 [ -------      0     ]
                                 [    2               ]
     (%i5) SX(1);
                              [           hbar            ]
                              [    0     -------     0    ]
                              [          sqrt(2)          ]
                              [                           ]
                              [  hbar              hbar   ]
     (%o5)                    [ -------     0     ------- ]
                              [ sqrt(2)           sqrt(2) ]
                              [                           ]
                              [           hbar            ]
                              [    0     -------     0    ]
                              [          sqrt(2)          ]

1.2.4 Expectation value and variance
------------------------------------

 -- Function: expect (O,psi)
     Computes the quantum mechanical expectation value of the operator
     ‘O’ in state ‘psi’, ‘<psi|O|psi>’.

     (%i4) ev(expect(Sy,xp+ym),ratsimp);
     (%o4)                               - hbar

 -- Function: qm_variance (O,psi)
     Computes the quantum mechanical variance of the operator ‘O’ in
     state ‘psi’, ‘sqrt(<psi|O^{2}|psi> - <psi|O|psi>^{2})’.

     (%i4) ev(qm_variance(Sy,xp+ym),ratsimp);
                                         %i hbar
     (%o4)                               -------
                                            2

1.2.5 Angular momentum representation of kets and bras
------------------------------------------------------

To create kets and bras in the <|j,m>> representation you can use the
following functions.

 -- Function: jm_ket (j,m)
     ‘jm_ket’ creates the ket <|j,m>> for total spin <j> and
     <z>-component <m>.

 -- Function: jm_bra (j,m)
     ‘jm_bra’ creates the bra <<j,m|> for total spin <j> and
     <z>-component <m>.

     (%i4) jm_bra(3/2,1/2);
                                            [ 3  1 ]
     (%o4)                          [jmbra, [ -  - ]]
                                            [ 2  2 ]

 -- Function: jm_ketp (jmket)
     ‘jm_ketp’ checks to see that the ket has the 'jmket' marker.

 -- Function: jm_brap (jmbra)
     ‘jm_brap’ checks to see that the bra has the 'jmbra' marker.

 -- Function: jm_check (j,m)
     ‘jm_check’ checks to see that <m> is one of {-j, ..., +j}.

 -- Function: jm_braket (jmbra,jmket)
     ‘jm_braket’ takes the inner product of the jm-kets.

     (%i4) K:jm_ket(j1,m1);
     (%o4)                         [jmket, [ j1  m1 ]]
     (%i5) B:jm_bra(j2,m2);
     (%o5)                         [jmbra, [ j2  m2 ]]
     (%i6) jm_braket(B,K);
     (%o6)                                  0
     (%i7) B:jm_bra(j1,m1);
     (%o7)                         [jmbra, [ j1  m1 ]]
     (%i8) jm_braket(B,K);
     (%o8)                                  1

1.2.6 Angular momentum and ladder operators
-------------------------------------------

 -- Function: SP (s)
     ‘SP’ is the raising ladder operator <S_{+}> for spin ‘s’.

 -- Function: SM (s)
     ‘SM’ is the raising ladder operator <S_{-}> for spin ‘s’.

   Examples of the ladder operators:

     (%i4) SP(1);
                            [ 0  sqrt(2) hbar       0       ]
                            [                               ]
     (%o4)                  [ 0       0        sqrt(2) hbar ]
                            [                               ]
                            [ 0       0             0       ]
     (%i5) SM(1);
                            [      0             0        0 ]
                            [                               ]
     (%o5)                  [ sqrt(2) hbar       0        0 ]
                            [                               ]
                            [      0        sqrt(2) hbar  0 ]

1.3 Rotation operators
======================

 -- Function: RX (s,t)
     ‘RX(s)’ for spin ‘s’ returns the matrix representation of the
     rotation operator ‘Rx’ for rotation through angle ‘t’.

 -- Function: RY (s,t)
     ‘RY(s)’ for spin ‘s’ returns the matrix representation of the
     rotation operator ‘Ry’ for rotation through angle ‘t’.

 -- Function: RZ (s,t)
     ‘RZ(s)’ for spin ‘s’ returns the matrix representation of the
     rotation operator ‘Rz’ for rotation through angle ‘t’.

     (%i4) RZ(1/2,t);
     Proviso: assuming 64*t # 0
                                  [     %i t         ]
                                  [   - ----         ]
                                  [      2           ]
                                  [ %e          0    ]
     (%o4)                        [                  ]
                                  [             %i t ]
                                  [             ---- ]
                                  [              2   ]
                                  [    0      %e     ]

1.4 Time-evolution operator
===========================

 -- Function: UU (H,t)
     ‘UU(H,t)’ is the time evolution operator for Hamiltonian ‘H’.  It
     is defined as the matrix exponential ‘matrixexp(-%i*H*t/hbar)’.

     (%i4) UU(w*Sy,t);
     Proviso: assuming 64*t*w # 0
                                [     t w         t w  ]
                                [ cos(---)  - sin(---) ]
                                [      2           2   ]
     (%o4)                      [                      ]
                                [     t w        t w   ]
                                [ sin(---)   cos(---)  ]
                                [      2          2    ]

1.5 Tensor products
===================

Tensor products are represented as lists in Maxima.  The ket tensor
product ‘|z+,z+>’ is represented as ‘[tpket,zp,zp]’, and the bra tensor
product ‘<a,b|’ is represented as ‘[tpbra,a,b]’ for kets ‘a’ and ‘b’.
The list labels ‘tpket’ and ‘tpbra’ ensure calculations are performed
with the correct kind of objects.

 -- Function: ketprod (k_{1}, k_{2}, ...)
     ‘ketprod’ produces a tensor product of kets ‘k_{i}’.  All of the
     elements must pass the ‘ketp’ predicate test to be accepted.

 -- Function: braprod (b_{1}, b_{2}, ...)
     ‘braprod’ produces a tensor product of bras ‘b_{i}’.  All of the
     elements must pass the ‘brap’ predicate test to be accepted.

 -- Function: braketprod (B,K)
     ‘braketprod’ takes the inner product of the tensor products ‘B’ and
     ‘K’.  The tensor products must be of the same length (number of
     kets must equal the number of bras).

   Examples below show how to create tensor products and take the
bracket of tensor products.

     (%i4) ketprod(zp,zm);
                                          [ 1 ]  [ 0 ]
     (%o4)                       [tpket, [[   ], [   ]]]
                                          [ 0 ]  [ 1 ]
     (%i5) ketprod('zp,'zm);
                                all elements must be kets

     (%o5)                                done
     (%i4) kill(a,b,c,d);
     (%o4)                                done
     (%i5) declare([a,b,c,d],complex);
     (%o5)                                done
     (%i6) braprod(bra([a,b]),bra([c,d]));
     (%o6)                    [tpbra, [[ a  b ], [ c  d ]]]
     (%i7) braprod(dagger(zp),bra([c,d]));
     (%o7)                    [tpbra, [[ 1  0 ], [ c  d ]]]
     (%i4) K:ketprod(zp,zm);
                                          [ 1 ]  [ 0 ]
     (%o4)                       [tpket, [[   ], [   ]]]
                                          [ 0 ]  [ 1 ]
     (%i5) zpb:dagger(zp);
     (%o5)                              [ 1  0 ]
     (%i6) zmb:dagger(zm);
     (%o6)                              [ 0  1 ]
     (%i7) B:braprod(zpb,zmb);
     (%o7)                    [tpbra, [[ 1  0 ], [ 0  1 ]]]
     (%i8) braketprod(K,B);
     (%o8)                                false
     (%i9) braketprod(B,K);
     (%o9)                                  1

Appendix A Function and Variable index
**************************************

* Menu:

* bra:                                   Functions and Variables for qm.
                                                              (line  55)
* braket:                                Functions and Variables for qm.
                                                              (line 109)
* braketprod:                            Functions and Variables for qm.
                                                              (line 449)
* brap:                                  Functions and Variables for qm.
                                                              (line  90)
* braprod:                               Functions and Variables for qm.
                                                              (line 445)
* commutator:                            Functions and Variables for qm.
                                                              (line 247)
* dagger:                                Functions and Variables for qm.
                                                              (line 100)
* expect:                                Functions and Variables for qm.
                                                              (line 306)
* jm_bra:                                Functions and Variables for qm.
                                                              (line 332)
* jm_braket:                             Functions and Variables for qm.
                                                              (line 350)
* jm_brap:                               Functions and Variables for qm.
                                                              (line 344)
* jm_check:                              Functions and Variables for qm.
                                                              (line 347)
* jm_ket:                                Functions and Variables for qm.
                                                              (line 328)
* jm_ketp:                               Functions and Variables for qm.
                                                              (line 341)
* ket:                                   Functions and Variables for qm.
                                                              (line  34)
* ketp:                                  Functions and Variables for qm.
                                                              (line  72)
* ketprod:                               Functions and Variables for qm.
                                                              (line 441)
* magsqr:                                Functions and Variables for qm.
                                                              (line 133)
* norm:                                  Functions and Variables for qm.
                                                              (line 121)
* qm_variance:                           Functions and Variables for qm.
                                                              (line 313)
* RX:                                    Functions and Variables for qm.
                                                              (line 391)
* RY:                                    Functions and Variables for qm.
                                                              (line 395)
* RZ:                                    Functions and Variables for qm.
                                                              (line 399)
* sigmax:                                Functions and Variables for qm.
                                                              (line 216)
* sigmay:                                Functions and Variables for qm.
                                                              (line 219)
* sigmaz:                                Functions and Variables for qm.
                                                              (line 222)
* SM:                                    Functions and Variables for qm.
                                                              (line 370)
* SP:                                    Functions and Variables for qm.
                                                              (line 367)
* Sx:                                    Functions and Variables for qm.
                                                              (line 225)
* SX:                                    Functions and Variables for qm.
                                                              (line 265)
* Sy:                                    Functions and Variables for qm.
                                                              (line 228)
* SY:                                    Functions and Variables for qm.
                                                              (line 270)
* Sz:                                    Functions and Variables for qm.
                                                              (line 231)
* SZ:                                    Functions and Variables for qm.
                                                              (line 275)
* UU:                                    Functions and Variables for qm.
                                                              (line 418)
* xm:                                    Functions and Variables for qm.
                                                              (line 161)
* xp:                                    Functions and Variables for qm.
                                                              (line 158)
* ym:                                    Functions and Variables for qm.
                                                              (line 167)
* yp:                                    Functions and Variables for qm.
                                                              (line 164)
* zm:                                    Functions and Variables for qm.
                                                              (line 155)
* zp:                                    Functions and Variables for qm.
                                                              (line 152)

* Menu:

* hbar:                                  Functions and Variables for qm.
                                                               (line 29)


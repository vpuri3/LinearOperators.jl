# A [Julia](http://julialang.org) Linear Operator Package

Operators behave like matrices but are defined by their effect when applied to a vector. They can be transposed, conjugated, or combined with other operators cheaply. The costly operation is deferred until multiplied with a vector.

## How to Install

````JULIA
Pkg.clone("https://github.com/dpo/linop.jl.git")
````

## Example 1

Operators may be defined from matrices and combined using the usual operations, but the result is deferred until the operator is applied.

````JULIA
julia> A1 = rand(5,7);
julia> A2 = sprand(7,3,.3);
julia> op1 = LinearOperator(A1);
julia> op2 = LinearOperator(A2);
julia> op = op1 * op2;  # Does not form A1 * A2
julia> x = rand(3);
julia> y = op * x;
````

## Example 2

Operators may be defined to represent (approximate) inverses.

````JULIA
julia> A = rand(5,5); A = A' * A;
julia> op = opCholesky(A);  # Use, e.g., as a preconditioner
julia> v = rand(5);
julia> norm(A \ v - op * v) / norm(v)
1.6522645623951567e-14
````

## Example 3

Operators may be defined from functions. In this case, the transpose isn't defined, but it may be inferred from the conjugate transposed.

````JULIA
julia> dft = LinearOperator(10, 10, Float64, false, false,
                            v -> fft(v), Nothing(), w -> ifft(w));
julia> x = rand(10);
julia> y = dft * x;
julia> norm(dft' * y - x)  # DFT is an orthogonal operator
2.2929868617541516e-16
julia> dft.' * y
julia> 10-element Array{Complex{Float64},1}:
  0.927514+0.227908im
 0.0472611+0.62094im
 ...
````

## Testing

````JULIA
julia> Pkg.test("linop")
````

[![GPLv3](http://www.gnu.org/graphics/gplv3-88x31.png)](http://www.gnu.org/licenses/gpl.html "GPLv3")

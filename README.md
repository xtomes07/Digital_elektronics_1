# Digital_elektronics_1
## Nadpis
### Podnadpis
*kurzíva*
**tučně**
__tučne__

- jedna
- dva
- pet
- deset

1. vec1
5. vec2
10. vec3

(https://www.seznam.cz „Domovská stránka Seznam“)

První hlavička | Druhá hlavička
-------------- |---------------
1 | AND
2 | OR

##zdrojový kod
'''vhdl
architecture dataflow of gates is
begin
    for_o  <= a_i or b_i;
    fand_o <= a_i and b_i;
    fxor_o <= a_i xor b_i;

end architecture dataflow;
'''

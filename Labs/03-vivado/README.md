# Lab 3 :

## 1. Přípravné úkoly

### Tabulka s připojením 16 posuvných spínačů(SWITCHU) a 16 LED diod pro desku Nexys A7 

| | **SWITCH => PIN** | **LED => PIN** |
| :-: | :-: | :-: |
| 0 | SW0 => J15 | LD0 => H17 |
| 1 | SW1 => L16 | LD1 => K15 |
| 2 | SW2 => M13 | LD2 => J13 |
| 3 | SW3 => R15 | LD3 => N14 |
| 4 | SW4 => R17 | LD4 => R18 |
| 5 | SW5 => T18 | LD5 => V17 |
| 6 | SW6 => U18 | LD6 => U17 |
| 7 | SW7 => R13 | LD7 => U16 |
| 8 | SW8 => T8 | LD8 => V16 |
| 9 | SW9 => U8 | LD9 => T15 |
| 10 | SW10 => R16 | LD10 =>  U14 |
| 11 | SW11 => T13 | LD11 => T16 |
| 12 | SW12 => H6 | LD12 => V15 |
| 13 | SW13 => U12 | LD13 => V14 |
| 14 | SW14 => U11 | LD14 => V12 | 
| 15 | SW15 => V10 | LD15 => V11 |

## 2. Dvoubitový multiplexor 4=>1

### Zdrojový kód z mux_2bit_4to1.vhd

```vhdl
architecture Behavioral of mux_2bit_4to1 is
begin
    f_o <= a_i when (sel_i = "00") else
           b_i when (sel_i = "01") else
           c_i when (sel_i = "10") else
           d_i;
    
end architecture Behavioral;
```

### Zdrojový kód z tb_mux_2bit_4to1.vhd

```vhdl
p_stimulus : process
    begin
    
    -- Report a note at the begining of stimulus process
        report "Stimulus process started" severity note;


        
        s_d <= "00"; s_c <= "00"; s_b <= "00"; s_a <= "00";
        s_sel <= "00"; wait for 100ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "01"; s_a <= "00";
        s_sel <= "00"; wait for 100ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "01"; s_a <= "11";
        s_sel <= "00"; wait for 100ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "01"; s_a <= "00";
        s_sel <= "01"; wait for 100ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "11"; s_a <= "00";
        s_sel <= "01"; wait for 100ns;
        

        -- Report a note at the end of stimulus process
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;    
```

### Screenshot se simulovanými časovými průběhy
![grafy](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/03-vivado/Obrazky/prubehy.PNG)

## 3 Návod k Vivado

### Tvorba nového projektu:
1. Na úvodní obrazovce zvolit "Create Project"
2. Vyplnit "Project name" a Project "location" 
3. Na strane Project Type zvolit "RTL Project" a nic nezaškrtávat
4. Na Add Sources => Create File => File type: VHDL, zvolit File => OK a přednout Target language a Simulator language do VHDL
5. NA Add Constraints => preskocit
6. Na Deafault Part => Boars => Search: Nexys A7-100t a zvolit ji
7. Na Nes Project summary to bude u hazet chybu na tretim radku, vse ok neresit => Finish
8. Po vytvořeni projektu vyskoci tabulka Define Module => nic nedelat jen OK a Yes
9. V okne Source se nam zobrazí soubor se zdrojovým kódem

### Přidání souboru testbench
1. V otevrenem projektu => File => Add sources
2. V Add sources vybrat "Add or create simulation sources" => Next
3. V Add or Create Desing Sources => Create File => File type VHDL, File name s predponou "tb_" => Finish
4. Vyskocí okno Define Module => nic nedelat jen OK a Yes

### Spouštění simulace
1. V levém horním rohu => Flow => Run simulation => Run Behavioral Simulation

### Přidání XDC souboru
1. V otevrenem projektu => File => Add sources
2. V Add sources vybrat "Add or create constraints" => Next
3. V Add or Create Constraints => Create File => File type:XDC, File name: "nexys-a7-50t" => Finish
4. Otevrem si  Sources => Constraints => a nas vytvoreny soubor "nexys-a7-50t"
5. do toho souboru nakopirujem ze stranky https://github.com/Digilent/digilent-xdc => Nexys-A7-50T-Master.xdc zdrojový kód, který nasledně upravíme
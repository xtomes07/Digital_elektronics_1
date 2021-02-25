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




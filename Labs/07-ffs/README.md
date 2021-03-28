# Lab 7

## 1. Přípravné úkoly

![Characteristic equations](Images/eq_flip_flops.png)
<!--
\begin{align*}
   q_{n+1}^D =& d \\
    q_{n+1}^{JK} =& j\overline{q}_n\,+\overline{k}q_n\\\
    q_{n+1}^T =& t\overline{q}_n\,+\overline{t}q_n\\\
\end{align*}-->

   | **clk** | **d** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 0 | 0 | Input signal is sampled at the rising edge of clk and stored to FF |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 1 | 0 | Input signal is sampled at the rising edge of clk and stored to FF |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 0 | 1 | Input signal is sampled at the rising edge of clk and stored to FF |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 1 | 1 | Input signal is sampled at the rising edge of clk and stored to FF |

   | **clk** | **j** | **k** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 0 | 0 | 0 | No change |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 0 | 1 | 1 | No change |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 1 | 0 | 0 | Reset |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 1 | 1 | 0 | Reset |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 0 | 0 | 1 | Set |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 0 | 1 | 1 | Set |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 1 | 0 | 1 | Toggle |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 1 | 1 | 0 | Toggle |

   | **clk** | **t** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 0 | 0 | no change |
   | ![rising](Obrázky/eq_uparrow.png) | 0 | 1 | 1 | no change |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 0 | 1 | invert |
   | ![rising](Obrázky/eq_uparrow.png) | 1 | 1 | 0 | invert |



## 2. D-latch

### Výpis kódu procesu p_d_latch

```vhdl
p_d_latch : process (d, arst, en)
begin
    if (arst= '1') then
        q     <= '0';
        q_bar <= '1';
   
    elsif (en = '1') then
            q <= d;
            q_bar <= not d;

    end if;
end process p_d_latch;
```

### Výpis reset a stimulus kódu z tb_d_latch.vhd

```vhdl
--------------------------------------------------------------------
-- Reset generation process
--------------------------------------------------------------------
    p_reset_gen : process
    begin 
        s_arst <= '0';
	   wait for 38 ns;
	 
	   s_arst <= '1';
	   wait for 53 ns;

	   s_arst <= '0';
	
	   wait for 80 ns;
	   s_arst <= '1';

	 wait;
        end process p_reset_gen;

--------------------------------------------------------------------
-- Data generation process
--------------------------------------------------------------------
    p_stimulus :process
    begin
        report "Stimulus peocess started" severity note;
	
	s_d  <= '0';
	s_en <= '0';
	
	--d sekvence
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '0') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed for reset:1 , en: 0 when s_d: 0" severity error;
	
	wait for 5 ns;
	s_d  <= '1';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '0') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed for reset: 1, en: 0 when s_d: 1" severity error;
	
	wait for 5 ns;
	s_d  <= '0';
	--/d sekv
	
	s_en <= '1';
	
	--d sekv
	wait for 10 ns;
	s_d  <= '1';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed for reset: 1, en: 1 when s_d: 1" severity error;
	
	wait for 5 ns;
	s_d  <= '0';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed for reset: 1, en: 1 when s_d : 0" severity error;          
	
	wait for 5 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 5 ns;
	
	assert ((s_arst = '0') and (s_en = '1') and (s_q = '1') and (s_q_bar = '0'))
	report "Test failed for reset: 0, en: 1 when s_d = :1" severity error;
	
	wait for 15 ns;
	s_d  <= '0';
	wait for 5 ns;
	
	assert ((s_arst = '0') and (s_en = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed for reset: 0, en: 1 when s_d : 0" severity error;
	
	--/d sekvence
	
	--d sekvence
	wait for 5 ns;
	s_d  <= '1';
	wait for 5 ns;
	s_en <= '0';
	wait for 5 ns;
	s_d  <= '0';

	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	--/d sekvence 
	
        report "Stimulus process finished" severity note;
        wait;
        end process p_stimulus;
```
### Screen ze simulace
![screen_simulace](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/07-ffs/Obr%C3%A1zky/D-latch.PNG)

## 3. Flip_flops

### VHDL kódy z procesů

#### p_d_ff_arst

```vhdl
p_d_ff_arst : process (clk, arst)
begin
    if (arst= '1') then
        q     <= '0';
        q_bar <= '1';
   
    elsif rising_edge(clk) then
            q <= d;
            q_bar <= not d;

    end if;
end process p_d_ff_arst;
```

#### p_d_ff_rst

```vhdl
p_d_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                q <= '0';
                q_bar <= '1';
            else
                q <= d;
                q_bar <= not d;
            end if;
        end if;
    end process p_d_ff_rst;
```

#### p_jk_ff_rst


```vhdl
p_jk_ff_rst : process (clk)
    begin
      
         if rising_edge(clk) then
            if (rst = '1')  then
                s_q <= '0';
            else
                 if (j = '0' and k= '0') then
                  s_q <= s_q;
                
                 elsif (j = '0' and k= '1') then
                    s_q <= '0';
                
                 elsif (j = '1' and k= '0') then
                    s_q <= '1';
                
                 elsif (j = '1' and k= '1') then
                    s_q <= not s_q;
                
                 end if;
             end if;
         end if;
```

#### p_t_ff_rst


```vhdl
 p_t_ff_rst : process (clk)
begin
    if rising_edge(clk) then
        if (rst = '1') then
            s_q <= '0';
        elsif (t = '1') then
            s_q <= not s_q;
        end if;
    end if;

end process p_t_ff_rst;
```

### Výpis VHDL z jednotlivých testbenchů

#### tb_d_ff_arst
```vhdl
--------------------------------------------------------------------
-- Clock generation process
--------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 40 ms loop        
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
--------------------------------------------------------------------
-- Reset generation process
--------------------------------------------------------------------

     p_reset_gen : process
        begin
            s_arst <= '0';
            wait for 28 ns;
            
            s_arst <= '1';
            wait for 13 ns;
    
            s_arst <= '0';
            wait for 17 ns;
            
            s_arst <= '1';
            wait for 33 ns;
            
            wait for 660 ns;
            s_arst <= '1';
    
            wait;
     end process p_reset_gen;

--------------------------------------------------------------------
-- Data generation process
--------------------------------------------------------------------
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        s_d  <= '0';
        
        --d sekvence
        wait for 14 ns;
        s_d  <= '1';
        wait for 2 ns;
        
        assert ((s_arst = '0') and (s_q = '1') and (s_q_bar = '0'))
        report "Test failed for reset:0, after clk rising when s_d:1'" severity error;
        
        wait for 8 ns;
        s_d  <= '0';
        wait for 6 ns;
                
        wait for 4 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 5 ns;
        
        assert ((s_arst = '1') and (s_q = '0') and (s_q_bar = '1'))
        report "Test failed for reset: 1, before clk rising when s_d: 0" severity error;
        
        wait for 5 ns;
        s_d  <= '0';
        --/d sekvence
        
        --d sekvence
        wait for 14 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        --/d sekvence
        
       
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```
#### tb_d_ff_rst
```vhdl
--------------------------------------------------------------------
-- Clock generation process
--------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 40 ms loop        
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
--------------------------------------------------------------------
-- Reset generation process
--------------------------------------------------------------------

     p_reset_gen : process
        begin
            s_rst <= '0';
            wait for 28 ns;
            
            s_rst <= '1';
            wait for 13 ns;
    
            s_rst <= '0';
            wait for 17 ns;
            
            s_rst <= '1';
            wait for 33 ns;
            
            wait for 660 ns;
            s_rst <= '1';
    
            wait;
     end process p_reset_gen;

--------------------------------------------------------------------
-- Data generation process
--------------------------------------------------------------------
p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        s_d  <= '0';
        
        --d sekvence
        wait for 14 ns;
        s_d  <= '1';
        wait for 2 ns;
        
        assert ((s_rst = '0') and (s_q = '1') and (s_q_bar = '0'))
        report "Test failed for reset low, after clk rising when s_d = '1'" severity error;
        
        wait for 8 ns;
        s_d  <= '0';
        wait for 6 ns;
        
        wait for 4 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 5 ns;
        

        assert ((s_rst = '1') and (s_q = '1') and (s_q_bar = '0'))
        report "Test failed for reset:1 , before clk rising when s_d =: 0" severity error;
        
        wait for 5 ns;
        s_d  <= '0';
        --/d sekvence
        
        --d sekvence
        wait for 14 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        --/d sekvence
        
       
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```
#### tb_jk_ff_rst
```vhdl
-------------------------------------------------------------------- 
-- Clock generation process                                          
-------------------------------------------------------------------- 
    p_clk_gen : process                                              
    begin                                                            
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';                                     
            wait for c_CLK_100MHZ_PERIOD / 2;                        
            s_clk_100MHz <= '1';                                     
            wait for c_CLK_100MHZ_PERIOD / 2;                        
        end loop;                                                    
        wait;                                                        
    end process p_clk_gen;    
    
--------------------------------------------------------------------
-- Reset generation process                                         
--------------------------------------------------------------------
    p_reset_gen : process                                           
    begin                                                           
        s_rst <= '0';
        wait for 18 ns;
            
        s_rst <= '1';
        wait for 13 ns;
    
        s_rst <= '0';
        wait for 47 ns;
            
        s_rst <= '1';
        wait for 33 ns;
            
        wait for 660 ns;
        s_rst <= '1';                                            
        wait;                                                       
        end process p_reset_gen;                                    
 
--------------------------------------------------------------------
-- Data generation process                                          
--------------------------------------------------------------------
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        s_j  <= '0';
        s_k  <= '0';

        wait for 38 ns;
        
        assert ((s_rst = '0') and (s_j = '0') and (s_k = '0') and (s_q = '0') and (s_q_bar = '1'))
        report "Test 'no change' failed for reset: 0, after clk rising when s_j: 0 and s_k: 0" severity error;
        
        wait for 2 ns;
        s_j  <= '1';
        s_k  <= '0';
        wait for 6 ns;
        
        assert ((s_rst = '0') and (s_j = '1') and (s_k = '0') and (s_q = '1') and (s_q_bar = '0'))
        report "Test 'set' failed for reset: 0, after clk rising when s_j: 1 and s_k: 0'" severity error;
        
        wait for 1 ns;
        s_j  <= '0';
        s_k  <= '1';
        wait for 13 ns;
        
        assert ((s_rst = '0') and (s_j = '0') and (s_k = '1') and (s_q = '0') and (s_q_bar = '1'))
        report "Test 'reset' failed for reset: 0, after clk rising when s_j: 0 and s_k: 1" severity error;
        
        wait for 1 ns;
        s_j  <= '1';
        s_k  <= '0';
        wait for 7 ns;
        s_j  <= '1';
        s_k  <= '1';
        
        wait for 8 ns;
        
        assert ((s_rst = '0') and (s_j = '1') and (s_k = '1') and (s_q = '0') and (s_q_bar = '1'))
        report "Test 'toggle' failed for reset: 0, after clk rising when s_j: 1 and s_k: 1" severity error;
        
        wait for 2 ns;
        s_j  <= '0';
        s_k  <= '0';
        wait for 7 ns;
        s_j  <= '0';
        s_k  <= '1';
        wait for 7 ns;
        s_j  <= '1';
        s_k  <= '0';
        wait for 7 ns;
        s_j  <= '1';
        s_k  <= '1';
        
        report "Stimulus process finished" severity note;
        wait;                                
                                                                                                                                                                              
    end process p_stimulus;
```
#### tb_t_ff_rst
```vhdl
--------------------------------------------------------------------
-- Clock generation process
--------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 40 ms loop        
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
--------------------------------------------------------------------
-- Reset generation process
--------------------------------------------------------------------

     p_reset_gen : process
        begin
            s_rst <= '0';
            wait for 18 ns;
            
            -- Reset activated
            s_rst <= '1';
            wait for 13 ns;
    
            --Reset deactivated
            s_rst <= '0';
            
            wait for 47 ns;
            
            s_rst <= '1';
            wait for 33 ns;
            
            wait for 660 ns;
            s_rst <= '1';
    
            wait;
     end process p_reset_gen;

--------------------------------------------------------------------
-- Data generation process
--------------------------------------------------------------------
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        s_t  <= '0';
        
        wait for 38 ns;
        
        assert ((s_rst = '0') and (s_t = '0') and (s_q = '0') and (s_q_bar = '1'))
        report "Test 'no change' failed for reset: 0, after clk rising when s_t: 0" severity error;
        
        wait for 2 ns;
        s_t  <= '1';
        wait for 6 ns;
        
        assert ((s_rst = '0') and (s_t = '1') and (s_q = '1') and (s_q_bar = '0'))
        report "Test 'toggle' failed for reset: 0, after clk rising when s_t: 0" severity error;
        
        wait for 1 ns;
        s_t  <= '0';
        wait for 13 ns;
        
        assert ((s_rst = '0') and (s_t = '0') and (s_q = '1') and (s_q_bar = '0'))
        report "Test 'no change' failed for reset: 0, after clk rising when s_t: 0" severity error;
        
        wait for 1 ns;
        s_t  <= '1';
        wait for 5 ns;
        
        assert ((s_rst = '0') and (s_t = '1') and (s_q = '0') and (s_q_bar = '1'))
        report "Test 'toggle' failed for reset:0 , after clk rising when s_t: 1" severity error;
        
        wait for 12 ns;
        s_t  <= '0';
        wait for 7 ns;
        s_t  <= '1';
        wait for 7 ns;
        s_t  <= '0';
        wait for 7 ns;
        s_t  <= '1';

    
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

### Screenshoty ze simulaci

#### tb_d_ff_arst
![screen_simulace1](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/07-ffs/Obr%C3%A1zky/d_ff_arst.PNG)
#### tb_d_ff_rst
![screen_simulace2](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/07-ffs/Obr%C3%A1zky/d_ff_rst.PNG)
#### tb_jk_ff_rst
![screen_simulace3](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/07-ffs/Obr%C3%A1zky/jk_ff_rst.PNG)
#### tb_t_ff_rst
![screen_simulace4](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/07-ffs/Obr%C3%A1zky/t_ff.PNG)
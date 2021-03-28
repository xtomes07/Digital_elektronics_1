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
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | Input signal is sampled at the rising edge of clk and stored to FF |
   | ![rising](Images/eq_uparrow.png) | 0 | 1 | 0 | Input signal is sampled at the rising edge of clk and stored to FF |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | Input signal is sampled at the rising edge of clk and stored to FF |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 1 | Input signal is sampled at the rising edge of clk and stored to FF |

   | **clk** | **j** | **k** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | 0 | No change |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 1 | 1 | No change |
   | ![rising](Images/eq_uparrow.png) | 0 | 1 | 0 | 0 | Reset |
   | ![rising](Images/eq_uparrow.png) | 0 | 1 | 1 | 0 | Reset |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 0 | 1 | Set |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | 1 | Set |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | 1 | Toggle |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 1 | 0 | Toggle |

   | **clk** | **t** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | no change |
   | ![rising](Images/eq_uparrow.png) | 0 | 1 | 1 | no change |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | invert |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | invert |



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

![screen_simulace](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/06-display_driver/Obr%C3%A1zky/imag_8digit.png)





# Lab 6

## 1. Přípravné úkoly
### Časový diagram pro zobrazení hodnoty 3.142

![diagram](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/06-display_driver/Obr%C3%A1zky/1_prubehy.PNG)



## 2. Ovladač displeje

### Výpis VHDL kódu procesu p_mux

```vhdl
p_mux : process(s_cnt, data0_i, data1_i, data2_i, data3_i, dp_i)
    begin
        case s_cnt is
            when "11" =>
                s_hex <= data3_i;
                dp_o  <= dp_i(3);
                dig_o <= "0111";

            when "10" =>
                s_hex <= data2_i;
                dp_o  <= dp_i(2);
                dig_o <= "1011";

            when "01" =>
                s_hex <= data1_i;
                dp_o  <= dp_i(1);
                dig_o <= "1101";

            when others =>
                s_hex <= data0_i;
                dp_o  <= dp_i(0);
                dig_o <= "1110";
        end case;
    end process p_mux;
```

### Výpis VHDL kódu procesu tb_driver_7seg_4digits

```vhdl
------------------------------------------------------------------------

library ieee;
use ieee.std_logic_1164.all;

------------------------------------------------------------------------
-- Entity declaration for testbench
------------------------------------------------------------------------
entity tb_driver_7seg_4digits is
    -- Entity of testbench is always empty
end entity tb_driver_7seg_4digits;

------------------------------------------------------------------------
-- Architecture body for testbench
------------------------------------------------------------------------
architecture testbench of tb_driver_7seg_4digits is

    -- Local constants
    constant c_CLK_100MHZ_PERIOD : time    := 10 ns;

    --Local signals
    signal s_clk_100MHz : std_logic;
    --- WRITE YOUR CODE HERE
    signal s_reset : std_logic ;
    
    signal s_data0 : std_logic_vector(4-1 downto 0);
    signal s_data1 : std_logic_vector(4-1 downto 0);
    signal s_data2 : std_logic_vector(4-1 downto 0);
    signal s_data3 : std_logic_vector(4-1 downto 0);
                                                             
     signal s_dp_i : std_logic_vector(4-1 downto 0); 
     signal s_dp_o : std_logic;   
     signal s_seg  : std_logic_vector(7-1 downto 0); 
     signal s_dig  : std_logic_vector(4-1 downto 0);                                                     
begin
    -- Connecting testbench signals with driver_7seg_4digits entity
    -- (Unit Under Test)
    utt_river_7seg_4digits : entity work.driver_7seg_4digits
    port map(
        clk     =>s_clk_100MHz,
        reset   => s_reset,
        
        data0_i => s_data0,
        data1_i => s_data1,
        data2_i => s_data2,
        data3_i => s_data3,
        
        dp_i    => s_dp_i,
        dp_o    => s_dp_o,
        seg_o   => s_seg,
        dig_o   => s_dig
    );
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
        s_reset <= '0';
        wait for 28ns;
        --reset activation
        s_reset <= '1';
        wait for 53 ns;
        --Reset deactivated
        s_reset <= '0';
        wait;
        end process p_reset_gen;

    --------------------------------------------------------------------
    -- Data generation process
    --------------------------------------------------------------------
    p_stimulus :process
    begin
        report "Stimulus peocess started" severity note;
        --3,142
        s_data3 <= "0011";
        assert (s_dig <= "0111")
        report "Test failed for input s_data3: 0011" severity error;
        
        s_data2 <= "0001";
        assert (s_dig <= "1011")
        report "Test failed for input s_data2: 0001" severity error;
        
        s_data1 <= "0100";
        assert (s_dig <= "1101")
        report "Test failed for input s_data1: 0100" severity error;
               
        s_data0 <= "0010";
        assert (s_dig <= "1110")
        report "Test failed for input s_data0: 0010" severity error;
       
        s_dp_i  <= "0111";
      
        
        report "Stimulus process finished" severity note;
        wait;
        end process p_stimulus;
```


## 3. T

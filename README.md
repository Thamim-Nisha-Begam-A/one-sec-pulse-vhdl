# one-sec-pulse-vhdl
--This VHDL code is a one-second pulse generator, assuming a clock frequency of 50 MHz
--temp signal: Acts as a 24-bit counter.
--100110001001011010000000 in binary = 5,000,000 in decimal.
--y signal: Toggles every second to form a pulse.

library ieee; 
use ieee.std_logic_1164.all; 
use ieee.std_logic_unsigned.all; 
entity onesecc is 
   port(clk,clr: in std_logic; 
      cout: out std_logic_vector(23 downto 0)); 
end onesecc; 
architecture behave of onesecc is 
signal temp: std_logic_vector(23 downto 0);
signal y:std_logic; 
begin 
   cnt_23:process(clk,clr) 
   begin 
      if (clr = '1' or temp = "100110001001011010000000") then 
         temp <= "000000000000000000000000"; 
         --y<='0';
   	  elsif rising_edge(clk) then 
         temp <= temp+1; 
   	  end if; 
   end process cnt_23;
   pulse:process(temp,clr,clk)
   begin
       if clr='1' then
           y<='0';
           elsif falling_edge(clk) then
              if(temp="100110001001011010000000")then 
                 y<= not y;
              end if;
          end if;
  end process;
  
cout<=temp;
end behave; 

# Codec-project-
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.NUMERIC_STD.ALL;

entity smart_lock is
Port(
clk         : in STD_LOGIC;
reset       : in STD_LOGIC;
key_in      : in STD_LOGIC_VECTOR(3 downto 0);

unlock      : out STD_LOGIC;
alert       : out STD_LOGIC
);
end smart_lock;

architecture Behavioral of smart_lock is

type pass_array is array(0 to 3)
of STD_LOGIC_VECTOR(3 downto 0);

constant PASSWORD : pass_array :=
("0001","0010","0011","0100");

signal index : integer range 0 to 4 := 0;
signal attempts : integer range 0 to 3 := 0;

begin

process(clk,reset)

begin

if reset='1' then

index <=0;
attempts<=0;

unlock<='0';
alert<='0';

elsif rising_edge(clk) then

if key_in=PASSWORD(index) then

index<=index+1;

if index=3 then
unlock<='1';
index<=0;
end if;

else

attempts<=attempts+1;
index<=0;

if attempts=2 then
alert<='1';
end if;

end if;

end if;

end process;

end Behavioral;
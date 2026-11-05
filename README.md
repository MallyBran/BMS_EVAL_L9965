Zhodnotenie BMS čipsetu ST L9965 s doskou NUCLEO-H723ZG
Použité :
    NUCLEO-H723ZG – riadiace MCU, SPI master
    
   EVL9965T – L9965T, izolovaný SPI transceiver (dual-die, 2 kanále, twisted-pair VIF). Most medzi logickou a izolovanou batériovou doménou.

  EVL9965A  – L9965A, 18-článkový battery monitor s balansovaním (400 mA/článok) + load board na emuláciu článkov pred zapojením skutočného 18S packu
    
  Komunikácia beží v režime VIF daisy chain: MCU → SPI → L9965T → izolovaný twisted pair → L9965A.
    
  STM ponúka firmware STSW-L9965FW, ten je však len pre STM32L4, preto bude preportovaný na naše STM.
    V balíku na preportovanie sa nachádza GUI na management BMS.
    
  Optional : EVL9965C
    

L9965C (monitor prúdu, náboja a izolácie HV packu – battery junction box) sa pripojí do tej istej VIF reťaze.
Testovanie článkov prebieha na Samsung Liíon 18650, neskôr možno Grepow LiPo

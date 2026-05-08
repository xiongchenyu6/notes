---
title: "POSIX"
date: 2021-12-04
---

# Charactor Class

  POSIX class    Equivalent to                                     Matches                                                                                                                 
  -------------- ------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------
  [:alnum:]    [A-Za-z0-9]                                     digits, uppercase and lowercase letters                                                                                 
  [:alpha:]    [A-Za-z]                                        upper- and lowercase letters                                                                                            
  [:ascii:]    [`\x00`-`\x`7F]                 ASCII characters                                                                                                        
  [:blank:]    [ `\t]`                                  space and TAB characters only                                                                                           
  [:cntrl:]    [`\x00`-`\x`1F`\x`7F]   Control characters                                                                                                      
  [:digit:]    [0-9]                                           digits                                                                                                                  
  [:graph:]    [\^ [:cntrl:]]                                graphic characters (all characters which have graphic representation)                                                   
  [:lower:]    [a-z]                                           lowercase letters                                                                                                       
  [:print:]    [[:graph:] ]                                  graphic characters and space                                                                                            
  [:punct:]    [-!"#\$%&\'()\*+,./:;\<=>?@[]\^\_\`{         }~]                                                                                                                   all punctuation characters (all graphic characters except letters and digits)
  [:space:]    [ `\t\n``\r\f\v]`                all blank (whitespace) characters, including spaces, tabs, new lines, carriage returns, form feeds, and vertical tabs   
  [:upper:]    [A-Z]                                           uppercase letters                                                                                                       
  [:word:]     [A-Za-z0-9\_]                                   word characters                                                                                                         
  [:xdigit:]   [0-9A-Fa-f]                                     hexadecimal digits

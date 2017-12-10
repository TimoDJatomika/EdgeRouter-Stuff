# squidguard proxy
You can use your Edge Router as a proxy server to block certain categories e.g. ads or malware.

## prerequisite
SSH into your Edge Router and download the available catogories. Depending on your device this could take a few minutes (It took about 100 minutes on my device).

```
update webproxy blacklists
```

## example config 
```
set service webproxy cache-size 0
set service webproxy default-port 3128
set service webproxy listen-address 172.22.3.1
set service webproxy mem-cache-size 5
set service webproxy url-filtering squidguard block-category ads
set service webproxy url-filtering squidguard block-category porn
set service webproxy url-filtering squidguard default-action allow
set service webproxy url-filtering squidguard redirect-url 'https://brainoftimo.com/not-for-you'
```
## possible categories to block
- ads                       
- adult                     
- aggressive                
- agressif                  
- arjel                     
- associations_religieuses  
- astrology                 
- audio-video               
- bank                      
- bitcoin                   
- blog                      
- celebrity                 
- chat                      
- child                     
- cleaning                  
- cooking                   
- cryptojacking             
- dangerous_material        
- dating                    
- ddos      
- dialer                    
- download                  
- drogue                    
- drugs                                    
- educational_games                        
- filehosting                              
- financial                                
- forums                                   
- gambling                                 
- games                                    
- hacking                                  
- jobsearch                                
- lingerie                                 
- liste_blanche                            
- liste_bu                                 
- local-ok-default                         
- local-ok-url-default                     
- mail                                     
- malware                                  
- manga                                    
- marketingware                            
- mixed_adult                              
- mobile-phone                             
- phishing                                 
- porn                      
- press 
- proxy
- publicite
- radio
- reaffected
- redirector
- remote-control
- sect
- sexual_education
- shopping
- shortener
- social_networks
- special
- sports
- strict_redirector
- strong_redirector
- translation
- tricheur
- update
- violence
- warez
- webmail

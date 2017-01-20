# VLAN-BOREAL-LAB
Creation de VLAN pour les besoins de laboratoire au college Boreal Toronto

![alt tag](https://github.com/CollegeBoreal/VLAN-BOREAL-LAB/blob/master/VLAN-LAB.png)


##Switch  
// Configuration initiale: Nom et mot de passe 
```
Switch>
Switch>enable
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname switch_principal                                ^

switch_principal(config)#banner motd # Switch principal #
switch_principal(config)#line con 0
switch_principal(config-line)#password cisco
switch_principal(config-line)#login
switch_principal(config-line)#line vty 0 4
switch_principal(config-line)#password cisco
switch_principal(config-line)#login
switch_principal(config-line)#exit
switch_principal(config)#enable secret cisco
```
// Creation des VLANs
```
switch_principal(config)#vlan 10
switch_principal(config-vlan)#name administration
switch_principal(config-vlan)#vlan 20
switch_principal(config-vlan)#name inf1021
switch_principal(config-vlan)#vlan 30
switch_principal(config-vlan)#name inf1041
switch_principal(config-vlan)#vlan 40
switch_principal(config-vlan)#name brice
switch_principal(config-vlan)#exit
switch_principal(config)#exit
switch_principal#
```

```
switch_principal#sh vlan
```
// Ajouter les ports aux differents VLANs 
```

switch_principal#config t
Enter configuration commands, one per line.  End with CNTL/Z.
switch_principal(config)#int range f0/1-3
switch_principal(config-if-range)#switchport mode access
switch_principal(config-if-range)#switchport access vlan 10
switch_principal(config-if-range)#int range f0/4-7
switch_principal(config-if-range)#switchport mode access
switch_principal(config-if-range)#switchport access vlan 20
switch_principal(config-if-range)#int range f0/8-10
switch_principal(config-if-range)#switchport mode access
switch_principal(config-if-range)#switchport access vlan 30
switch_principal(config-if-range)#int range f0/11-13
switch_principal(config-if-range)#switchport mode access
switch_principal(config-if-range)#switchport access vlan 40
switch_principal(config-if-range)#exit
switch_principal(config)#exit
switch_principal#

switch_principal#
switch_principal#copy run start
```

// Creation du Trunk Inter- VLAN

```
switch_principal(config)#int g0/1
switch_principal(config-if)#switchpor mode trunk
```

-----------------------

##Routeur : 
// Configuration initiale du routeur.  

```
Router>
Router>en
Router#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname routeur_lab
routeur_lab(config)#banner motd # Routeur Lab #
routeur_lab(config)#enable secret cisco
routeur_lab(config)#line con 0
routeur_lab(config-line)#password cisco
routeur_lab(config-line)#login
routeur_lab(config-line)#line vty 0 4
routeur_lab(config-line)#password cisco
routeur_lab(config-line)#login
routeur_lab(config-line)#line aux 0 
routeur_lab(config-line)#password cisco
routeur_lab(config-line)#login
routeur_lab(config-line)#exit
routeur_lab(config)#service password-en
routeur_lab(config)#service password-encryption 
```

// Configuration des sous-interfaces pour Inter-VLAN

```
routeur_lab(config)#int g0/0.10

routeur_lab(config-subif)#encapsulation dot1Q 10
routeur_lab(config-subif)#ip address 10.13.237.1 255.255.255.240
routeur_lab(config-subif)#no sh
routeur_lab(config-subif)#int g0/0.20
routeur_lab(config-subif)#encapsulation dot1Q 20
routeur_lab(config-subif)#ip address 10.13.237.81 255.255.255.240
routeur_lab(config-subif)#no sh
routeur_lab(config-subif)#int g0/0.30
routeur_lab(config-subif)#encapsulation dot1Q 30
routeur_lab(config-subif)#ip address 10.13.237.113 255.255.255.240
routeur_lab(config-subif)#no sh
routeur_lab(config-subif)#int g0/0.40
routeur_lab(config-subif)#encapsulation dot1Q 40
routeur_lab(config-subif)#ip address 10.13.237.145 255.255.255.240
routeur_lab(config-subif)#no sh
routeur_lab(config-subif)#
```

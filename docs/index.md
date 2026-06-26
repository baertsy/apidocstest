# MiniCRM API Alapok

A MiniCRM API használatához és a kérések hitelesítéséhez a **SystemID** és az **API kulcs** elengedhetetlen.

## SystemID
A SystemID egy 5 számjegyből álló alapfogalom a MiniCRM API-ban, amely a kérések azonosítására szolgál. Ez egy stabil azonosító, a jövőben sem fog változni. 

* **Hol található?** A böngésző címsorában a domain után, például: `r3.minicrm.hu/51546/TodayView` (ebben az esetben a `51546` a SystemID).
* **cURL példa:** `curl https://r3.minicrm.hu/Api...`

## API Kulcs (API Key) generálása
Az API használatához API kulcsra van szükség. Ezt kizárólag egy adminisztrátor generálhatja a MiniCRM rendszerén belül a **Beállítások -> Rendszer** oldalon. 

> **Fontos:** A generált kulcs csak **egyszer** jelenik meg. Kérjük, azonnal mentse el egy biztonságos helyre! Ha elveszíti, újat kell generálnia, ami azt jelenti, hogy a meglévő integrációkat is frissítenie kell az új kulccsal.

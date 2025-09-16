<script setup lang="ts">
const selectedCampaign = ref<string | null>(null)
const currentLine = ref(0)
const bootTextElement = ref<HTMLElement>()
const menuElement = ref<HTMLElement>()

const bootSequence = [
  "MONARCH CORPORATION CONTRACTOR TERMINAL",
  "Model: MCT-2384 | Firmware: v7.3.2",
  "",
  "Initializing...",
  "",
  "Checking system integrity... ",
  "Memory test: 2048MB... OK",
  "Network interface: CONNECTED",
  "Encryption module: ACTIVE",
  "Authentication service: READY",
  "",
  "WARNING: Unauthorized access prohibited",
  "All activities are monitored and recorded",
  "",
  "Loading contractor database...",
  "",
  "Scanning for active contracts...",
  "████████████████████████████████████████████████ 100%",
  "",
  "4 assignments found",
  "Clearance level: STANDARD",
  "",
  "Boot complete. Welcome to MONARCH employment services.",
  "",
  "Loading mission selection interface..."
]

onMounted(() => {
  setTimeout(bootNextLine, 1000)
})

function typeText(text: string, callback: () => void) {
  let i = 0
  const currentDiv = document.createElement('div')
  
  if (text.includes('Loading') || text.includes('Scanning') || text.includes('Checking') || text.includes('Initializing')) {
    currentDiv.className = 'hitching'
  }
  
  if (text.includes('WARNING:')) {
    currentDiv.className = 'warning'
  } else if (text.includes('OK') || text.includes('READY') || text.includes('100%')) {
    currentDiv.className = 'success'
  } else if (text.includes('████')) {
    currentDiv.innerHTML = '<div class="loading-bar"><div class="loading-progress"><span class="loading-percentage">0%</span></div></div>'
    currentDiv.className = 'hitching'
    bootTextElement.value?.appendChild(currentDiv)
    
    const percentageElement = currentDiv.querySelector('.loading-percentage') as HTMLElement
    let currentPercent = 0
    const percentageInterval = setInterval(() => {
      currentPercent += Math.random() * 8 + 2
      if (currentPercent >= 100) {
        currentPercent = 100
        clearInterval(percentageInterval)
      }
      if (percentageElement) {
        percentageElement.textContent = Math.floor(currentPercent) + '%'
      }
    }, 80)
    
    setTimeout(callback, 3000)
    return
  }
  
  bootTextElement.value?.appendChild(currentDiv)
  
  function type() {
    if (i < text.length) {
      currentDiv.textContent += text.charAt(i)
      i++
      const delay = text.includes('Loading') || text.includes('Scanning') ? 
        50 + Math.random() * 100 : 20 + Math.random() * 40
      setTimeout(type, delay)
    } else {
      setTimeout(callback, 100)
    }
  }
  
  if (text === "") {
    currentDiv.innerHTML = "&nbsp;"
    setTimeout(callback, 50)
  } else {
    type()
  }
}

function bootNextLine() {
  if (currentLine.value < bootSequence.length) {
    typeText(bootSequence[currentLine.value], () => {
      currentLine.value++
      bootNextLine()
    })
  } else {
    setTimeout(() => {
      if (bootTextElement.value) {
        bootTextElement.value.innerHTML = ''
      }
      menuElement.value?.classList.add('active')
      
      const menuLines = document.querySelectorAll('.menu-line')
      menuLines.forEach((line, index) => {
        ;(line as HTMLElement).style.animationDelay = `${index * 0.8}s`
      })
    }, 500)
  }
}

function selectCampaign(campaign: string) {
  selectedCampaign.value = campaign
  if (bootTextElement.value) {
    bootTextElement.value.innerHTML = ''
  }
  menuElement.value?.classList.remove('active')
  
  const campaignDetails: Record<string, string[]> = {
    'VR Dead': [
      "",
      ">>> MISSION SELECTED: VR DEAD <<<",
      "",
      "JOB POSTING:",
      "Immediate opening for VR systems maintenance technician. Competitive pay,",
      "full medical coverage, luxury accommodations in state-of-the-art virtual",
      "reality wellness facility. Minor technical glitch requires on-site",
      "troubleshooting. Contract duration: 72 hours maximum. Apply now!",
    ],
    'Another Bug Hunt': [
      "",
      ">>> MISSION SELECTED: ANOTHER BUG HUNT <<<",
      "",
      "JOB POSTING:",
      "Seeking qualified personnel for routine colony status assessment. Beautiful",
      "terraformed world, excellent climate conditions. Previous inspection team",
      "experienced communication equipment failure - standard technical issue.",
      "Bonus pay for rapid deployment. All expenses paid, return trip guaranteed.",
    ],
    'Gradient Descent': [
      "",
      ">>> MISSION SELECTED: GRADIENT DESCENT <<<",
      "",
      "JOB POSTING:",
      "Confidential survey mission for experienced spelunkers and cave specialists.",
      "Significant depth bonuses available. Unique geological formations require",
      "expert assessment. Previous teams report \"unusual discoveries\" - excellent",
      "research opportunities! Premium hazard pay, NDA required.",
    ],
    'Owe My Soul': [
      "",
      ">>> MISSION SELECTED: OWE MY SOUL TO THE COMPANY STORE <<<",
      "",
      "JOB POSTING:",
      "Tired of juggling multiple creditors? Our exclusive Employee Equity Program",
      "combines debt relief with guaranteed employment! Sign our comprehensive",
      "service agreement and let Monarch handle all your financial worries. New",
      "employee benefits include housing, meals, and lifetime job security!",
    ]
  }
  
  const messages = [
    ...campaignDetails[campaign],
    "",
    "Preparing contractor briefing...",
    "Uploading mission parameters...",
    "Reviewing terms and conditions...",
    "",
    "WARNING: This contract is legally binding.",
    "Monarch Corporation assumes no liability for injury or death.",
    "",
    "<button class='accept-button' onclick='acceptContract()'>[ ACCEPT CONTRACT ]</button>",
    "",
    "<div class='campaign-option' onclick='returnToMenu()' style='display: inline-block;'>[RETURN TO MISSION SELECT]</div>",
    "",
    "<span class='cursor'>█</span>"
  ]
  
  let msgIndex = 0
  function showMessage() {
    if (msgIndex < messages.length) {
      const div = document.createElement('div')
      div.innerHTML = messages[msgIndex]
      if (messages[msgIndex].includes('>>>')) {
        div.className = 'warning'
        div.style.textAlign = 'center'
      }
      if (messages[msgIndex].includes('JOB POSTING:')) {
        div.className = 'success'
        div.style.fontWeight = 'bold'
      }
      if (messages[msgIndex].includes('WARNING:')) {
        div.className = 'error'
        div.style.textAlign = 'center'
      }
      if (messages[msgIndex].includes('ACCEPT CONTRACT') || messages[msgIndex].includes('RETURN TO MISSION SELECT')) {
        div.style.textAlign = 'center'
      }
      bootTextElement.value?.appendChild(div)
      msgIndex++
      setTimeout(showMessage, 800)
    }
  }
  showMessage()
}

function acceptContract() {
  const campaignCodes: Record<string, string> = {
    'VR Dead': 'VRD-7739',
    'Another Bug Hunt': 'ABH-4421', 
    'Gradient Descent': 'GDS-9982',
    'Owe My Soul': 'EEP-1138'
  }
  
  const contractCode = campaignCodes[selectedCampaign.value!] + '-' + Math.floor(Math.random() * 10000).toString().padStart(4, '0')
  
  if (bootTextElement.value) {
    bootTextElement.value.innerHTML = ''
  }
  
  const confirmationMessages = [
    "",
    ">>> CONTRACT ACCEPTED <<<",
    "",
    "Processing legal documentation...",
    "Generating contractor identification...",
    "Finalizing transport arrangements...",
    "",
    "<div class='contract-code'>CONTRACT CONFIRMATION CODE:<br>" + contractCode + "</div>",
    "",
    "IMPORTANT: Copy this code and send it to your GM",
    "This code confirms your mission assignment",
    "",
    "Welcome to Monarch Corporation, contractor.",
    "Your success is our priority.",
    "",
    "<div class='campaign-option' onclick='returnToMenu()' style='display: inline-block;'>[SELECT DIFFERENT MISSION]</div>",
    "",
    "<span class='cursor'>█</span>"
  ]
  
  let msgIndex = 0
  function showConfirmation() {
    if (msgIndex < confirmationMessages.length) {
      const div = document.createElement('div')
      div.innerHTML = confirmationMessages[msgIndex]
      if (confirmationMessages[msgIndex].includes('>>>')) {
        div.className = 'success'
        div.style.textAlign = 'center'
      }
      if (confirmationMessages[msgIndex].includes('CONTRACT CONFIRMATION CODE')) {
        div.style.textAlign = 'center'
      }
      if (confirmationMessages[msgIndex].includes('IMPORTANT:') || confirmationMessages[msgIndex].includes('This code confirms')) {
        div.className = 'warning'
        div.style.textAlign = 'center'
      }
      if (confirmationMessages[msgIndex].includes('Welcome to Monarch') || confirmationMessages[msgIndex].includes('Your success')) {
        div.style.textAlign = 'center'
      }
      if (confirmationMessages[msgIndex].includes('SELECT DIFFERENT MISSION')) {
        div.style.textAlign = 'center'
      }
      bootTextElement.value?.appendChild(div)
      msgIndex++
      setTimeout(showConfirmation, 600)
    }
  }
  showConfirmation()
}

function returnToMenu() {
  if (bootTextElement.value) {
    bootTextElement.value.innerHTML = ''
  }
  menuElement.value?.classList.add('active')
}

function showEasterEgg() {
  if (bootTextElement.value) {
    bootTextElement.value.innerHTML = ''
  }
  menuElement.value?.classList.remove('active')
  
  const easterEggMessages = [
    "",
    ">>> UNAUTHORIZED ACCESS DETECTED <<<",
    "",
    "Tracing connection...",
    "IP: 192.168.CLASSIFIED",
    "Location: [SCRAMBLED]",
    "",
    "Just kidding! You found the easter egg!",
    "",
    "MESSAGE FROM THE MOTHERSHIP GM:",
    "",
    "Congratulations, curious contractor! Your attention to detail",
    "and willingness to click suspicious links shows you have exactly",
    "the kind of reckless curiosity that Monarch Corporation values",
    "in its disposable-- er, VALUED employees.",
    "",
    "As a reward for your digital spelunking, here's a bonus:",
    "",
    "<div class='contract-code'>BONUS CODE: CURIOUS-CAT-2384<br>Share this with your GM for a special reward!</div>",
    "",
    "Remember: In space, curiosity definitely won't kill the cat.",
    "The vacuum will do that first.",
    "",
    "- Management",
    "",
    "<div class='campaign-option' onclick='returnToMenu()' style='display: inline-block;'>[RETURN TO TERMINAL]</div>",
    "",
    "<span class='cursor'>█</span>"
  ]
  
  let msgIndex = 0
  function showEasterEggMessage() {
    if (msgIndex < easterEggMessages.length) {
      const div = document.createElement('div')
      div.innerHTML = easterEggMessages[msgIndex]
      if (easterEggMessages[msgIndex].includes('>>>')) {
        div.className = 'error'
        div.style.textAlign = 'center'
      }
      if (easterEggMessages[msgIndex].includes('Just kidding') || easterEggMessages[msgIndex].includes('MESSAGE FROM')) {
        div.className = 'success'
        div.style.textAlign = 'center'
        div.style.fontWeight = 'bold'
      }
      if (easterEggMessages[msgIndex].includes('BONUS CODE')) {
        div.style.textAlign = 'center'
      }
      if (easterEggMessages[msgIndex].includes('Remember:') || easterEggMessages[msgIndex].includes('The vacuum') || easterEggMessages[msgIndex].includes('- Management')) {
        div.style.textAlign = 'center'
      }
      if (easterEggMessages[msgIndex].includes('RETURN TO TERMINAL')) {
        div.style.textAlign = 'center'
      }
      bootTextElement.value?.appendChild(div)
      msgIndex++
      setTimeout(showEasterEggMessage, 600)
    }
  }
  showEasterEggMessage()
}

// Make functions available globally for onclick handlers
if (process.client) {
  ;(window as any).acceptContract = acceptContract
  ;(window as any).returnToMenu = returnToMenu
  ;(window as any).showEasterEgg = showEasterEgg
}
</script>

<template>
  <div class="terminal">
    <div ref="bootTextElement" id="boot-text"></div>
    <div ref="menuElement" class="menu" id="campaign-menu">
      <div class="logo menu-line">
███╗   ███╗  ██████╗  ███╗   ██╗  █████╗  ██████╗   ██████╗ ██╗  ██╗
████╗ ████║ ██╔═══██╗ ████╗  ██║ ██╔══██╗ ██╔══██╗ ██╔════╝ ██║  ██║
██╔████╔██║ ██║   ██║ ██╔██╗ ██║ ███████║ ██████╔╝ ██║      ███████║
██║╚██╔╝██║ ██║   ██║ ██║╚██╗██║ ██╔══██║ ██╔══██╗ ██║      ██╔══██║
██║ ╚═╝ ██║ ╚██████╔╝ ██║ ╚████║ ██║  ██║ ██║  ██║ ╚██████╗ ██║  ██║
╚═╝     ╚═╝  ╚═════╝  ╚═╝  ╚═══╝ ╚═╝  ╚═╝ ╚═╝  ╚═╝  ╚═════╝ ╚═╝  ╚═╝

"Your Success Is Our Priority"
      </div>
      
      <div class="menu-line" style="margin: 20px 0;">
================================================================================
CONTRACTOR ASSIGNMENT SYSTEM
SELECT MISSION PARAMETERS
================================================================================
      </div>
      
      <div class="campaign-option menu-line" @click="selectCampaign('VR Dead')">
[1] VR-TECH-7739 - Virtual Reality Systems Maintenance
    Location: Station 7 | Duration: 72hrs | Pay: 850 credits/day
      </div>
      
      <div class="campaign-option menu-line" @click="selectCampaign('Another Bug Hunt')">
[2] ABH-4421 - Colonial Site Inspection  
    Location: Samsa VI | Duration: Variable | Bonus pay available
      </div>
      
      <div class="campaign-option menu-line" @click="selectCampaign('Gradient Descent')">
[3] GD-9982 - Deep Space Survey Mission [CLASSIFIED]
    Location: [REDACTED] | Hazard pay: Premium | NDA required
      </div>
      
      <div class="campaign-option menu-line" @click="selectCampaign('Owe My Soul')">
[4] CS-1138 - Employee Equity Program
    Location: Multiple | Duration: Lifetime | Benefits: Comprehensive
      </div>
      
      <div class="menu-line" style="text-align: center; margin: 20px 0; color: #ffff00;">
          Click to select mission assignment
      </div>
      
      <div class="menu-line" style="text-align: center; color: #666;">
          <span class="cursor">█</span>
      </div>
      
      <div class="menu-line" style="margin-top: 40px;">
================================================================================
      </div>
      
      <div class="menu-line" style="font-size: 10px; color: #666; margin: 10px 0;">
MONARCH CORPORATION © 2384 - All Rights Reserved
Unauthorized access punishable by spacing. Corporate Charter #MC-7739-DEEP
Licensed by Terran Colonial Authority. "Your Success Is Our Priority" ™
      </div>
      
      <div class="menu-line" style="font-size: 8px; color: #333; margin-top: 10px;">
For technical support, contact: <span @click="showEasterEgg()" style="cursor: pointer; text-decoration: underline;">sysadmin@monarch.corp</span>
System ID: MCT-2384-7739 | Terminal Auth: GUEST-ACCESS | Uptime: 847 days
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Courier+Prime:wght@400;700&display=swap');

body {
    background-color: #000;
    color: #00ff00;
    font-family: 'Courier Prime', monospace;
    font-size: 14px;
    line-height: 1.4;
    margin: 0;
    padding: 20px;
    min-height: 100vh;
    overflow-y: auto;
}

.terminal {
    max-width: 80ch;
    margin: 0 auto;
    white-space: pre;
    background-color: #000;
    color: #00ff00;
    font-family: 'Courier Prime', monospace;
    font-size: 14px;
    line-height: 1.4;
    padding: 20px;
    min-height: 100vh;
    overflow-y: auto;
}

.boot-sequence {
    display: none;
}

.boot-sequence.active {
    display: block;
    animation: typewriter 0.05s steps(1, end);
}

.menu {
    display: none;
    margin-top: 20px;
}

.menu.active {
    display: block;
}

.menu-line {
    opacity: 0;
    animation: fadeInLine 0.5s ease-in forwards;
}

.campaign-option {
    margin: 5px 0;
    padding: 5px 10px;
    cursor: pointer;
    transition: all 0.2s;
}

.campaign-option:hover {
    background-color: #00ff00;
    color: #000;
}

:deep(.accept-button) {
    background-color: #003300;
    border: 2px solid #00ff00;
    color: #00ff00;
    padding: 10px 20px;
    margin: 10px 0;
    cursor: pointer;
    font-family: 'Courier Prime', monospace;
    font-size: 14px;
    transition: all 0.2s;
}

:deep(.accept-button:hover) {
    background-color: #00ff00;
    color: #000;
}

:deep(.contract-code) {
    background-color: #001100;
    border: 1px solid #00ff00;
    padding: 15px;
    margin: 20px 0;
    text-align: center;
    font-weight: bold;
}

.cursor {
    animation: blink 1s infinite;
}

.logo {
    color: #00ff00;
    font-weight: bold;
    text-align: center;
    margin-bottom: 20px;
}

:deep(.error) {
    color: #ff0000;
}

:deep(.warning) {
    color: #ffff00;
}

:deep(.success) {
    color: #00ff00;
}

@keyframes blink {
    0%, 50% { opacity: 1; }
    51%, 100% { opacity: 0; }
}

@keyframes typewriter {
    from { width: 0; }
    to { width: 100%; }
}

@keyframes fadeInLine {
    to { opacity: 1; }
}

:deep(.loading-bar) {
    width: 50ch;
    height: 2px;
    background-color: #333;
    margin: 10px 0;
    position: relative;
}

:deep(.loading-progress) {
    height: 100%;
    background-color: #00ff00;
    width: 0%;
    animation: loading 3s ease-in-out forwards;
    position: relative;
}

:deep(.loading-percentage) {
    position: absolute;
    right: -60px;
    top: -8px;
    color: #00ff00;
    font-size: 14px;
}

@keyframes loading {
    to { width: 100%; }
}

@keyframes hitch {
    0% { opacity: 1; }
    20% { opacity: 0.3; }
    40% { opacity: 1; }
    60% { opacity: 0.5; }
    80% { opacity: 1; }
    100% { opacity: 1; }
}

:deep(.hitching) {
    animation: hitch 2s ease-in-out;
}
</style>

<script setup>
const linkGroups = [
  {
    title: 'Official',
    description: 'Account tools, news, and official resources.',
    links: [
      { label: 'World of Warcraft', href: 'https://worldofwarcraft.blizzard.com/' },
      { label: 'Battle.net Launcher', href: 'https://www.blizzard.com/apps/battle.net/desktop' },
      { label: 'WoW News', href: 'https://worldofwarcraft.blizzard.com/en-us/news' }
    ]
  },
  {
    title: 'Databases & Guides',
    description: 'Items, quests, rotations, and class guides.',
    links: [
      { label: 'Wowhead', href: 'https://www.wowhead.com/' },
      { label: 'Icy Veins', href: 'https://www.icy-veins.com/wow/' },
      { label: 'Method.gg', href: 'https://www.method.gg/' }
    ]
  },
  {
    title: 'Raid & Mythic+',
    description: 'Logs, analysis, and best-in-slot tooling.',
    links: [
      { label: 'Warcraft Logs', href: 'https://www.warcraftlogs.com/' },
      { label: 'Raidbots', href: 'https://www.raidbots.com/' },
      { label: 'Mythic+ Rankings', href: 'https://raider.io/' }
    ]
  },
  {
    title: 'Addons & UI',
    description: 'Curate and customize your interface.',
    links: [
      { label: 'CurseForge Addons', href: 'https://www.curseforge.com/wow/addons' },
      { label: 'Wago.io', href: 'https://wago.io/' },
      { label: 'WeakAuras', href: 'https://www.curseforge.com/wow/addons/weakauras-2' }
    ]
  },
  {
    title: 'Class Builds',
    description: 'Talent builds and top player loadouts.',
    links: [
      { label: 'Murlok.io', href: 'https://murlok.io/' },
      { label: 'Subcreation', href: 'https://subcreation.net/' },
      { label: 'WowAnalyzer', href: 'https://wowanalyzer.com/' }
    ]
  }
]
</script>

<template>
  <div class="page">
    <header class="hero">
      <div class="hero-badge">Atlas of Azeroth</div>
      <h1>World of Warcraft Link Hub</h1>
      <p>
        A fast, focused launchpad for raids, logs, guides, and tools.
      </p>
      <div class="hero-actions">
        <a class="primary" href="https://worldofwarcraft.blizzard.com/" target="_blank" rel="noreferrer">
          Launch WoW
        </a>
        <a class="ghost" href="https://raider.io/" target="_blank" rel="noreferrer">
          Check Raider.IO
        </a>
      </div>
    </header>

    <section class="grid" aria-label="World of Warcraft resources">
      <article v-for="group in linkGroups" :key="group.title" class="card">
        <div class="card-header">
          <h2>{{ group.title }}</h2>
          <p>{{ group.description }}</p>
        </div>
        <ul class="link-list">
          <li v-for="link in group.links" :key="link.href">
            <a :href="link.href" target="_blank" rel="noreferrer">
              {{ link.label }}
              <span aria-hidden="true">↗</span>
            </a>
          </li>
        </ul>
      </article>
    </section>
  </div>
</template>




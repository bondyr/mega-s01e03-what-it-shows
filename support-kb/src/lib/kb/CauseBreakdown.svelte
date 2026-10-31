<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { groupByCause, readAs, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  const merged = uniqueById(reports as Report[]).filter((report) => report.mergedInto === 4471);
  const groups = groupByCause(merged, tenantOf);
  const channels = (items: Report[]) => {
    const counts = items.reduce<Record<string, number>>((all, report) => {
      all[report.channel] = (all[report.channel] ?? 0) + 1;
      return all;
    }, {});
    return Object.entries(counts).map(([name, count]) => `${name} ×${count}`).join(' · ');
  };
  const tags = (items: Report[]) => {
    const counts = items.reduce<Record<string, number>>((all, report) => {
      all[report.meta.intakeTag] = (all[report.meta.intakeTag] ?? 0) + 1;
      return all;
    }, {});
    return Object.entries(counts).sort((a, b) => a[0].localeCompare(b[0]))
      .map(([name, count]) => `${name} ×${count}`).join(' · ');
  };
</script>

<p class="sum"><strong>{merged.length}</strong> unique reports in #4471 · <strong>{groups.length}</strong> causes</p>

<table>
  <thead>
    <tr><th scope="col">Cause</th><th scope="col">Reports</th><th scope="col">Tenants</th><th scope="col">Channels</th><th scope="col">Intake tags</th><th scope="col">Period</th></tr>
  </thead>
  <tbody>
    {#each groups as group (group.cause)}
      {@const dates = group.reports.map((report) => report.receivedAt.slice(0, 10)).sort()}
      {@const oneDayMoves = readAs(group, notes.oneDayMove)}
      <tr>
        <th scope="row"><code>{group.cause}</code></th>
        <td>{group.reports.length}</td>
        <td>{group.tenants}</td>
        <td>{channels(group.reports)}</td>
        <td>{tags(group.reports)}</td>
        <td>{dates[0]} → {dates[dates.length - 1]}</td>
      </tr>
      {#if oneDayMoves}
        <tr class="note"><td colspan="6">{oneDayMoves} one-day-move reports are counted under <code>{group.cause}</code> by tenant and form.</td></tr>
      {/if}
    {/each}
  </tbody>
</table>

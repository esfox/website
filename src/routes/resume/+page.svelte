<script>
  import PromptString from '../../components/PromptString.svelte';
  import json from '../../data/info.json';

  const {
    fullname,
    shortenedFullName,
    contact,
    experience,
    skills,
    otherSkills,
    softSkills,
    education,
  } = json;

  function downloadPDF() {
    window.print();
  }
</script>

<svelte:head>
  <title>Resume | {shortenedFullName}</title>
</svelte:head>

<PromptString />
<p class="command indent-after-prompt">cat resume.html</p>
<div class="pdf-download-container">
  <button class="pdf-download-btn" on:click={downloadPDF}>
    <i class="fa-solid fa-file-pdf"></i>
    Download as PDF
  </button>
</div>
<div class="resume-container" id="resume-content">
  <header class="resume-header">
    <h1>{fullname}</h1>
    <div class="contact-info">
      <div><i class="fa-solid fa-house"></i> {contact.address}</div>
      <div><i class="fa-solid fa-mobile-screen"></i> {contact.phone}</div>
      <div>
        <i class="fa-solid fa-envelope"></i>
        <a href="mailto:{contact.email}" target="_blank">{contact.email}</a>
      </div>
      <div>
        <i class="fa-brands fa-linkedin"></i>
        <a href={contact.linkedIn} target="_blank">{contact.linkedIn}</a>
      </div>
      <div>
        <i class="fa-brands fa-github"></i>
        <a href={contact.github} target="_blank">{contact.github}</a>
      </div>
    </div>
  </header>

  <div class="recommendation">
    <strong
      >I strongly recommend to check my GitHub profile linked above to see how I write code.</strong
    >
  </div>

  <section class="resume-section">
    <h2><span class="section-icon">💼</span> Experience</h2>
    {#each experience as job}
      <div class="experience-item">
        <h3 class="role">{job.role}</h3>
        <div class="company-info">
          <span class="company">{job.place}</span>
          <span class="period">{job.timePeriod}</span>
        </div>
        <ul class="job-details">
          {#each job.details as detail}
            <li
              class={detail.startsWith('  ') ? 'sub-item' : ''}
              class:sub-project={detail.startsWith('AI-') ||
                detail.startsWith('Higher') ||
                detail.startsWith('Logistics') ||
                detail.startsWith('Property') ||
                detail.startsWith('College') ||
                detail.startsWith('Company') ||
                detail.startsWith('End-of-life') ||
                detail.startsWith('Online') ||
                detail.startsWith('Mobile')}
            >
              {detail.replace(/^\s{2}/, '')}
            </li>
          {/each}
        </ul>
        {#if job !== experience[experience.length - 1]}
          <hr />
        {/if}
      </div>
    {/each}
  </section>

  <section class="resume-section">
    <h2><span class="section-icon">🛠️</span> Skills</h2>
    {#each skills as skillCategory}
      <div class="skill-category">
        <h3>{skillCategory.name}</h3>
        {#each skillCategory.categories as category}
          <div class="skill-group">
            <span class="skill-category-name">{category.name}:</span>
            <span class="skill-items">{category.items.join(', ')}</span>
          </div>
        {/each}
        <hr />
      </div>
    {/each}

    <div class="skill-category">
      <h3>Other Skills</h3>
      <ul>
        {#each otherSkills as skill}
          <li>{@html skill}</li>
        {/each}
      </ul>
      <hr />
    </div>

    <div class="skill-category">
      <h3>Soft Skills</h3>
      <ul>
        {#each softSkills as skill}
          <li>{skill}</li>
        {/each}
      </ul>
    </div>
  </section>

  <section class="resume-section">
    <h2><span class="section-icon">🎓</span> Education</h2>
    <ul>
      {#each education as edu}
        <li>
          <h3 class="role">{edu.major}</h3>
          <div class="company-info">
            <span class="company">{edu.school}</span>
            <span class="period">{edu.timePeriod}</span>
          </div>
        </li>
      {/each}
    </ul>
  </section>

  <div class="footer-note">
    <div>* References available upon request</div>
    <div class="resume-credit">
      <em
        ><small
          >This resume is a webpage created with Svelte by yours truly then saved as a PDF.</small
        ></em
      >
    </div>
  </div>
</div>

<style lang="scss">
  :global(h2) {
    margin-bottom: 4px !important;
  }

  .command {
    margin-top: -18px;
  }

  .resume-container {
    max-width: 100%;
    padding: 20px;
    font-family: 'JetBrains Mono', monospace;
    line-height: 1.6;
    color: var(--foreground);
  }

  .resume-header {
    margin-bottom: 20px;
    padding-bottom: 20px;

    h1 {
      margin: 0 0 15px 0;
      font-size: 1.8em;
      color: var(--green);
    }

    .contact-info {
      display: grid;
      gap: 4px;

      div {
        display: flex;
        align-items: center;
        gap: 10px;

        i {
          width: 20px;
          color: var(--yellow);
        }

        a {
          color: var(--blue);
          text-decoration: none;

          &:hover {
            text-decoration: underline;
          }
        }
      }
    }
  }

  .recommendation {
    text-align: center;
    margin: 8px 0;
    padding: 15px;
    background: var(--black);
    border-radius: 4px;
    font-size: 0.9em;
  }

  .resume-section {
    margin: 30px 0;

    h2 {
      margin: 0 0 20px 0;
      font-size: 1.4em;
      color: var(--yellow);
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .section-icon {
      font-size: 1.2em;
    }

    h3 {
      margin: 0 0 5px 0;
      font-size: 1.1em;
      color: var(--green);
    }

    .company-info {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      font-size: 0.9em;

      .company {
        font-style: italic;
        color: var(--foreground);
      }

      .period {
        color: var(--blue);
      }
    }

    .job-details {
      margin: 10px 0;
      padding-left: 20px;

      li {
        margin-bottom: 5px;

        &.sub-item {
          margin-left: 20px;
          list-style-type: circle;
        }

        &.sub-project {
          margin-left: 20px;
          list-style-type: square;
          color: var(--yellow);
        }
      }
    }
  }

  .skill-category {
    margin: 20px 0;

    .skill-group {
      display: grid;
      grid-template-columns: auto 1fr;
      gap: 10px;
      margin-bottom: 8px;
      font-size: 0.9em;

      .skill-category-name {
        color: var(--blue);
        font-weight: bold;
      }

      .skill-items {
        color: var(--foreground);
      }
    }

    ul {
      margin: 10px 0;
      padding-left: 20px;

      li {
        margin-bottom: 5px;
      }
    }
  }

  .footer-note {
    margin-top: 40px;
    padding-top: 20px;
    text-align: center;
    font-size: 0.9em;

    .resume-credit {
      margin-top: 10px;
      color: var(--divider);
    }
  }

  .pdf-download-container {
    margin: 20px 0;
    text-align: center;
  }

  .pdf-download-btn {
    background: var(--blue);
    color: white;
    border: none;
    padding: 12px 24px;
    border-radius: 6px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9em;
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    transition: background-color 0.2s;

    &:hover {
      background: var(--green);
    }

    i {
      font-size: 1.1em;
    }
  }

  @media (max-width: 768px) {
    .resume-container {
      padding: 15px;
    }

    .company-info {
      flex-direction: column;
      align-items: flex-start;
      gap: 5px;
    }

    .skill-group {
      grid-template-columns: 1fr;
      gap: 5px;
    }
  }

  @media print {
    body {
      background: white !important;
      color: black !important;
      font-family: 'JetBrains Mono', monospace;
    }

    .pdf-download-container,
    .command,
    .recommendation,
    .resume-credit {
      display: none !important;
    }

    #resume-content {
      max-width: 100%;
      margin: 0;
      padding: 40px;
      background: white !important;
      color: black !important;
      font-size: 12pt;
      line-height: 1.4;
    }

    .resume-header {
      border-bottom: 2px solid black !important;
      padding-bottom: 20px;
      margin-bottom: 30px;

      h1 {
        color: black !important;
        font-size: 24pt;
        margin-bottom: 20px;
        text-align: center;
      }

      .contact-info {
        display: block;
        text-align: center;
        font-size: 11pt;

        div {
          display: block;
          margin: 4px 0;
          justify-content: center;

          i {
            color: black !important;
            margin-right: 8px;
          }

          a {
            color: black !important;
            text-decoration: none;
          }
        }
      }
    }

    .resume-section {
      margin: 25px 0;
      page-break-inside: avoid;

      h2 {
        color: black !important;
        font-size: 16pt;
        border-bottom: 1px solid black;
        padding-bottom: 5px;
        margin-bottom: 15px;

        .section-icon {
          display: none;
        }
      }

      h3 {
        color: black !important;
        font-size: 14pt;
        margin-bottom: 3px;
      }

      .company-info {
        .company {
          color: black !important;
          font-style: italic;
        }

        .period {
          color: black !important;
          font-weight: normal;
        }
      }

      .job-details {
        li {
          color: black !important;
          margin-bottom: 6px;

          &.sub-project {
            color: black !important;
          }
        }
      }
    }

    .skill-category {
      margin: 20px 0;
      page-break-inside: avoid;

      .skill-group {
        .skill-category-name {
          color: black !important;
          font-weight: bold;
        }

        .skill-items {
          color: black !important;
        }
      }

      ul li {
        color: black !important;
      }
    }

    .footer-note {
      margin-top: 30px;
      padding-top: 15px;
      border-top: 1px solid black;
      text-align: center;
      color: black !important;
      font-size: 10pt;
    }

    hr {
      border-color: black !important;
      margin: 15px 0;
    }

    @page {
      margin: 0.5in;
      size: letter;
    }
  }
</style>

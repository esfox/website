<script>
  import PromptString from '../../components/PromptString.svelte';
  import json from '../../data/info.json';

  const {
    fullname,
    shortenedFullName,
    contact,
    experience,
    works,
    skills,
    otherSkills,
    softSkills,
    education,
  } = json;

  function openPrintableResume() {
    window.open('/resume/print', '_blank', 'noopener');
  }
</script>

<svelte:head>
  <title>Resume | {shortenedFullName}</title>
</svelte:head>

<PromptString />
<div class="command heading">
  <p class="indent-after-prompt">cat resume.html</p>
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

  <hr />

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
            <li class={detail.startsWith('  ') ? 'sub-item' : ''}>
              {detail.replace(/^\s{2}/, '')}
            </li>
          {/each}
        </ul>
      </div>
    {/each}
  </section>

  <hr />

  <section class="resume-section">
    <h2><span class="section-icon">📂</span> Works</h2>
    {#each works as work}
      <div class="experience-item">
        <h3 class="role">{work.company}</h3>
        <ul class="job-details">
          {#each work.projects as project}
            <li>{project.description}</li>
            {#if project.details}
              {#each project.details as detail}
                <li class="sub-item">
                  {detail.replace(/^\s{2}/, '')}
                </li>
              {/each}
            {/if}
          {/each}
        </ul>
      </div>
    {/each}
  </section>

  <hr />

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
      </div>
    {/each}

    <div class="skill-category">
      <h3>Other Skills</h3>
      <ul>
        {#each otherSkills as skill}
          <li>{skill}</li>
        {/each}
      </ul>
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

  <hr />

  <section class="resume-section">
    <h2><span class="section-icon">🎓</span> Education</h2>
    <ul>
      {#each education as edu}
        <li>
          <h3 class="role">{edu.major}</h3>
          <div class="company-info">
            <span class="company">
              {edu.school}
            </span>
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

<div class="pdf-download-button-wrapper">
  <button class="pdf-download-button" type="button" on:click={openPrintableResume}>
    <i class="fa-solid fa-file-pdf"></i>
    Download PDF
  </button>
</div>

<style lang="scss">
  .heading {
    display: flex;
    justify-content: space-between;
    padding-inline-end: 12px;
  }

  .command {
    margin-top: -18px;
  }

  .resume-container {
    max-width: 100%;
    padding-inline: 20px;
    margin-block-start: 8px;
    line-height: 1.6;
  }

  .resume-header {
    background: var(--black);
    margin-top: 24px;
    margin-bottom: 16px;
    padding: 16px;
    padding-top: 6px;

    h1 {
      margin-block-end: 10px;
      font-size: 1.8rem;
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
          text-align: center;
        }

        a {
          text-decoration: none;

          &:hover {
            text-decoration: underline;
          }
        }
      }
    }
  }

  .resume-section {
    margin-top: 8px;

    h2 {
      font-size: 1.4rem;
      color: var(--yellow);
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 6px;
    }

    .section-icon {
      font-size: 1.2rem;
    }

    h3 {
      margin: 0 0 5px 0;
      font-size: 1.1rem;
      color: var(--green);
    }

    .company-info {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      font-size: 1rem;

      .company {
        font-style: italic;
        font-weight: bold;
        color: var(--foreground);
      }

      .period {
        color: var(--blue);
        font-size: 0.8rem;
      }
    }

    .job-details {
      padding-left: 24px;
      margin-bottom: 32px;

      li {
        margin-bottom: 5px;

        &.sub-item {
          margin-left: 20px;
          color: var(--foreground-muted);
        }
      }
    }
  }

  .skill-category {
    margin-top: 6px;
    margin-bottom: 28px;

    .skill-group {
      display: grid;
      grid-template-columns: auto 1fr;
      gap: 10px;
      margin-bottom: 6px;
      font-size: 0.9rem;

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
    margin-bottom: 20px;
    color: var(--divider);
    font-style: italic;
    text-align: center;
    font-size: 0.9rem;

    .resume-credit {
      display: none;
    }
  }

  .pdf-download-button-wrapper {
    text-align: end;
    padding: 28px;
    padding-top: 12px;

    .pdf-download-button {
      background: var(--green);
      border: none;
      padding: 9px 12px;
      border-radius: 6px;
      font-size: 0.8rem;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 8px;
      color: #08120c;
      z-index: 10;
      letter-spacing: -0.04rem;

      &:hover {
        background: color-mix(in srgb, var(--green) 70%, transparent);
      }

      i {
        font-size: 1.1rem;
      }
    }
  }

  @media (max-width: 768px) {
    .resume-container {
      padding: 15px;
    }

    .resume-header {
      h1 {
        font-size: 1.25rem;
      }
    }

    .company-info {
      display: grid !important;
      gap: 5px;
    }

    .skill-group {
      grid-template-columns: 1fr;
      gap: 5px;
    }
  }

  @media print {
    :global(body) {
      background: white !important;
      color: black !important;
      font-family: 'JetBrains Mono', monospace;
    }

    .command,
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

          &.sub-item {
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

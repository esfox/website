<script>
  import { onMount } from 'svelte';
  import json from '../../../data/info.json';

  const { fullname, contact, experience, works, skills, otherSkills, softSkills, education } = json;

  onMount(() => {
    let hasAttemptedPrint = false;

    const exitPrint = () => {
      window.close();
      window.setTimeout(() => {
        if (!window.closed) {
          window.location.assign('/resume');
        }
      }, 100);
    };

    const handleAfterPrint = () => {
      exitPrint();
    };

    const handleVisibilityChange = () => {
      if (hasAttemptedPrint && document.visibilityState === 'visible') {
        exitPrint();
      }
    };

    const handleFocus = () => {
      if (hasAttemptedPrint) {
        exitPrint();
      }
    };

    window.addEventListener('afterprint', handleAfterPrint);
    document.addEventListener('visibilitychange', handleVisibilityChange);
    window.addEventListener('focus', handleFocus);

    const printTimer = window.setTimeout(() => {
      hasAttemptedPrint = true;
      window.print();
    }, 50);

    return () => {
      window.removeEventListener('afterprint', handleAfterPrint);
      document.removeEventListener('visibilitychange', handleVisibilityChange);
      window.removeEventListener('focus', handleFocus);
      window.clearTimeout(printTimer);
    };
  });
</script>

<svelte:head>
  <title>charles-sabarillo-resume</title>
</svelte:head>

<div class="print-resume">
  <header class="resume-header">
    <h1>{fullname}</h1>
    <div class="contact-info">
      <div><i class="fa-solid fa-house"></i> {contact.address}</div>
      <div><i class="fa-solid fa-mobile-screen"></i> {contact.phone}</div>
      <div>
        <i class="fa-solid fa-envelope"></i>
        <a href="mailto:{contact.email}" target="_blank" rel="noreferrer">{contact.email}</a>
      </div>
      <div>
        <i class="fa-brands fa-linkedin"></i>
        <a href={contact.linkedIn} target="_blank" rel="noreferrer">{contact.linkedIn}</a>
      </div>
      <div>
        <i class="fa-brands fa-github"></i>
        <a href={contact.github} target="_blank" rel="noreferrer">{contact.github}</a>
      </div>
    </div>
  </header>

  <hr />

  <section class="resume-section">
    <h2>Experience</h2>
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
              class:sub-item={detail.startsWith('AI-') ||
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
      </div>
    {/each}
  </section>

  <hr />

  <section class="resume-section">
    <h2>Works</h2>
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
    <h2>Skills</h2>
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
    <h2>Education</h2>
    <ul class="education-list">
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

  <hr />

  <div class="footer-note">
    <div>* References available upon request</div>
    <div class="credit">
      <p>This resume is a webpage created with Svelte by yours truly then saved as a PDF.</p>
      <p>Web version can be viewed at https://ckftm.pages.dev/resume</p>
    </div>
  </div>
</div>

<style lang="scss">
  .print-resume {
    --foreground: #111111;
    --foreground-muted: #555555;
    width: min(8.5in, 100%);
    color: var(--foreground);
    line-height: 1.5;
    margin: 0 auto;
    padding-top: 36px;
    padding-bottom: 36px;

    hr {
      margin-top: 18px;
      margin-bottom: 18px;
      border: none;
      border-top: 1px solid var(--foreground-muted);
    }

    .resume-header {
      h1 {
        margin: 0 0 12px;
        font-size: 2rem;
        letter-spacing: -0.04em;
      }

      .contact-info {
        display: grid;
        gap: 6px;
        font-size: 0.95rem;

        div {
          display: flex;
          align-items: center;
          gap: 10px;
        }

        i {
          width: 18px;
          color: var(--foreground-muted);
          text-align: center;
        }

        a {
          color: inherit;
          text-decoration: none;
        }
      }
    }

    .resume-section {
      h2 {
        font-size: 1.4rem;
        font-weight: 800;
        text-decoration: underline;
        text-transform: uppercase;
      }
    }

    .experience-item + .experience-item {
      margin-top: 18px;
    }

    .role {
      margin-top: 12px;
      margin-bottom: 6px;
      font-weight: 700;
    }

    .company-info {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      font-size: 0.95rem;
      margin-bottom: 8px;

      .company {
        font-style: italic;
        font-weight: 600;
      }

      .period {
        white-space: nowrap;
        color: var(--foreground-muted);
      }
    }

    .job-details {
      padding-left: 20px;
      font-weight: 500;

      li {
        margin-bottom: 6px;
      }

      .sub-item {
        margin-left: 18px;
        color: var(--foreground-muted);
      }
    }

    .skill-category {
      margin-top: 16px;

      &:not(:first-of-type) {
        margin-top: 24px;
      }

      .skill-group {
        display: grid;
        grid-template-columns: auto 1fr;
        gap: 10px;
        margin-bottom: 8px;
        font-size: 0.95rem;

        &:first-of-type {
          margin-top: 12px;
        }

        .skill-items {
          font-weight: 500;
        }
      }

      .skill-category-name {
        color: var(--foreground-muted);
      }

      ul {
        margin: 8px 0 0;
        padding-left: 18px;
      }
    }

    .education-list {
      padding-left: 18px;
    }

    .footer-note {
      margin-top: 30px;
      padding-top: 16px;
      text-align: center;
      font-size: 0.9rem;
      color: #555555;

      .credit {
        margin-top: 20px;
        font-size: 0.7rem;
        text-align: center;
      }
    }
  }

  @media print {
    :global(body) {
      -webkit-print-color-adjust: exact !important;
      print-color-adjust: exact !important;
    }

    .print-resume {
      max-width: none;
      width: 100%;
      min-height: auto;
      margin: 0;
      -webkit-print-color-adjust: exact !important;
      print-color-adjust: exact !important;
      padding-top: 0;
      padding-bottom: 0;
    }

    @page {
      margin: 0.5in;
    }

    * {
      -webkit-print-color-adjust: exact !important;
      print-color-adjust: exact !important;
    }
  }
</style>

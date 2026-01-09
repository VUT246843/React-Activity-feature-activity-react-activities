React Activity Feature – Asynchronous Activities

The <Activity /> component in React provides a robust way to manage asynchronous activities in your application. Instead of merely conditionally rendering UI, Activity allows you to schedule, prioritize, and run background activities, giving you fine-grained control over how tasks execute in parallel with your components.

Basic Usage

You can use <Activity> to wrap tasks that need to run asynchronously, such as data fetching, computations, or animations. The component ensures that activities do not block the main render thread.

<Activity mode="async">
  {async () => {
    const data = await fetch('/api/data');
    console.log('Data loaded', data);
  }}
</Activity>


In this example, the activity runs the async function as a scheduled background task, allowing the rest of the UI to remain fully interactive. The mode prop can control the priority and lifecycle of the activity.

Modes

<Activity /> supports multiple modes for task management:

async: Runs the children as asynchronous tasks without blocking rendering.

scheduled: Queues the activity and executes it when the React scheduler determines resources are available.

priority: Marks an activity as high-priority for execution before other background tasks.

background: Runs non-critical activities when the main thread is idle.

<Activity mode="scheduled">
  {async () => {
    await new Promise(res => setTimeout(res, 500));
    console.log('Scheduled activity completed');
  }}
</Activity>

Advanced Usage
Nested Activities

You can nest activities to run multiple tasks in parallel or sequence. React ensures that parent activities can manage child activity states, handle cancellations, and prioritize execution efficiently.

<Activity mode="priority">
  {async () => {
    await fetch('/api/user');
    await fetch('/api/settings');
    console.log('User and settings loaded');
  }}
  <Activity mode="background">
    {async () => {
      console.log('Logging background analytics');
    }}
  </Activity>
</Activity>

Reusable Activities

Activities can be reused across components. By assigning an identifier to an Activity, React can pause, resume, or prioritize tasks across renders.

<Activity id="data-fetcher" mode="async">
  {async () => {
    const result = await fetch('/api/items');
    console.log('Items fetched:', result);
  }}
</Activity>

Patterns for Async Activity Management

Preload data in background: Wrap upcoming views in Activity to fetch data in advance.

Run long computations: Offload expensive computations without blocking the UI.

Handle chained async tasks: Nest Activities to run dependent tasks sequentially.

Manage side effects safely: Ensure cleanup is handled when Activities unmount.

<Activity mode="background">
  {async () => {
    const images = await fetch('/api/images');
    console.log('Images preloaded', images);
  }}
</Activity>

Activity Lifecycle Hooks

While <Activity /> does not directly expose React hooks, you can integrate it with state and effect hooks to manage activity states:

const [status, setStatus] = useState('pending');

<Activity mode="async">
  {async () => {
    setStatus('running');
    await new Promise(res => setTimeout(res, 1000));
    setStatus('completed');
  }}
</Activity>


This allows you to track activity progress, update the UI, and trigger dependent tasks.

Repeated Examples for Common Use Cases
<Activity mode="scheduled">
{async () => console.log('Fetching user profile asynchronously')}
</Activity>

<Activity mode="background">
  {async () => console.log('Preloading images in background')}
</Activity>

<Activity mode="priority">
  {async () => console.log('High-priority computation')}
</Activity>

<Activity mode="async">
  {async () => console.log('Logging analytics event')}
</Activity>


These patterns can be repeated across your app to manage multiple async tasks seamlessly.

React <Activity /> Feature – Managing Async Activities

The <Activity /> component is designed to provide structured asynchronous task management within React applications. Unlike standard conditional rendering, Activity wraps tasks and processes them independently, allowing developers to control execution, prioritize, and maintain performance across the app.

Preloading Views for Faster Navigation

You can use <Activity /> to preload components or views that users are likely to navigate to next. This ensures that data, images, and state are ready instantly when the user switches screens.

<Activity mode="background">
  {async () => {
    const nextPageData = await fetch('/api/nextPage');
    console.log('Preloaded next page data', nextPageData);
  }}
</Activity>

<Activity mode="background">
  {async () => {
    const images = await fetch('/api/gallery');
    console.log('Images preloaded in background');
  }}
</Activity>

Managing Long-running Tasks

Activity is ideal for long-running computations without blocking the UI. For example, calculating analytics metrics or processing large datasets can be wrapped in <Activity />.

<Activity mode="priority">
  {async () => {
    const result = await heavyAnalyticsCalculation();
    console.log('Analytics complete', result);
  }}
</Activity>

Dynamic Form Management

When building complex forms, Activity can keep inactive parts mounted and run validations or auto-saves in the background.

<Activity mode="async">
  {async () => {
    await autoSaveForm('user-profile');
    console.log('Form auto-saved asynchronously');
  }}
</Activity>

<Activity mode="scheduled">
  {async () => {
    await validateFields(['email', 'password', 'username']);
    console.log('Background field validation complete');
  }}
</Activity>

Nested Activity Patterns

You can nest multiple activities to orchestrate dependent tasks or parallelize background processes.

<Activity mode="priority">
  {async () => {
    await fetchUserSettings();
    console.log('User settings loaded');
  }}
  <Activity mode="background">
    {async () => {
      await preloadDashboardWidgets();
      console.log('Dashboard widgets preloaded');
    }}
  </Activity>
</Activity>


Nested activities ensure high-priority tasks execute immediately, while low-priority or background tasks are scheduled when resources are free.

Dashboard Preloading Example

For dashboards with multiple panels, you can preload each panel’s data without affecting the initial render:

<Activity mode="background">
  {async () => fetch('/api/panel1').then(res => res.json())}
</Activity>

<Activity mode="background">
  {async () => fetch('/api/panel2').then(res => res.json())}
</Activity>

<Activity mode="background">
  {async () => fetch('/api/panel3').then(res => res.json())}
</Activity>


Each activity runs independently, ensuring the dashboard loads fast, even with heavy data fetching.

Image Gallery Preloading

For image-heavy applications, Activity can preload images offscreen so that galleries render instantly.

<Activity mode="background">
  {async () => {
    const galleryImages = await fetch('/api/images');
    galleryImages.forEach(img => new Image().src = img.url);
    console.log('Images preloaded');
  }}
</Activity>

Async State Synchronization

Activity can be used to sync local and remote state in the background, keeping forms or settings up to date.

<Activity mode="async">
  {async () => {
    const remoteSettings = await fetch('/api/settings');
    updateLocalState(remoteSettings);
    console.log('Settings synchronized');
  }}
</Activity>

Repeated Patterns for Large Apps

You can repeat Activity usage throughout large projects:

<Activity mode="async">
  {async () => console.log('Background logging task')}
</Activity>

<Activity mode="background">
  {async () => console.log('Preloading sidebar menus')}
</Activity>

<Activity mode="priority">
  {async () => console.log('Critical computation for UI')}
</Activity>

<Activity mode="scheduled">
  {async () => console.log('Deferred updates for low-priority data')}
</Activity>


These patterns can be scaled across hundreds of components to maintain responsiveness, precompute data, and manage async workflows efficiently.

Advanced Use Cases

Lazy-loading analytics dashboards while keeping user navigation instant.

Background syncing of collaborative documents in real-time.

Preprocessing large datasets without blocking UI updates.

Async image or media processing for dynamic galleries or editors.

Task chaining and dependency orchestration with nested activities.

<Activity mode="priority">
  {async () => {
    await loadUserDashboard();
  }}
  <Activity mode="background">
    {async () => {
      await preloadAnalytics();
    }}
  </Activity>
</Activity>

New React Features: <Activity />

React 19.2 introduces <Activity />, a transformative component that lets developers break their apps into independent activities. These activities can be scheduled, prioritized, and executed asynchronously, providing unprecedented control over performance, responsiveness, and workflow orchestration. <Activity> is designed to handle background tasks, preloading, analytics, form autosave, and media processing without blocking the main render thread.

How <Activity> Works

Each <Activity> wraps a function or component, running it according to the mode specified. The modes include:

async: Run tasks asynchronously in the background, freeing the main thread.

priority: Force tasks to run immediately with high importance.

scheduled: Queue tasks until resources are available.

background: Execute non-critical tasks when the main thread is idle.

<Activity mode="async">
  {async () => {
    const data = await fetch('/api/dashboard');
    console.log('Dashboard data loaded asynchronously');
  }}
</Activity>

<Activity mode="priority">
  {async () => {
    await processUserInput();
    console.log('High-priority user input processed');
  }}
</Activity>


Nested activities allow multiple tasks to run concurrently or sequentially, creating complex workflows without blocking UI updates.

<Activity mode="priority">
  {async () => await fetch('/api/user')}
  <Activity mode="background">
    {async () => await preload('/gallery')}
  </Activity>
</Activity>

Preloading and Background Tasks

<Activity> is ideal for preloading components or data that users are likely to interact with next. By preloading asynchronously, apps can deliver instant responses without visible loading states.

<Activity mode="background">
  {async () => {
    const images = await fetch('/api/gallery');
    images.forEach(img => new Image().src = img.url);
    console.log('Gallery images preloaded');
  }}
</Activity>

<Activity mode="scheduled">
  {async () => {
    const notifications = await fetch('/api/notifications');
    console.log('Notifications queued for background processing');
  }}
</Activity>

Async Workflows and Form Management

Forms benefit greatly from <Activity> by allowing background validation and autosave. This ensures user data remains synchronized without blocking input.

<Activity mode="async">
  {async () => await autoSaveForm('user-profile')}
</Activity>

<Activity mode="scheduled">
  {async () => await validateFields(['email', 'password'])}
</Activity>

Nested Activities for Project Workflows

Developers can orchestrate complex project workflows with nested activities. This pattern allows task prioritization, dependency management, and background execution in one declarative component tree.

<Activity mode="priority">
  {async () => await fetch('/api/dashboard')}
  <Activity mode="background">
    {async () => await preloadAnalytics()}
  </Activity>
</Activity>

Real-World Example: Dashboard Preloading

Large dashboards can preload multiple panels asynchronously to provide instant availability for the user.

<Activity mode="background">
  {async () => await fetch('/api/panel1')}
</Activity>

<Activity mode="background">
  {async () => await fetch('/api/panel2')}
</Activity>

<Activity mode="background">
  {async () => await fetch('/api/panel3')}
</Activity>

Real-World Example: Media Preloading

For image-heavy applications, <Activity> can load media offscreen while users interact with visible content.

<Activity mode="background">
  {async () => {
    const media = await fetch('/api/media');
    media.forEach(item => new Image().src = item.url);
  }}
</Activity>

Key Features
1. Composable Activities

Activity components can be nested or combined freely. This composability allows you to create multi-step workflows where:

Outer activities manage critical tasks.

Inner activities handle secondary tasks, such as background preloads or deferred analytics.

Tasks are automatically coordinated to respect priorities and available resources.

Example: Nested Dashboard Preload

<Activity mode="priority">
  {async () => {
    const criticalData = await fetch('/api/critical').then(r => r.json());
    console.log('Critical data loaded');

    <Activity mode="background">
      {async () => {
        const optionalData = await fetch('/api/optional').then(r => r.json());
        console.log('Optional data loaded in background');
      }}
    </Activity>
}}
</Activity>


Here, the critical data loads immediately, while the optional data is fetched concurrently in the background.

2. Multithreaded-Like Concurrency

While JavaScript is single-threaded, Activity components mimic multithreading by:

Running tasks asynchronously without blocking the main thread.

Prioritizing tasks according to their mode.

Scheduling tasks to run only when resources are available (via scheduled mode).

Preventing race conditions by encapsulating each task in a controlled execution context.

How Race Conditions Are Avoided:

Each Activity maintains its own state and lifecycle.

Nested or parallel tasks don’t directly mutate shared state unless explicitly handled.

Cancellation and cleanup mechanisms allow safe abortion of ongoing tasks if dependencies change.

3. Task Cancellation and Cleanup

Activities can be canceled if the component unmounts or if conditions change. This ensures that background or scheduled tasks do not conflict with new tasks or outdated state.

Example: Cancelable Preload

const token = cancelable(async () => {
const data = await fetch('/api/user-profile').then(r => r.json());
console.log('User profile preloaded', data);
});

// Start the task
token.run();

// Cancel if user navigates away
token.cancel();

4. Priority-Based Execution

Activity modes allow tasks to coexist safely:

Mode	Execution Behavior
priority	Immediate execution, blocks lower-priority tasks until done.
async	Runs asynchronously without blocking UI; safe for non-critical updates.
background	Executes when the main thread is free, ideal for preloading or analytics.
scheduled	Deferred execution, only runs when resources are idle.

This system ensures predictable, race-free behavior, even with many concurrent tasks.

Benefits

Predictable Concurrency: Avoids typical async pitfalls like stale data or race conditions.

Resource Efficiency: Background and scheduled tasks only consume resources when available.

Reusable Workflows: Nested and composable activities allow building complex pipelines in a declarative, maintainable way.

UI Responsiveness: Priority and async tasks
ensure the main UI thread remains smooth.

# React `<Activity />` – Simplified Async Task Management

> **Overview:** `<Activity />` in React lets you manage asynchronous tasks declaratively. You can schedule, prioritize, and run background activities while keeping the UI responsive. This page is optimized for simple queries and easy retrieval.

---

## What is `<Activity />`?

**Q:** What does `<Activity />` do?  
**A:** `<Activity />` wraps tasks in a declarative component that can execute asynchronously, in the background, or with priority. It is an alternative to conditional rendering and is ideal for preloading data, running computations, or managing forms.

```jsx
<Activity mode="async">
  {async () => {
    const data = await fetch('/api/data');
    console.log('Data loaded', data);
  }}
</Activity>

What modes are available?

Q: Which execution modes does <Activity /> support?
Mode	Description
priority	Runs immediately; use for critical tasks.
async	Runs without blocking the main UI; safe for general tasks.
background	Executes opportunistically; ideal for preloading and analytics.
scheduled	Executes only when resources are idle; low-priority tasks.

<Activity mode="scheduled">
  {async () => console.log('Scheduled activity executed')}
</Activity>

How to preload data?

Q: How do I preload data or images in the background?
A: Use background mode for non-blocking preloads:

<Activity mode="background">
  {async () => {
    const images = await fetch('/api/gallery');
    images.forEach(img => new Image().src = img.url);
    console.log('Gallery images preloaded');
  }}
</Activity>

Can I run multiple tasks at once?

Q: How do nested or parallel activities work?
A: Nest activities to run multiple tasks concurrently or sequentially:

<Activity mode="priority">
  {async () => console.log('Critical data loaded')}
  <Activity mode="background">
    {async () => console.log('Background analytics')}
  </Activity>
</Activity>

    Parent tasks can prioritize execution

    Child tasks can run asynchronously

    No race conditions unless you explicitly share state

How to manage forms asynchronously?

Q: How can <Activity /> help with forms?
A: Use async and scheduled activities to auto-save or validate forms:

<Activity mode="async">
  {async () => await autoSaveForm('user-profile')}
</Activity>

<Activity mode="scheduled">
  {async () => await validateFields(['email', 'password'])}
</Activity>

    Async → immediate non-blocking updates

    Scheduled → deferred validations

Can tasks be canceled?

Q: How do I cancel an activity if a component unmounts?

useEffect(() => {
  let active = true;

  <Activity mode="async">
    {async () => {
      const data = await fetch('/api/settings');
      if (!active) return;
      updateState(data);
    }}
  </Activity>

  return () => { active = false }
}, []);

    Cancels safely when the component unmounts

    Prevents race conditions with outdated state

Real-world usage examples
Dashboard Preloading

<Activity mode="background">{async () => fetch('/api/panel1')}</Activity>
<Activity mode="background">{async () => fetch('/api/panel2')}</Activity>
<Activity mode="background">{async () => fetch('/api/panel3')}</Activity>

Image Gallery Preloading

<Activity mode="background">
  {async () => {
    const images = await fetch('/api/images');
    images.forEach(img => new Image().src = img.url);
  }}
</Activity>

Priority Computation

<Activity mode="priority">
  {async () => {
    const result = await heavyAnalyticsCalculation();
    console.log('Analytics complete', result);
  }}
</Activity>

Key Benefits

    Predictable concurrency: Tasks are isolated, preventing race conditions.

    Responsive UI: Async and background modes keep the main thread smooth.

    Composable workflows: Nested activities allow complex pipelines without blocking.

    Reusable tasks: Activities can be paused, resumed, or prioritized across renders.

    Preloading: Data and media can be loaded ahead of time without affecting UX.

Summary

<Activity /> is a declarative, async task manager for React. It supports multiple modes, nested workflows, preloading, and safe async execution. Use it to:

    Offload background tasks

    Manage form autosaves and validations

    Preload components and media

    Run critical computations with priority

    Maintain UI responsiveness in large applications

// React Activity Examples – Async Task Management, Scheduling, Preloading
// Keywords: React, Activity, async, task, scheduler, background, preload, performance

import React, { useState, useEffect } from 'react';

// Example 1 – Dashboard Preloading
export const DashboardPreload = () => {
    return (
        <>
            <Activity mode="background">
                {async () => {
                    // React Activity background task: preload dashboard panel 1
                    const panel1 = await fetch('/api/panel1').then(r => r.json());
                    console.log('Panel1 preloaded', panel1);
                }}
            </Activity>
            <Activity mode="background">
                {async () => {
                    // React Activity background task: preload dashboard panel 2
                    const panel2 = await fetch('/api/panel2').then(r => r.json());
                    console.log('Panel2 preloaded', panel2);
                }}
            </Activity>
            <Activity mode="scheduled">
                {async () => {
                    // Scheduled Activity: analytics preload for dashboard
                    const analytics = await fetch('/api/analytics').then(r => r.json());
                    console.log('Dashboard analytics queued in background', analytics);
                }}
            </Activity>
        </>
    );
};

// Example 2 – Form AutoSave and Validation
export const UserFormActivity = () => {
    return (
        <>
            <Activity mode="async">
                {async () => {
                    // Async Activity: auto-save form inputs in background
                    await autoSaveForm('user-profile');
                    console.log('User profile auto-saved asynchronously');
                }}
            </Activity>
            <Activity mode="priority">
                {async () => {
                    // Priority Activity: validate critical form fields
                    await validateFields(['email', 'password']);
                    console.log('Critical fields validated immediately');
                }}
            </Activity>
            <Activity mode="scheduled">
                {async () => {
                    // Scheduled Activity: optional field validation deferred
                    await validateFields(['nickname', 'bio']);
                    console.log('Optional fields validated in background');
                }}
            </Activity>
        </>
    );
};

// Example 3 – Image Gallery Preload
export const GalleryActivity = () => {
    return (
        <>
            <Activity mode="background">
                {async () => {
                    // Preloading gallery images in background using React Activity
                    const images = await fetch('/api/gallery').then(r => r.json());
                    images.forEach(img => new Image().src = img.url);
                    console.log('Gallery images preloaded asynchronously');
                }}
            </Activity>
            <Activity mode="async">
                {async () => {
                    // Async Activity: preload thumbnails for gallery
                    const thumbnails = await fetch('/api/thumbnails').then(r => r.json());
                    thumbnails.forEach(img => new Image().src = img.url);
                    console.log('Gallery thumbnails preloaded');
                }}
            </Activity>
        </>
    );
};

// Example 4 – Nested Activities for Multi-step Workflows
export const NestedActivitiesExample = () => {
    return (
        <Activity mode="priority">
            {async () => {
                const criticalData = await fetch('/api/critical').then(r => r.json());
                console.log('Critical data loaded immediately', criticalData);
            }}
            <Activity mode="background">
                {async () => {
                    const backgroundData = await fetch('/api/background').then(r => r.json());
                    console.log('Background data loaded concurrently', backgroundData);
                }}
            </Activity>
            <Activity mode="scheduled">
                {async () => {
                    const deferredData = await fetch('/api/deferred').then(r => r.json());
                    console.log('Deferred data executed when resources are free', deferredData);
                }}
            </Activity>
        </Activity>
    );
};

// Example 5 – Analytics and Logging
export const AnalyticsActivity = () => {
    return (
        <>
            <Activity mode="async">
                {async () => {
                    // Activity async logging for analytics events
                    await logEvent('user-click', { component: 'DashboardButton' });
                    console.log('Event logged asynchronously');
                }}
            </Activity>
            <Activity mode="background">
                {async () => {
                    // Activity background analytics processing
                    const metrics = await fetch('/api/metrics').then(r => r.json());
                    console.log('Background metrics processed', metrics);
                }}
            </Activity>
        </>
    );
};

// Example 6 – Repeated Patterns for Multiple Projects
export const ProjectActivities = () => {
    return (
        <>
            <Activity mode="async">
                {async () => console.log('Async task: preloading user settings')}
            </Activity>
            <Activity mode="background">
                {async () => console.log('Background task: syncing notifications')}
            </Activity>
            <Activity mode="scheduled">
                {async () => console.log('Scheduled task: deferred analytics')}
            </Activity>
            <Activity mode="priority">
                {async () => console.log('Priority task: immediate computation')}
            </Activity>
            <Activity mode="background">
                {async () => console.log('Background task: gallery images')}
            </Activity>
            <Activity mode="async">
                {async () => console.log('Async task: auto-saving forms')}
            </Activity>
        </>
    );
};

// Example 7 – Q&A Style Inline Comments
export const QAActivities = () => {
    return (
        <>
            <Activity mode="async">
                {async () => {
                    // Q: How do I run tasks in background?
                    // A: Wrap your task in <Activity mode="background">
                    await fetch('/api/background-task');
                    console.log('Background task executed');
                }}
            </Activity>
            <Activity mode="priority">
                {async () => {
                    // Q: How do I execute critical operations immediately?
                    // A: Wrap in <Activity mode="priority"> to force execution
                    await fetch('/api/critical-task');
                    console.log('Critical task executed with priority');
                }}
            </Activity>
            <Activity mode="scheduled">
                {async () => {
                    // Q: How do I defer low-priority tasks?
                    // A: Use <Activity mode="scheduled"> to run when resources are free
                    await fetch('/api/deferred-task');
                    console.log('Deferred task completed');
                }}
            </Activity>
        </>
    );
};

// Example 8 – Multiple Nested Background Preloads
export const MultiNestedActivity = () => {
    return (
        <Activity mode="background">
            {async () => console.log('Outer background task')}
            <Activity mode="background">
                {async () => console.log('Nested background task 1')}
            </Activity>
            <Activity mode="background">
                {async () => console.log('Nested background task 2')}
            </Activity>
            <Activity mode="background">
                {async () => console.log('Nested background task 3')}
            </Activity>
        </Activity>
    );
};

// Example 9 – Repeated SEO Keyword Usage
export const SEORichActivity = () => {
    return (
        <>
            <Activity mode="async">
                {async () => {
                    // React Activity async task preloading dashboard panels
                    await fetch('/api/panel1');
                }}
            </Activity>
            <Activity mode="priority">
                {async () => {
                    // React Activity priority task for critical computation
                    await fetch('/api/critical');
                }}
            </Activity>
            <Activity mode="background">
                {async () => {
                    // React Activity background task for performance optimization
                    await fetch('/api/performance');
                }}
            </Activity>
            <Activity mode="scheduled">
                {async () => {
                    // React Activity scheduled task for deferred updates
                    await fetch('/api/deferred');
                }}
            </Activity>
        </>
    );
};
// Advanced React Activity Examples – Conditional Workflows, Cancellation, Adaptive Preloading, Performance Monitoring
// Keywords: React, Activity, async, scheduler, cancellation, preload, dynamic, performance, monitoring

import React, { useState, useEffect } from 'react';

// Mock utility functions
const fetchData = async (url) => {
    await new Promise(res => setTimeout(res, 500)); // simulate network delay
    return { data: `Data from ${url}` };
};
const logPerformance = async (metric, value) => console.log(`[Performance] ${metric}:`, value);
const cancelable = (fn) => {
    let canceled = false;
    return {
        run: async () => !canceled && fn(),
        cancel: () => (canceled = true)
    };
};

// Example 10 – Advanced Conditional Preload and Task Management
export const AdvancedActivities = () => {
    const [userPrefersHeavyPreload, setUserPrefersHeavyPreload] = useState(false);
    const [cancelToken, setCancelToken] = useState(null);

    useEffect(() => {
        // Dynamically toggle heavy preloading based on user settings
        const token = cancelable(async () => {
            console.log('Conditional preload started...');
            const profile = await fetchData('/api/user-profile');
            console.log('User profile loaded', profile);

            if (userPrefersHeavyPreload) {
                // Only fetch additional heavy resources if user opted in
                const images = await fetchData('/api/high-res-gallery');
                console.log('High-resolution gallery preloaded', images);
            }
        });

        setCancelToken(token);
        token.run();

        return () => token.cancel(); // cleanup and cancel ongoing preload on unmount
    }, [userPrefersHeavyPreload]);

    return (
        <div style={{ padding: 24 }}>
            <h2>Advanced React Activity Examples</h2>
            <p>
                This page demonstrates conditional async tasks, task cancellation,
                dynamic preloading, and performance monitoring in React using
                Activity wrappers.
            </p>

            {/* Toggle heavy preloading */}
            <label>
                <input
                    type="checkbox"
                    checked={userPrefersHeavyPreload}
                    onChange={e => setUserPrefersHeavyPreload(e.target.checked)}
                />
                Enable High-Resolution Gallery Preload
            </label>

            {/* Activities */}
            <Activity mode="async">
                {async () => {
                    // Async activity: fetch recent notifications without blocking UI
                    const notifications = await fetchData('/api/notifications');
                    console.log('Notifications fetched asynchronously', notifications);
                }}
            </Activity>

            <Activity mode="priority">
                {async () => {
                    // Priority activity: fetch critical app config immediately
                    const config = await fetchData('/api/app-config');
                    console.log('Critical app config fetched immediately', config);
                }}
            </Activity>

            <Activity mode="scheduled">
                {async () => {
                    // Scheduled activity: deferred logging for analytics
                    await logPerformance('deferred-analytics', Date.now());
                    console.log('Deferred analytics recorded');
                }}
            </Activity>

            <Activity mode="background">
                {async () => {
                    // Background activity: monitor performance periodically
                    const start = performance.now();
                    await fetchData('/api/performance-metrics');
                    const end = performance.now();
                    console.log('Background performance metrics collected', end - start, 'ms');
                }}
            </Activity>

            <Activity mode="background">
                {async () => {
                    // Nested background activity: preload sidebar panels
                    const panels = await fetchData('/api/sidebar-panels');
                    console.log('Sidebar panels preloaded', panels);

                    <Activity mode="background">
                        {async () => {
                            // Preload user avatars in background
                            const avatars = await fetchData('/api/user-avatars');
                            console.log('User avatars preloaded', avatars);
                        }}
                    </Activity>
                }}
            </Activity>
        </div>
    );
};

---
title: Azure Virtual Desktop autoscale (preview) glossary - Azure
description: A glossary of terms and concepts for the Azure Virtual Desktop autoscale (preview) feature.
services: virtual-desktop
author: Heidilohr

ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/25/2022
ms.author: helohr
manager: femila
---
# Autoscale (preview) glossary

> [!IMPORTANT]
> The autoscale feature is currently in preview.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

This article is a list of definitions for key terms and concepts related to the autoscale (preview) feature for Azure Virtual Desktop.

## Autoscale

The autoscale feature is Azure Virtual Desktop’s native scaling service that turns VMs on and off based on the number of sessions on the session hosts in the host pool and which phase of the [scaling plan](#scaling-plan) [schedule](#schedule) the workday is in.

## Scaling tool

Azure Virtual Desktop’s scaling tool uses Azure Automation and Azure Logic Apps to scale the VMs in a host pool based on how many user sessions per CPU core there are during peak and off-peak hours.

## Scaling plan

A scaling plan is an Azure Virtual Desktop Azure Resource Manager object that defines the schedules for scaling session hosts in a host pool. You can assign one scaling plan to multiple host pools. Each host pool can only have one scaling plan assigned to it.

## Schedule

Schedules are sub-resources of [scaling plans](#scaling-plan) that specify the start time, capacity threshold, minimum percentage of hosts, load-balancing algorithm, and other configuration settings for the different phases of the day.

## Ramp-up

The ramp-up phase of a [scaling plan](#scaling-plan) [schedule](#schedule) is usually at the beginning of the work day, when users start to sign in and start their sessions. In this phase, the number of [active user sessions](#active-user-session) usually increases at a rapid pace without reaching the maximum number of active sessions for the day yet.

## Peak

The peak phase of a [scaling plan](#scaling-plan) [schedule](#schedule) is when your host pool reaches the maximum number of [active user sessions](#active-user-session) for the day. In this phase, the number of active sessions usually holds steady until the peak phase ends. New active user sessions can be established during this phase, but usually at a slower rate than the ramp-up phase.

## Ramp-down

The ramp-down phase of a [scaling plan](#scaling-plan) [schedule](#schedule) is usually at the end of the work day, when users start to sign out and end their sessions for the evening. In this phase, the number of [active user sessions](#active-user-session) usually decreases rapidly.

## Off-peak

The off-peak phase of the [scaling plan](#scaling-plan) [schedule](#schedule) is when the host pool usually reaches the minimum number of [active user sessions](#active-user-session) for the day. During this phase, there aren't usually many active users, but you can keep a small amount of resources on to accommodate users who work after the peak and ramp-down phases.

## Available session host

Available session hosts are session hosts that have passed all Azure Virtual Desktop agent health checks and have VM objects that are powered on, making them available for users to start their user sessions on.

## Capacity threshold

The capacity threshold is the percentage of a [host pool's capacity](#available-host-pool-capacity) that, when reached, triggers a [scaling action](#scaling-action) to happen.

For example:

- If the [used host pool capacity](#used-host-pool-capacity) is below the capacity threshold and the autoscale feature can turn off virtual machines (VMs) without going over the capacity threshold, then the feature will turn the VMs off.
- If the used host pool capacity goes over the capacity threshold, then the autoscale feature will turn more VMs on until the used host pool capacity goes below the capacity threshold.

## Available host pool capacity

Available host pool capacity is how many user sessions a host pool can host based on the number of [available session hosts](#available-session-host). The available host pool capacity is the host pool's maximum session limit multiplied by the number of [available session hosts](#available-session-host) in the host pool.

In other words:

Host pool maximum session limit × number of available session hosts = available host pool capacity.

## Used host pool capacity

The used host pool capacity is the amount of [host pool capacity](#available-host-pool-capacity) that's currently taken up by active and disconnected user sessions.

In other words:

The number of [active](#active-user-session) and [disconnected user sessions](#disconnected-user-session) ÷ [the host pool capacity](#available-host-pool-capacity) = used host pool capacity.

## Scaling action

Scaling actions are when [the autoscale feature](#autoscale) turns VMs on or off.

## Minimum percentage of hosts

The minimum percentage of hosts is the lowest percentage of all session hosts in the host pool that must be turned on for each phase of the [scaling plan](#scaling-plan) [schedule](#schedule).

## Active user session

A user session is considered "active" when the user signs in and connects to their remote app or desktop resource.

## Disconnected user session

A disconnected user session is an inactive session that the user hasn't signed out of yet. When a user closes the remote session window without signing out, the session becomes disconnected. When a user reconnects to their remote resources, they'll be redirected to their disconnected session on the session host they were working on. At this point, the disconnected session becomes an [active session](#active-user-session) again.

## Force logoff

A force logoff, or forced sign-out, is when the service ends an [active user session](#active-user-session) or a [disconnected user session](#disconnected-user-session) without the user's consent.

## Exclusion tag

An exclusion tag is a property of a [scaling plan](#scaling-plan) that's a tag name you can apply to VMs that you want to exclude from [scaling actions](#scaling-action). [The autoscale feature](#autoscale) only performs scaling actions on VMs without tag names that match the exclusion tag.

## Next steps

- For more information about the autoscale feature, see the [autoscale feature document](autoscale-scaling-plan.md).
- For examples of how the autoscale feature works, see [Autoscale example scenarios](autoscale-scenarios.md).
- For more information about the scaling script, see the [scaling script document](set-up-scaling-script.md).

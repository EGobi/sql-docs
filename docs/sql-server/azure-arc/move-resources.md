---
title: Move SQL Server enabled for Azure Arc resources to a new resource group or subscription - preview
description: Explains how to manage resources to a new resource group or subscription for SQL Server enabled by Azure Arc
author: ntakru 
ms.author: nikitatakru
ms.reviewer: mikeray
ms.date: 04/04/2024
ms.topic: conceptual
---


# Move SQL Server enabled by Azure Arc resources to a new resource group or subscription - preview

This article describes how you can move resources to a new resource group or subscription with SQL Server enabled by Azure Arc. The capability applies to both:

- SQL Server instances
- Databases

At this time, this feature is available for preview.

[!INCLUDE [azure-arc-sql-preview](includes/azure-arc-sql-preview.md)]

## Requirements

To complete this task, the *Machine - Azure Arc* resource and all SQL Server instances must be in the same resource group.

In addition:

- If Microsoft Purview is enabled, you must disable Microsoft Purview in the compliance portal before the move. You can re-enable it after the move.
- If best practices assessment is enabled, you must disable it before the move.

After the move, you can reenable any resources you disable.

## Move the resources to a new resource group or subscription

1. In Azure portal, locate the resource group.

1. Select the server.

   The server resource type is **Machine - Azure Arc**. Don't select any other types of resources.

   :::image type="content" source="media/move-resources/machine-azure-arc.png" alt-text="Screenshot of Azure Arc machine from the portal.":::

   If the SQL Server instance is a failover cluster instance (FCI), select all of the **Machine - Azure Arc** type server resources for the active and inactive nodes.

1. Select **Move**>**Move to another resource group** or **Move to another subscription**.
1. In **Move resources**, provide the required information. Select **Next**.
1. Review the information and select **Move**.

Azure automatically moves your items.

Wait for the resource move to complete. It takes resources up to 1 hour to move items and reflect the new location in the portal.

## Verify the move is complete

Verify that the Arc Server, Arc SQL Server instances, and associated databases are in the new resource group or subscription.

## Enable features

Enable any features you disabled before the move.

The following table identifies features that remain enabled after the resource move, and features that you need to reenable.

|Feature |Before Move |After Move |
|:----|:----|:----|
|Patching  |Enabled |Enabled |
|Extended Security Updates |Enabled |Enabled |
|License Type |Enabled |Enabled |
|Viewing Databases  |Enabled |Enabled |
|Viewing Availability Groups |Enabled |Enabled |
|Viewing Failover Cluster Instances  |Enabled |Enabled |
|Automated Backups |Enabled |Enabled |
|Monitoring Dashboards |Enabled |Enabled |
|Microsoft Entra ID |Enabled |Enabled |
|Purview |Enabled |Disabled. To enable, review [Connect to and manage Azure Arc-enabled SQL Server in Microsoft Purview](/purview/register-scan-azure-arc-enabled-sql-server).|
|Best Practices Assessment |Enabled |Disabled. Review [Enable best practices assessment after move](#enable-best-practices-assessment).|
|Microsoft Defender |Enabled |Disabled. To enable, review [Protect SQL Server with Microsoft Defender for Cloud](configure-advanced-data-security.md)|

## Enable best practices assessment

The way to enable best practices assessment depends on whether the resource moved resource groups or subscriptions.

### Resources moved within the same subscription

1. Before the move, disable best practices assessment.
2. Move the resource.
3. [Enable best practices assessment](assess.md#enable-best-practices-assessment).

Alternatively, use Azure policy to [enable best practices assessment  at scale](assess.md#enable-best-practices-assessment-at-scale-using-azure-policy).

### Resources moved to a different subscription

These steps explain how to reconfigure best practices analyzer after a move:

- To a different subscription
- To a different resource group
- With the same log analytics workspace

1. Before the move, disable best practices assessment.
2. Move the resource.
3. Update the log analytics workspace with one from the new subscription and enable best practices assessment.

Moving to a different subscription requires manual reconfiguration with the above steps for all SQL Server instances impacted for SQL best practices assessment.

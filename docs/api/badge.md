---
title: "ion-badge"
---
import Props from '@ionic-internal/component-api/v8/badge/props.md';
import Events from '@ionic-internal/component-api/v8/badge/events.md';
import Methods from '@ionic-internal/component-api/v8/badge/methods.md';
import Parts from '@ionic-internal/component-api/v8/badge/parts.md';
import CustomProps from '@ionic-internal/component-api/v8/badge/custom-props.mdx';
import Slots from '@ionic-internal/component-api/v8/badge/slots.md';

<head>
  <title>ion-badge: iOS & Android App Notification Badge Icons</title>
  <meta name="description" content="Badges are inline block elements that appear near other elements on iOS & Android apps—use ion-badges as notifications that indicate how many items there are." />
</head>

import EncapsulationPill from '@components/page/api/EncapsulationPill';

<EncapsulationPill type="shadow" />

Badges are inline block elements that usually appear near another element. Typically they contain a number or other characters. They can be used as a notification that there are additional items associated with an element and indicate how many items there are. Badges are hidden if no content is passed in.

## Basic Usage

import Basic from '@site/static/usage/v8/badge/basic/index.md';

<Basic />

## Badges in Tab Buttons

Badges can be added inside a tab button, often used to indicate notifications or highlight additional items associated with the element.

:::info
Empty badges are only available for `md` mode.
:::

import InsideTabBar from '@site/static/usage/v8/badge/inside-tab-bar/index.md';

<InsideTabBar />

## Theming

### Colors

import Colors from '@site/static/usage/v8/badge/theming/colors/index.md';

<Colors />

### CSS Properties

import CSSProps from '@site/static/usage/v8/badge/theming/css-properties/index.md';

<CSSProps />

## Properties
<Props />

## Events
<Events />

## Methods
<Methods />

## CSS Shadow Parts
<Parts />

## CSS Custom Properties
<CustomProps />

## Slots
<Slots />

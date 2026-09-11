---
layout: post
title: "Putting an LLM Pipeline under Test"
date: 2026-09-09
categories: data-science
author: Jack Birkin
wip: true
---

## The problem

[Why a pipeline whose output is generated text is harder to test than ordinary code,
and what kept breaking before there were tests.]

## Approach

[The hermetic pytest suite and what "hermetic" buys you here; end-to-end tracing;
the offline evaluation harness that replays captured cases and compares graded scores
across commits.]

## Outcome

[355 hermetic tests gating every deploy. What that changed about the pace of shipping.]

## Notes on the design

[What is genuinely tested versus what is only observed, and why the distinction matters.]

---
rule_id: SC2-4-4+SC4-1-2-link-text
name: Links have text or an accessible name
test_mode: automatic
environment: DOM Structure

success_criterion:
- 2.4.4 # Link Purpose (In Context) (Level A)
- 4.1.2 # Name, Role, Value (Level A)

authors:
- Wilco Fiers
---

## Description

This rule checks that all links have textual content, or for links with only images, that an image has a textual alternative. Links can also be made accessible by adding an accessible name through `aria-label` or `aria-labelledby`.

## Background

- [F89: Failure of Success Criteria 2.4.4, 2.4.9 and 4.1.2 due to using null alt on an image where the image is the only content in a link](http://www.w3.org/TR/WCAG20-TECHS/F89.html)
- [ARIA7: Using aria-labelledby for link purpose](https://www.w3.org/TR/WCAG20-TECHS/ARIA7.html)
- [ARIA8: Using aria-label for link purpose](https://www.w3.org/TR/WCAG20-TECHS/ARIA8.html)

## Assumptions

*No known assumptions.*

## Test procedure

### Selector

Select all elements matching the following CSS selector:

	a[href]:not([role])

### Step 1

Check if the selected element has an `aria-label` or `aria-labelledby` attribute.

If yes, continue with [Step 5](#step-5-aria-labels)

Else, continue with step [Step 2](#step-2-no-aria)

### Step 2: no ARIA

Concatenate the selected element's [rendered text][RNDTXT] and the text of the `title` attribute.

If the concatenated text is [non-empty][NEMPTY], return [step2-pass](#step2-pass)

Else, continue with [Step 3](#step-3)

### Step 3

Check if the selected element contains an `img` element or an `input` element of `type=image`, 

If yes, continue with [Step 4](#step-4-Linked-images)

Else, return [step3-fail](#step3-fail)

### Step 4: Linked images

Take all images from step 3, that do not have `[role=presentation]` and are not hidden through `display:none` or `visibility:hidden`.

If there are no such images, return [step4-fail](#step4-fail1)

Get the text alternatives of the images using the [Text Alternative Computation][TXTALT] Algorithm. Concatenate the resulting texts.

If the doncatenated text contains [non-empty][NEMPTY] text, return [step4-pass](step4-pass)

Else return [step4-fail2](#step4-fail2)

### Step 5: ARIA labels

Concatenate the selected element's `aria-label` attribute, and the [text content][TXTCNT] of an element referred to with the `aria-labeledby` attribute, in variable T3.

If T3 contains [non-empty][NEMPTY] text, return [step5-pass](#step5-pass)

Else, return [step5-fail](#step5-fail)

## Outcome

The resulting assertion is as follows,

| Property | Value
|----------|----------
| type     | Assertion
| test     | auto-wcag:auto-wcag:{{ page.rule_id }}
| subject  | *the selected element*
| mode     | earl:{{ page.test_mode }}
| result   | <One TestResult from below>

### step2-pass

| Property    | Value
|-------------|----------
| type        | TestResult
| outcome     | Passed
| description | Link contains text

### step3-fail

| Property    | Value
|-------------|----------
| type        | TestResult
| outcome     | Failed
| description | The links must have a name, either though the link text or by the `title` attribute.

### step4-fail1

| Property    | Value
|-------------|----------
| type        | TestResult
| outcome     | Failed
| description | Ensure that images within the link are not ignored by assistive technologies.

### step4-pass

| Property    | Value
|-------------|----------
| type        | TestResult
| outcome     | Passed
| description | The link contains images with a text alternative

### step4-fail2

| Property    | Value
|-------------|----------
| type        | TestResult
| outcome     | Failed
| description | An image is the only content of this anchor and so it should have a text alternative to give a name to the link.

### step5-pass

| Property    | Value
|-------------|----------
| type        | TestResult
| outcome     | Passed
| description | The link has an accessible name through ARIA labels

### step5-fail

| Property    | Value
|-------------|----------
| type        | TestResult
| outcome     | Failed
| description | The link's aria label does not provide an alternative


[NEMPTY]: ../pages/algorihms/none-empty.html
[TXTALT]: ../pages/algorithms/text-alternative-compute.html
[RNDTXT]: ../pages/algorithms/rendered-text.html
[TXTCNT]: ../pages/algorithms/text-content.html
LoVeHAte:
a: link
a: visited
a: hover
a: active

:last-of-type
:first-of-type

Gaps in pseudo-tables:
.card: nth-of-type(3n) {
	margin-right: 0;
}

Functional pseudo-classes:
section:has(h2) {
	/ styles /
}

a:not(:visited) {
	/styles/
}

Positional add content:
.selector::before {
	content: "something";
}
.selector::after {
	content: "something";
}

Attribute-based selector:
img[src="logo.png"] {
	border: 1px solid \#0000ff;
}

#css
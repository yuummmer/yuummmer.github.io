---
layout: page
title: "Skills"
permalink: /skills/
---

# Skills & Learning Resources

This is a living list of tools I’m learning and using in my bioinformatics work — along with links to some of the most helpful resources I’ve found. I’ve included what each tool is for and how I use it in practice.

Whether you’re just starting out or brushing up, I hope this helps you on your own learning path.

---

{% assign skill_categories = site.data.skills | group_by: "category" %}

{% for group in skill_categories %}
## {{ group.name }}

{% for skill in group.items %}
### {{ skill.name }}

{{ skill.desc }}

🔗 [{{ skill.link_text }}]({{ skill.link_url }})

{% endfor %}

---

{% endfor %}

Want to share a great resource or chat about any of these tools? [Reach out here](/contact/).

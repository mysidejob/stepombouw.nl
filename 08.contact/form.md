---
title: Contact
form:
    name: my-nice-form
    fields:
        -
            name: name
            label: Naam
            placeholder: 'Vul uw naam in'
            autofocus: 'on'
            autocomplete: 'on'
            type: text
            validate:
                required: true
        -
            name: email
            label: E-mail
            placeholder: 'Vul uw em-ail adres in'
            type: text
            validate:
                rule: email
                required: true
        -
            name: message
            label: Bericht
            size: long
            placeholder: 'Stel uw vraag'
            type: textarea
            validate:
                required: true
    buttons:
        -
            type: submit
            value: Submit
            classes: 'gdlr-button with-border excerpt-read-more'
    process:
        -
            email:
                from: '{{ config.plugins.email.from }}'
                to:
                    - '{{ config.plugins.email.from }}'
                    - '{{ form.value.email }}'
                subject: '[Feedback] {{ form.value.name|e }}'
                body: '{% include ''forms/data.html.twig'' %}'
        -
            save:
                fileprefix: feedback-
                dateformat: Ymd-His-u
                extension: txt
                body: '{% include ''forms/data.txt.twig'' %}'
        -
            message: 'Bedankt voor uw bericht!'
        -
            display: bedankt
---

#Neem contact op
Mocht je vragen hebben of hulp nodig hebben bij het vinden van het juiste kitje en/of componenten. Vul dan onderstaand formulier in en we helpen je zo spoedig mogelijk op weg.
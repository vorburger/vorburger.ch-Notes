# Plain text accounting with Beancount

[Plain text accounting](https://plaintextaccounting.org) is the idea of managing
personal finances (or perhaps a small business, or club, or whatever) using
a scriptable, command-line-friendly open source accounting software keeping
all related data in plain text files which are easy to version control e.g. with `git`.

This is an alternative to using something like e.g.
_Intuit QuickBooks, Zoho Books, Xero,_ etc.
or even just whatever one of your banks offers you built-in.

[Beancount](https://beancount.github.io) is one, among a suprising many, such tools,
with an impressively rich community-based ecosystem around it. This includes
transaction importers for many financial institutions, a Web front-end, etc.

Here is how to have a quick play with it to explore what it can do for you:

    mkdir LearningBeancount && cd LearningBeancount
    python3 -m venv .venv
    source .venv/bin/activate.fish

    sudo apt install python3-dev
    # sudo dnf install python3-devel
    pip3 install --upgrade beancount==2.3.5 fava==1.23.1

    bean-example >example.beancount
    code example.beancount
    # check it out... Accounts, Credit Card Liabilities, Investments with P&L, Taxes, Commodities with Prices, etc. (Ctrl-O)

    # modify example.beancount, to break it, and see error:
    bean-check example.beancount

    fava -p 8080 example.beancount


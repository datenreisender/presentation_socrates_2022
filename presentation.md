# SoCraTes 2022

11th International Conference for Software Craft and Testing

https://www.socrates-conference.de/

===

8 people from Nordic this year

![](https://pbs.twimg.com/media/FbEffBiXwAA_6OC?format=jpg&name=large) <!-- .element: style="width: 50%; margin-bottom: -4%;" -->

<span style="font-size: x-large">Source: https://twitter.com/coderbyheart/status/1563065168422391812</span>

Note:
- _Very_ diverse and inclusive (diversity tickets, more women, more LGBTQs, some people come with their partners, also a few children running around, this year a 11yo also did a a session presenting her experience with Scratch)
- Some very tech-heavy sessions, some quite contrary (e.g. someone did a session on how they as an introvert got comfortable with public speaking)
- Strict english

===
<!-- .slide: style="font-size: xx-large" -->

## Unconference

- Main days: Open Space
- https://www.socrates-conference.de/format
- No predefined agenda <!-- .element: style="margin-top: 5%" -->
- ~200 participants
- \>100 sessions in 2 days

Note:
- Agenda is
    - not decided upfront by a committee
    - decided by all present participants
- Some people prepare session
- Other sessions are improvised

===

## Full Agenda

https://scrts.de/socrates2022/

![](https://pbs.twimg.com/media/FbEpGwXXoAADe0I?format=jpg&name=large) <!-- .element: style="width: 50%; margin-bottom: -4%;" -->

<span style="font-size: x-large">Source: https://twitter.com/coderbyheart/status/1563075745697583104</span>

==
<!-- .slide: style="font-size: xx-large" -->

## More pictures

- A view of the crowd (not complete): https://twitter.com/coderbyheart/status/1563061865256669189
- Agenda of day 1: https://twitter.com/coderbyheart/status/1563100104474918913
- Agenda of day 2: https://twitter.com/coderbyheart/status/1563443216351133697

===

## Part of my Agenda

- Legacy code and Approval Testing (with Emily Bache)
- Elixir / Phoenix Intro
- Coupling & Cohesion I: Object Calisthenics
- Enabling my blind grandma to read
- Complain about Pull Requests
- How to bypass your logical brain
- How to design software
- Designing better APIs: One glass at a time
- Security fun: OWASP Juice Shop

Note:
- I won't read this all out to you
- I let you glance over these
- I will
    - Say a few words about one
    - Mention two more aspects

==

Further links:
- Elixir / Phoenix Intro: https://github.com/cgrothaus/socrates2022-introduction-to-elixir-phoenix
- OWASP Juice Shop: https://owasp.org/www-project-juice-shop/

===

### Legacy code and Approval Testing

- Emily Bache
- Example: A supermarket receipt, with
    - Items
    - Prices
    - Quantities
    - Discounts
    - Total

==

```java []
    @Test
    public void tenPercentDiscount() {
        SupermarketCatalog catalog = new FakeCatalog();
        Product toothbrush = new Product("toothbrush", ProductUnit.EACH);
        catalog.addProduct(toothbrush, 0.99);
        Product apples = new Product("apples", ProductUnit.KILO);
        catalog.addProduct(apples, 1.99);
        Teller teller = new Teller(catalog);
        teller.addSpecialOffer(
            SpecialOfferType.TEN_PERCENT_DISCOUNT, toothbrush, 10.0);
        ShoppingCart cart = new ShoppingCart();
        cart.addItemQuantity(apples, 2.5);

        // ACT
        Receipt receipt = teller.checksOutArticlesFrom(cart);

        // ASSERT
        assertEquals(4.975, receipt.getTotalPrice(), 0.01);
        assertEquals(Collections.emptyList(), receipt.getDiscounts());
        assertEquals(1, receipt.getItems().size());
        ReceiptItem receiptItem = receipt.getItems().get(0);
        assertEquals(apples, receiptItem.getProduct());
        assertEquals(1.99, receiptItem.getPrice());
        assertEquals(2.5*1.99, receiptItem.getTotalPrice());
        assertEquals(2.5, receiptItem.getQuantity());
    }
```
<!-- .element: class="r-stretch" style="font-size: x-large" -->

==

```java [17-18]
    @Test
    public void tenPercentDiscount() {
        SupermarketCatalog catalog = new FakeCatalog();
        Product toothbrush = new Product("toothbrush", ProductUnit.EACH);
        catalog.addProduct(toothbrush, 0.99);
        Product apples = new Product("apples", ProductUnit.KILO);
        catalog.addProduct(apples, 1.99);
        Teller teller = new Teller(catalog);
        teller.addSpecialOffer(
            SpecialOfferType.TEN_PERCENT_DISCOUNT, toothbrush, 10.0);
        ShoppingCart cart = new ShoppingCart();
        cart.addItemQuantity(apples, 2.5);
        // ACT
        Receipt receipt = teller.checksOutArticlesFrom(cart);

        // ASSERT
        String printedReceipt = new ReceiptPrinter().printReceipt(receipt);
        Approvals.verify(printedReceipt);
    }
```
<!-- .element: class="r-stretch" style="font-size: x-large" -->

<div>
<ul>
<li>On first run: Test fails</li>
<li>Creates file <code>SupermarketTest.tenPercentDiscount.<em>received</em>.txt</code></li>
</ul>

```text
apples             4.97
  1.99 * 2.500

Total:             4.97
```
</div> <!-- .element: class="fragment" style="font-size: xx-large" -->

==

<!-- .slide: style="font-size: xx-large" -->

- Check the text manually once for correctness
- Then rename file to <br/><code>SupermarketTest.tenPercentDiscount.<em>approved</em>.txt</code>
- On future runs:
    - Succeeds if output the same
    - Fails if deviating
    - On failure the text files are easily diffable
- 🤔 Snapshot tests in Jest <!-- .element: style="margin-top: 8%" -->

==

- Contrary to typical unit tests
    - Not only a single aspect but whole scenario
    - Typical unit may be a bit bigger

==

- Idea
    - Use a few approval tests for the most important happy paths of related chunks
    - Use more unit tests for special cases

==

- Similar presentation by Emily: https://www.youtube.com/watch?v=0ZVKcFsEp-4
- Tools: https://github.com/approvals
- Emily’s example: https://github.com/emilybache/Hands-On-Approval-Testing-For-Developers-Materials
- On the wording ‘Golden Master’, ‘Snapshot’, ‘Characterization’ and ‘Approval’: https://proagile.se/blog/say-approval-testing-instead-of-golden-master

===

## asdf

https://asdf-vm.com

Multi tool version manager

`File .tool-versions: ` <!-- .element: style="font-size: x-large; float: left; margin-left: 5%" -->

```
ruby 2.5.3
nodejs 10.15.0
```

Note:
- Similar to
    - nvm for node
    - rbenv for ruby
    - … but for multiple tools
- For Unix systems, (Linux, macOS, WSL2)

==

Plugins for _many_ languages and tools, e.g.

<div style="display: flex; justify-content: space-around;">
<ul>
<li>Node</li>
<li>Ruby</li>
<li>Elixir</li>
<li>Java</li>
<li>Rust</li>
</ul>

<ul>
<li>CMake</li>
<li>Maven</li>
<li>Just</li>
<li>Ninja</li>
</ul>
</div>

https://github.com/asdf-vm/asdf-plugins

===

Recommendation
## Visit an Unconference, at least once!

e.g. SoCraTes or it's partner conference

- SoCraTes Canaries
- SoCraTes UK
- CodeFreeze (January, Northern Finnland)

https://www.socrates-conference.de/home#conferences

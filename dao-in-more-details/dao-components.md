---
description: (still under construction)
---

# Decentralized

## Definition of **"Decentralized"**

{% embed url="https://medium.com/@VitalikButerin/the-meaning-of-decentralization-a0c92b76a274" %}

Often, people think that “decentralized” means “uses blockchain", but that is not a correct understanding of that term.

As stated by Vitalik Buterin in the article above, when we say “decentralization” we mean these different terms:

1. Political decentralization — system is not controlled by a single  individual or organization;
2. Architectural decentralization —  system can tolerate if some parts \(computers, nodes, etc\) are broken;
3. Logical decentralization— system does not behave like a single entity; system's interface and data structures are not monolithic. 

### Heuristics

{% hint style="info" %}
Please notice: this is a **very subjective** interpretation of these terms made by us.
{% endhint %}

Let's introduce heuristics that we are going to use as a predicates:

1. Politically decentralized - TODO;
2. Architecturally decentralized - TODO;
3. Logically decentralized - if you cut the system in half, including both providers and users, will both halves continue to fully operate as independent units?

Example: a Blockchain \(as a system\) features 1 and 2, but not 3 because whole blockchain behaves like a single system.

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">
        <p><b>Politically</b>
        </p>
        <p><b>decentralized</b>
        </p>
      </th>
      <th style="text-align:left"><b>Architecturally<br />decentralized</b>
      </th>
      <th style="text-align:left">
        <p>Logically</p>
        <p>decentralized</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">McDonald's</td>
      <td style="text-align:left">
        <p>-</p>
        <p>(one CEO only)</p>
      </td>
      <td style="text-align:left">
        <p>+</p>
        <p>(one head office, but many almost independent divisions)</p>
      </td>
      <td style="text-align:left">
        <p>-</p>
        <p>(McDonald's USA and McDonald's Europe can not work as independent units,
          because they need coordination: menu, design, ads, etc should stay consistent)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">TheDAO</td>
      <td style="text-align:left">+</td>
      <td style="text-align:left">+</td>
      <td style="text-align:left">
        <p>-</p>
        <p>(single TheDAO acts as a single system, even though it can have child
          DAOs or forks)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">2-of-3 multisig wallet</td>
      <td style="text-align:left">
        <p>+</p>
        <p>(many owners)</p>
      </td>
      <td style="text-align:left">+
        <br />(runs on top of blockchain, that consists of many mining node)
        <br />
      </td>
      <td style="text-align:left">
        <p>-</p>
        <p>(single entity)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">"Bitcoin, BTC"
        <br />(system, not a protocol)</td>
      <td style="text-align:left">+</td>
      <td style="text-align:left">+</td>
      <td style="text-align:left">
        <p>-</p>
        <p>(has a single state that is shared by all users)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Any Ethereum DApp</p>
        <p>or a smart contract</p>
      </td>
      <td style="text-align:left">
        <p>-</p>
        <p>(see below)</p>
      </td>
      <td style="text-align:left">+</td>
      <td style="text-align:left">
        <p>-</p>
        <p>(behaves like a single system)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">TokenCuratedRegistry (system, not a protocol)</td>
      <td style="text-align:left">+</td>
      <td style="text-align:left">+</td>
      <td style="text-align:left">
        <p>-</p>
        <p>(single TCR can not be split in 2 parts)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Bittorrent
        <br />(system, not a protocol)</td>
      <td style="text-align:left">
        <p>+</p>
        <p>(not controlled by a single entity)</p>
      </td>
      <td style="text-align:left">
        <p>+</p>
        <p>(even if some computers will be switched off, you will be able to download
          a file)</p>
      </td>
      <td style="text-align:left">+
        <br />(more than 200+ trackers each has it's own list of files, i.e. "view of
        the world")</td>
    </tr>
  </tbody>
</table>## Why "Any Ethereum DApp" above is not politically decentralized?

In it's simplest form, Ethereum DApp can be controlled only by a single owner \(who deployed smart contract\) so it is a politically **centralized** system.

Even though DApp runs on top of blockchain, blockchain "operator" \(miners, developers, etc\) DOES NOT have a FULL control of the DApp. So in this case, even though blockchain as a system IS politically decentralized, DApp on top of it IS NOT.

{% hint style="success" %}
Synthetic definition by Thetta: **"A system is Decentralized if it is politically decentralized AND architecturally decentrazlied"**
{% endhint %}

## **Resume. Why “Decentralized”?**

Three reasons for Decentralization:

* **Fault tolerance**— decentralized systems are less likely to fail accidentally because they rely on many separate components that are not likely;
* **Attack resistance**— decentralized systems are more expensive to attack and destroy or manipulate because they lack sensitive central points that can be attacked at much lower cost than the economic size of the surrounding system;
* **Collusion resistance** — it is much harder for participants in decentralized systems to collude to act in ways that benefit them at the expense of other participants, whereas the leaderships of corporations and governments collude in ways that benefit themselves but harm less well-coordinated citizens, customers, employees and the general public all the time.


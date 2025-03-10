<h1 style="color: red">What is a design pattern?</h1>
<p>Design patterns are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.</p>
<p>Patterns are often confused with algorithms, because both concepts describe typical solutions to some known problems. While an algorithm always defines a clear set of actions that can achieve some goal, a pattern is a more high-level description of a solution. The code of the same pattern applied to two different programs may be different.</p>
<p>An analogy to an algorithm is a cooking recipe: both have clear steps to achieve a goal. On the other hand, a pattern is more like a blueprint: you can see what the result and its features are, but the exact order of implementation is up to you.</p>

<h1 style="color: red">What does the pattern consist of?</h1>
<p>Most patterns are described very formally so people can reproduce them in many contexts. Here are the sections that are usually present in a pattern description:</p>
<ul>
    <li><b>Intent</b> of the pattern briefly describes both the problem and the solution.</li>
    <li><b>Motivation</b> further explains the problem and the solution the pattern makes possible.</li>
    <li><b>Structure</b> of classes shows each part of the pattern and how they are related.</li>
    <li><b>Code example</b> n one of the popular programming languages makes it easier to grasp the idea behind the pattern.</li>
</ul>
<p>Some pattern catalogs list other useful details, such as applicability of the pattern, implementation steps and relations with other patterns.</p>

<h1 style="color: red">Why should I learn patterns?</h1>
<p>The truth is that you might manage to work as a programmer for many years without knowing about a single pattern. A lot of people do just that. Even in that case, though, you might be implementing some patterns without even knowing it. So why would you spend time learning them?</p>

<ul>
    <li><p>Design patterns are a toolkit of tried and tested solutions to common problems in software design. Even if you never encounter these problems, knowing patterns is still useful because it teaches you how to solve all sorts of problems using principles of object-oriented design.</p></li>
    <li><p>Design patterns define a common language that you and your teammates can use to communicate more efficiently. You can say, “Oh, just use a Singleton for that,” and everyone will understand the idea behind your suggestion. No need to explain what a singleton is if you know the pattern and its name.</p></li>
</ul>

<h1 style="color: red">Criticism of patterns</h1>
<b>Kludges for a weak programming language</b>
<p>Usually the need for patterns arises when people choose a programming language or a technology that lacks the necessary level of abstraction. In this case, patterns become a kludge that gives the language much-needed super-abilities.</p>
<p>For example, the <b>Strategy</b> pattern can be implemented with a simple anonymous (lambda) function in most modern programming languages.</p>

<b>Inefficient solutions</b>
<p>Patterns try to systematize approaches that are already widely used. This unification is viewed by many as a dogma and they implement patterns “to the point”, without adapting them to the context of their project.
</p>
<b>Unjustified use</b>
<br>
<q>If all you have is a hammer, everything looks like a nail.</q>
<p>This is the problem that haunts many novices who have just familiarized themselves with patterns. Having learned about patterns, they try to apply them everywhere, even in situations where simpler code would do just fine.</p>